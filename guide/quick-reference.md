---
name: ADONIS Quick Command Reference
description: Quick reference for most commonly used ADONIS scripting commands with syntax and examples
type: project
---

## Essential ADONIS Commands

### Model Setup
```javascript
newmodel()                                    // Create new model
set("unit","stress-pa")                       // Set units (stress-pa, stress-kpa, stress-mpa)
set("gravity", 0, 9.81)                       // Set gravity vector
```

### Geometry
```javascript
rect("startPoint", x1, y1, "endPoint", x2, y2)
line("startPoint", x1, y1, "endPoint", x2, y2)
circle("centerPoint", cx, cy, "radius", r, "numSeg", n)
arc("startPoint", x1, y1, "midPoint", xm, ym, "endPoint", x2, y2, "numSeg", n)
crack("startPoint", x1, y1, "endPoint", x2, y2)
joint("startPoint", x1, y1, "endPoint", x2, y2, "id", id)
```

### Meshing
```javascript
discretize("maxedge", size)
segment("id", seg_id, "numedge", n)
gmsh("maxedge", size, "elemtype", "T3"|"Q4", "useNMD", "on"|"off")
```

### Materials
```javascript
// Create
material("create", "Mohr-Coulomb", "matid", id, "matname", name,
  "density", val, "shear", val, "bulk", val,
  "coh", val, "fric", val, "dil", val, "tens", val)

// Assign
material("assign", "matid", id)                          // Entire model
material("assign", "matid", id, "region", x, y)          // Specific region
```

### Boundary Conditions
```javascript
applybc("xfix"|"yfix"|"xyfix", "xlim", x1, x2, "ylim", y1, y2)
applybc("xvel"|"yvel", value, "xlim", x1, x2, "ylim", y1, y2)
applybc("sxx"|"syy"|"nstress", value, "xlim", x1, x2, "ylim", y1, y2)
```

### Initial Stresses
```javascript
initial("syy", value_at_surface, "yvar", gradient, "xlim", x1, x2, "ylim", y1, y2)
initial("sxx", value, "yvar", gradient, "xlim", x1, x2, "ylim", y1, y2)
initial("szz", value, "yvar", gradient, "xlim", x1, x2, "ylim", y1, y2)
initial("xydisp", 0)    // Reset displacements
```

### Excavation & Construction
```javascript
excavate("region", x, y)
excavate("region", x, y, "reset", "off")    // Temporary (for installing structures)
fill("region", x, y, "reset", "off")        // Backfill
```

### Structural Elements
```javascript
// Beam/Liner
structure("drawbeam", "beamid", id, "xlim", x1, x2, "ylim", y1, y2)
structure("drawliner", "beamid", id, "ifid1", iid1, "ifid2", iid2, "xlim", x1, x2, "ylim", y1, y2)
structure("material", "beamid", id, "area", A, "I", I_val, "ymod", E)

// Tieback
structure("drawtieback", "tieid", id,
  "frompoint", x1, y1, "topoint", x2, y2,
  "grouted", L, "segnum", n)
structure("material", "tieid", id,
  "area", A, "ymod", E, "kbond", k, "sbond", s)
```

### Solve
```javascript
solve()                                     // Static equilibrium
solve("elastic")                            // Elastic only (no failure)
solve("fos")                                // Factor of safety (SSR)
solve("relax", "relaxFactor", 0.6, "relaxStep", 250, "xlim", x1, x2, "ylim", y1, y2)
```

### Plot
```javascript
plot("contour", "totdisp"|"xdisp"|"ydisp"|"sxx"|"syy"|"sxy"|"szz"|"pp")
plot("struc", "beam"|"tieback", "moment"|"axialforce"|"shearforce")
plot("interf", "normalstress"|"shearstress")
```

### Low-Level Access
```javascript
// Get element data
elist = getelem("allid")
stress = getelem("stress", eid, gauss_point)    // Returns [sxx, syy, sxy, szz]
pp = getelem("pp", eid, gauss_point)

// Set element data
setelem("stress", eid, gauss_point, component, value)    // component: 1=sxx, 2=syy, 3=sxy, 4=szz
setelem("pp", eid, gauss_point, value)

// Get node data
nlist = getnode("allid")
disp = getnode("disp", nid)                   // Returns [xdisp, ydisp]

// Set node data
setnode("xforce"|"yforce", nid, value)
setnode("xvel"|"yvel", nid, value)
```

### Water Table
```javascript
watertable("add", "dens", 1000, "elev", elevation)
watertable("remove")
```

### File Operations
```javascript
script("call", "filename", "other_script.ajs")
exportmodel("image", "filename", "path/to/file.png")
```
