---
name: ADONIS Script Generation Guide
description: Guidelines for generating ADONIS FEM scripts from natural language - covers workflow, patterns, and best practices
type: project
---

## ADONIS Script Generation Workflow

### 1. Understand User Requirements
Extract these parameters from natural language:
- **Geometry**: tunnel shape/size, depth, domain dimensions, slope geometry, excavation shape
- **Soil/Rock layers**: material types, thicknesses, properties (or use typical values)
- **Groundwater**: water table depth
- **Construction stages**: sequential excavation? support installation?
- **Analysis type**: static, FOS (slope stability), tunnel excavation, deep excavation
- **Output**: what to plot (displacements, stresses, structural forces)

### 2. Script Structure (Always Follow This Order)

```javascript
// 1. Initialize
newmodel()
set("unit","stress-pa")  // or other unit system

// 2. Create Geometry
rect(), line(), circle(), arc(), crack(), joint()

// 3. Discretize & Mesh
discretize("maxedge", size)
gmsh("maxedge", size, "elemtype", "T3"|"Q4", "useNMD", "on")

// 4. Create Materials
material("create", model_type, "matid", id, "matname", name, props...)

// 5. Assign Materials
material("assign", "matid", id, "region", x, y)

// 6. Apply Boundary Conditions
applybc("xfix"|"yfix"|"xyfix", "xlim", x1, x2, "ylim", y1, y2)

// 7. Set Initial Stresses (if gravity loading)
set("gravity", 0, 9.81)
initial("syy", value, "yvar", gradient, "xlim", x1, x2, "ylim", y1, y2)
initial("sxx", k0*value, "yvar", k0*gradient, "xlim", x1, x2, "ylim", y1, y2)
initial("szz", k0*value, "yvar", k0*gradient, "xlim", x1, x2, "ylim", y1, y2)

// 8. Optional: Water Table
watertable("add", "dens", 1000, "elev", elevation)

// 9. Solve (Initial Equilibrium)
solve()

// 10. Construction Stages (if applicable)
initial("xydisp", 0)  // Reset displacements
excavate("region", x, y)
solve("relax", "relaxFactor", 0.6, "relaxStep", 250, "xlim", x1, x2, "ylim", y1, y2)
structure("drawbeam"|"drawliner"|"drawtieback", ...)
structure("material", ...)
solve()

// 11. Plot Results
plot("contour", "totdisp"|"ydisp"|"syy"|...)
plot("struc", "beam", "moment"|"axialforce")
tab("plot")  // New plot tab
```

### 3. Common Patterns

#### Pattern A: Simple Tunnel (Circular)
```javascript
newmodel()
set("unit","stress-pa")

// Domain: surface at y=0, tunnel at depth
rect("startPoint",-50,-50,"endPoint",50,0)
circle("centerPoint",0,-20,"radius",5,"numSeg",20)

discretize("maxedge",1.5)
gmsh("maxedge",1.5,"elemtype","T3","useNMD","on")

material("create","Mohr-Coulomb","matid",1,"matname","Soil",
  "density",2000,"shear",5e7,"bulk",8.3e7,"coh",20000,"fric",30,"dil",0,"tens",0)
material("assign","matid",1)

applybc("xfix","xlim",-50.1,-49.9,"ylim",-50.1,0.1)
applybc("xfix","xlim",49.9,50.1,"ylim",-50.1,0.1)
applybc("xyfix","xlim",-50.1,50.1,"ylim",-50.1,-49.9)

set("gravity",0,9.81)
var gamma = 2000*9.81
var k0 = 0.5
initial("syy",0,"yvar",gamma,"xlim",-50,50,"ylim",-50,0)
initial("sxx",0,"yvar",k0*gamma,"xlim",-50,50,"ylim",-50,0)
initial("szz",0,"yvar",k0*gamma,"xlim",-50,50,"ylim",-50,0)

solve()
initial("xydisp",0)
excavate("region",0,-20)
solve()

plot("contour","totdisp")
```

#### Pattern B: Slope Stability (FOS)
```javascript
newmodel()
set("unit","stress-pa")

// Slope geometry
line("startPoint",0,0,"endPoint",100,0)
line("startPoint",100,0,"endPoint",100,40)
line("startPoint",100,40,"endPoint",60,40)
line("startPoint",60,40,"endPoint",30,20)
line("startPoint",30,20,"endPoint",0,20)
line("startPoint",0,20,"endPoint",0,0)

discretize("maxedge",1.0)
gmsh("maxedge",1.0,"elemtype","Q4")

set("gravity",0,9.8)

material("create","Mohr-Coulomb","matid",1,"matname","Soil",
  "density",1900,"shear",2e7,"bulk",5e7,"coh",10000,"fric",25,"dil",0,"tens",0)
material("assign","matid",1)

applybc("xyfix","xlim",-0.1,0.1,"ylim",-0.1,20.1)
applybc("xyfix","xlim",99.9,100.1,"ylim",-0.1,40.1)
applybc("xyfix","xlim",-0.1,100.1,"ylim",-0.1,0.1)

solve("fos")
```

#### Pattern C: Deep Excavation with Support
```javascript
newmodel()
set("unit","stress-pa")

// Domain
rect("startPoint",0,0,"endPoint",60,-40)
line("startPoint",0,-5,"endPoint",60,-5)
line("startPoint",0,-15,"endPoint",60,-15)

discretize("maxedge",1.0)
gmsh("maxedge",1.0,"elemtype","T3","useNMD","on")

material("create","Mohr-Coulomb","matid",1,"matname","Soil",
  "density",1800,"shear",1e7,"bulk",2e7,"coh",5000,"fric",28,"dil",0,"tens",0)
material("assign","matid",1)

applybc("xfix","xlim",-0.1,0.1,"ylim",-40.1,0.1)
applybc("xfix","xlim",59.9,60.1,"ylim",-40.1,0.1)
applybc("xyfix","xlim",-0.1,60.1,"ylim",-40.1,-39.9)

set("gravity",0,9.81)
var gamma = 1800*9.81
initial("syy",0,"yvar",gamma,"xlim",0,60,"ylim",-40,0)
initial("sxx",0,"yvar",0.5*gamma,"xlim",0,60,"ylim",-40,0)
initial("szz",0,"yvar",0.5*gamma,"xlim",0,60,"ylim",-40,0)

solve()
initial("xydisp",0)

// Excavate
excavate("region",30,-3)
solve()

// Install wall
structure("drawliner","beamid",1,"iftype","bothSides","ifid1",1,"ifid2",2,
  "xlim",29.9,30.1,"ylim",-15.1,0.1)
structure("material","beamid",1,"area",0.5,"I",0.01,"ymod",3e10)
imaterial("assign","Mohr-Coulomb","ifid",1,"matname","Int1","jkn",1e9,"jks",1e9,"friction",25)
imaterial("assign","Mohr-Coulomb","ifid",2,"matname","Int2","jkn",1e9,"jks",1e9,"friction",25)

solve()

// Install tieback
structure("drawtieback","tieid",1,"fromstrucnodeatpoint",30.0,-3.0,
  "topoint",45,-10,"pretens",100000,"grouted",0.5,"segnum",5)
structure("material","tieid",1,"area",0.001,"ymod",2e11,"kbond",1e8,"sbond",1e8)

solve()

plot("contour","xdisp")
plot("struc","beam","moment")
```

### 4. Material Property Guidelines

When user doesn't specify properties, use these typical values:

| Material | density (kg/m³) | shear (Pa) | bulk (Pa) | coh (Pa) | fric (°) | dil (°) | tens (Pa) |
|----------|----------------|------------|-----------|----------|----------|---------|-----------|
| Soft Clay | 1600-1800 | 1e6-5e6 | 2e6-1e7 | 5000-20000 | 15-25 | 0 | 0 |
| Stiff Clay | 1800-2000 | 1e7-5e7 | 2e7-1e8 | 20000-100000 | 20-30 | 0-5 | 0-10000 |
| Loose Sand | 1600-1800 | 1e7-3e7 | 2e7-6e7 | 0-5000 | 28-35 | 0 | 0 |
| Dense Sand | 1800-2000 | 3e7-8e7 | 6e7-1.6e8 | 0-10000 | 35-45 | 5-15 | 0 |
| Rock | 2400-2700 | 1e9-1e10 | 2e9-2e10 | 1e5-1e7 | 35-50 | 0-10 | 1e4-1e6 |

**K0 values (lateral earth pressure coefficient):**
- Normally consolidated clay: 0.5-0.7
- Overconsolidated clay: 0.7-1.5
- Sand: 0.3-0.5

### 5. Important Notes

1. **Coordinate system**: y=0 is ground surface, negative y is depth
2. **Stress sign convention**: Compression is negative
3. **excavate("region", x, y)**: Point (x,y) must be inside the region to excavate
4. **solve("relax")**: Used for tunnel excavation to simulate stress relaxation
5. **initial("xydisp", 0)**: Reset displacements before construction stages
6. **T3 vs Q4**: T3 (triangle) with NMD is good for complex geometry; Q4 (quad) is more accurate
7. **NMD**: Nodal Mixed Discretization prevents volumetric locking in T3 elements

### 6. Validation Checklist

Before delivering script:
- [ ] All coordinates are consistent (y=0 at surface, negative below)
- [ ] Boundary conditions are properly applied (no rigid body motion)
- [ ] Material properties are reasonable for the material type
- [ ] Initial stresses match gravity loading (k0 method)
- [ ] Mesh size is appropriate (smaller near excavations/structures)
- [ ] Construction sequence is logical (excavate → install support → solve)
- [ ] Plot commands show relevant results
