---
name: ADONIS Example Templates
description: Complete example templates for common ADONIS use cases - tunnels, slopes, excavations, foundations
type: project
---

# ADONIS Script Templates

## Coordinate Conventions

This file uses TWO different coordinate conventions. **Always check which convention each template uses:**

### Convention A (Default - Tunnel, Excavation, Foundation)
- `y = 0` at ground surface
- Negative `y` for depth below surface
- Example: tunnel at 20m depth → center at `(0, -20)`
- Used by: Templates 1A, 1B, 2, 4, 5, 6, 7

### Convention B (Slope Stability Only)
- `y = 0` at bottom of model
- Positive `y` for height above bottom
- Example: 20m high slope → crest at `y = 20`
- Used by: Template 3 only

**Why two conventions?** Slope stability models typically show the slope rising from a base, making positive-y more intuitive. Tunnel/excavation models show depth below surface, making negative-y more intuitive.

---

## Template 1A: Simple Circular Tunnel (IsoElastic - Truly Elastic Analysis)

**Use when**: User specifically requests elastic analysis, or for preliminary stress distribution study without plastic failure.

```javascript
// Simple circular tunnel - IsoElastic (no plastic failure)
// Coordinate convention: y=0 at surface, negative y = depth
newmodel()
set("unit","stress-pa")

// Domain: 100m wide, 50m deep, tunnel at 20m depth
rect("startPoint",-50,-50,"endPoint",50,0)
circle("centerPoint",0,-20,"radius",5,"numSeg",20)

discretize("maxedge",1.5)
gmsh("maxedge",1.5,"elemtype","T3","useNMD","on")

// IsoElastic material - NO yield criterion, purely elastic behavior
material("create","IsoElastic","matid",1,"matname","Rock",
  "density",2400,"shear",2e9,"bulk",3.3e9)
material("assign","matid",1)

// Boundary conditions
applybc("xfix","xlim",-50.1,-49.9,"ylim",-50.1,0.1)
applybc("xfix","xlim",49.9,50.1,"ylim",-50.1,0.1)
applybc("xyfix","xlim",-50.1,50.1,"ylim",-50.1,-49.9)

// Initial stress (K0 = 0.5)
set("gravity",0,9.81)
var gamma = 2400*9.81
var k0 = 0.5
initial("syy",0,"yvar",gamma,"xlim",-50,50,"ylim",-50,0)
initial("sxx",0,"yvar",k0*gamma,"xlim",-50,50,"ylim",-50,0)
initial("szz",0,"yvar",k0*gamma,"xlim",-50,50,"ylim",-50,0)

solve()

// Excavate tunnel
initial("xydisp",0)
excavate("region",0,-20)
solve()

// Plot results
plot("contour","totdisp")
plot("contour","syy")
```

---

## Template 1B: Simple Circular Tunnel (Mohr-Coulomb - With Plastic Failure)

**Use when**: User wants realistic soil/rock behavior with shear failure, or doesn't specify elastic analysis.

```javascript
// Simple circular tunnel - Mohr-Coulomb (with plastic failure)
// Coordinate convention: y=0 at surface, negative y = depth
newmodel()
set("unit","stress-pa")

// Domain: 100m wide, 50m deep, tunnel at 20m depth
rect("startPoint",-50,-50,"endPoint",50,0)
circle("centerPoint",0,-20,"radius",5,"numSeg",20)

discretize("maxedge",1.5)
gmsh("maxedge",1.5,"elemtype","T3","useNMD","on")

// Mohr-Coulomb material - includes cohesion, friction, tension limit
material("create","Mohr-Coulomb","matid",1,"matname","Rock",
  "density",2400,"shear",2e9,"bulk",3.3e9,
  "coh",1e5,"fric",40,"dil",0,"tens",1e4)
material("assign","matid",1)

// Boundary conditions
applybc("xfix","xlim",-50.1,-49.9,"ylim",-50.1,0.1)
applybc("xfix","xlim",49.9,50.1,"ylim",-50.1,0.1)
applybc("xyfix","xlim",-50.1,50.1,"ylim",-50.1,-49.9)

// Initial stress (K0 = 0.5)
set("gravity",0,9.81)
var gamma = 2400*9.81
var k0 = 0.5
initial("syy",0,"yvar",gamma,"xlim",-50,50,"ylim",-50,0)
initial("sxx",0,"yvar",k0*gamma,"xlim",-50,50,"ylim",-50,0)
initial("szz",0,"yvar",k0*gamma,"xlim",-50,50,"ylim",-50,0)

solve()

// Excavate tunnel
initial("xydisp",0)
excavate("region",0,-20)
solve()

// Plot results
plot("contour","totdisp")
plot("contour","syy")
plot("element","state")  // Show plastic state (only meaningful for MC)
```

## Template 2: NATM Tunnel with Shotcrete (Sequential Excavation)

```javascript
// NATM tunnel with sequential excavation and shotcrete
newmodel()
set("unit","stress-pa")

// Domain
rect("startPoint",-60,-40,"endPoint",60,0)
line("startPoint",-60,-2,"endPoint",60,-2)

// Tunnel profile (horseshoe shape)
arc("startPoint",5,-15,"midPoint",0,-11,"endPoint",-5,-15,"numSeg",25)
arc("startPoint",-5,-15,"midPoint",0,-16,"endPoint",5,-15,"numSeg",20)

discretize("maxedge",1.0)
gmsh("maxedge",1.0,"elemtype","T3","useNMD","on")

material("create","Mohr-Coulomb","matid",1,"matname","Soil",
  "density",2200,"shear",4e7,"bulk",6.7e7,"coh",3.9e4,"fric",35,"dil",0,"tens",0)
material("assign","matid",1)

applybc("xfix","xlim",-60.1,-59.9,"ylim",-40.1,0.1)
applybc("xfix","xlim",59.9,60.1,"ylim",-40.1,0.1)
applybc("xyfix","xlim",-60.1,60.1,"ylim",-40.1,-39.9)

// Gravity stress
set("gravity",0,9.81)
var gamma = 2200*9.81
initial("syy",0,"yvar",gamma,"xlim",-60,60,"ylim",-40,0)
initial("sxx",0,"yvar",0.6*gamma,"xlim",-60,60,"ylim",-40,0)
initial("szz",0,"yvar",0.6*gamma,"xlim",-60,60,"ylim",-40,0)

solve()
initial("xydisp",0)

// Excavate top heading
excavate("region",0,-13)
solve("relax","relaxFactor",0.6,"relaxStep",250,"xlim",-8,8,"ylim",-20,-8)

// Install shotcrete
var beam_nu = 0.2
var beam_th = 0.15
var beam_area = beam_th * 1.0
var beam_I = (1.0*Math.pow(beam_th,3))/12.0
var beam_ymod = 20e9 / (1-Math.pow(beam_nu,2))
structure("drawbeam","beamid",1,"xlim",-6,6,"ylim",-16,-10)
structure("material","beamid",1,"area",beam_area,"I",beam_I,"ymod",beam_ymod)

solve()

// Excavate bench
excavate("region",0,-15)
solve()

plot("contour","ydisp")
plot("struc","beam","moment")
```

## Template 3: Slope Stability (Factor of Safety)

**Coordinate Convention B**: y=0 at model bottom, positive y = height above bottom. (Different from tunnel/excavation templates!)

```javascript
// Slope stability analysis - Factor of Safety (SSR method)
// COORDINATE CONVENTION B: y=0 at bottom, positive y = height
// Slope height: 20m, total model height: 40m
newmodel()
set("unit","stress-pa")

// Slope geometry (y=0 at bottom, y=40 at top)
line("startPoint",0,0,"endPoint",100,0)
line("startPoint",100,0,"endPoint",100,40)
line("startPoint",100,40,"endPoint",60,40)
line("startPoint",60,40,"endPoint",30,20)
line("startPoint",30,20,"endPoint",0,20)
line("startPoint",0,20,"endPoint",0,0)

discretize("maxedge",1.5)
gmsh("maxedge",1.5,"elemtype","Q4")

set("gravity",0,9.8)

material("create","Mohr-Coulomb","matid",1,"matname","Soil",
  "density",1900,"shear",2e7,"bulk",5e7,"coh",10000,"fric",25,"dil",0,"tens",0)
material("assign","matid",1)

applybc("xyfix","xlim",-0.1,0.1,"ylim",-0.1,20.1)
applybc("xyfix","xlim",99.9,100.1,"ylim",-0.1,40.1)
applybc("xyfix","xlim",-0.1,100.1,"ylim",-0.1,0.1)

solve("fos")
```

## Template 4: Deep Excavation with Diaphragm Wall and Tiebacks

```javascript
// Deep excavation with diaphragm wall and tiebacks
newmodel()
set("unit","stress-pa")

// Domain
rect("startPoint",0,0,"endPoint",80,-50)
line("startPoint",0,-5,"endPoint",80,-5)
line("startPoint",0,-15,"endPoint",80,-15)
line("startPoint",0,-25,"endPoint",80,-25)

discretize("maxedge",1.5)
gmsh("maxedge",1.5,"elemtype","T3","useNMD","on")

material("create","Mohr-Coulomb","matid",1,"matname","Clay",
  "density",1800,"shear",1.5e7,"bulk",3e7,"coh",15000,"fric",22,"dil",0,"tens",0)
material("assign","matid",1)

applybc("xfix","xlim",-0.1,0.1,"ylim",-50.1,0.1)
applybc("xfix","xlim",79.9,80.1,"ylim",-50.1,0.1)
applybc("xyfix","xlim",-0.1,80.1,"ylim",-50.1,-49.9)

set("gravity",0,9.81)
var gamma = 1800*9.81
initial("syy",0,"yvar",gamma,"xlim",0,80,"ylim",-50,0)
initial("sxx",0,"yvar",0.6*gamma,"xlim",0,80,"ylim",-50,0)
initial("szz",0,"yvar",0.6*gamma,"xlim",0,80,"ylim",-50,0)

solve()
initial("xydisp",0)

// Excavate to -5m
excavate("region",40,-3)
solve()

// Install diaphragm wall
structure("drawliner","beamid",1,"ifid1",1,"ifid2",2,
  "xlim",39.9,40.1,"ylim",-25.1,0.1)
structure("material","beamid",1,"area",0.8,"I",0.043,"ymod",3e10)
imaterial("assign","Mohr-Coulomb","ifid",1,"matname","Int1",
  "jkn",1e9,"jks",1e9,"friction",20,"cohesion",0)
imaterial("assign","Mohr-Coulomb","ifid",2,"matname","Int2",
  "jkn",1e9,"jks",1e9,"friction",20,"cohesion",0)

solve()

// Excavate to -10m
excavate("region",40,-8)
solve()

// Install first tieback
structure("drawtieback","tieid",1,"frompoint",40.0,-5.0,
  "topoint",60,-15,"grouted",0.5,"segnum",5)
structure("material","tieid",1,"area",0.0015,"ymod",2.1e11,
  "kbond",1e8,"sbond",1e8,"spacing",2.0)

solve()

// Excavate to -15m
excavate("region",40,-13)
solve()

// Install second tieback
structure("drawtieback","tieid",2,"frompoint",40.0,-10.0,
  "topoint",60,-20,"grouted",0.5,"segnum",5)
structure("material","tieid",2,"area",0.0015,"ymod",2.1e11,
  "kbond",1e8,"sbond",1e8,"spacing",2.0)

solve()

plot("contour","xdisp")
plot("struc","beam","moment")
plot("struc","tieback","axialforce")
```

## Template 5: Foundation on Layered Soil

```javascript
// Strip foundation on layered soil
newmodel()
set("unit","stress-pa")

// Domain
rect("startPoint",-30,-30,"endPoint",30,0)
line("startPoint",-30,-5,"endPoint",30,-5)
line("startPoint",-30,-15,"endPoint",30,-15)

discretize("maxedge",1.5)
gmsh("maxedge",1.5,"elemtype","Q4")

// Layer 1: Fill
material("create","Mohr-Coulomb","matid",1,"matname","Fill",
  "density",1800,"shear",1e7,"bulk",2e7,"coh",5000,"fric",28,"dil",0,"tens",0)

// Layer 2: Clay
material("create","Mohr-Coulomb","matid",2,"matname","Clay",
  "density",1900,"shear",2e7,"bulk",4e7,"coh",20000,"fric",22,"dil",0,"tens",0)

// Layer 3: Dense Sand
material("create","Mohr-Coulomb","matid",3,"matname","Sand",
  "density",2000,"shear",5e7,"bulk",1e8,"coh",0,"fric",38,"dil",5,"tens",0)

material("assign","matid",1,"region",0,-2)
material("assign","matid",2,"region",0,-10)
material("assign","matid",3,"region",0,-20)

applybc("xfix","xlim",-30.1,-29.9,"ylim",-30.1,0.1)
applybc("xfix","xlim",29.9,30.1,"ylim",-30.1,0.1)
applybc("xyfix","xlim",-30.1,30.1,"ylim",-30.1,-29.9)

set("gravity",0,9.81)

// Layered stress initialization
// Formula: modified_value = value + yvar * y  (per API reference)
// Convention A: y is negative below surface, so compression stress is negative
var g1 = 1800*9.81
var g2 = 1900*9.81
var g3 = 2000*9.81
var k0 = 0.5

// Offsets ensure stress continuity at layer boundaries
var off2 = 5*(g2 - g1)       // At y=-5: off2 + g2*(-5) = -g1*5 (matches layer 1 bottom)
var off3 = 15*g3 - 5*g1 - 10*g2  // At y=-15: off3 + g3*(-15) = -g1*5 - g2*10

initial("syy",0,"yvar",g1,"xlim",-30,30,"ylim",-5,0)
initial("syy",off2,"yvar",g2,"xlim",-30,30,"ylim",-15,-5)
initial("syy",off3,"yvar",g3,"xlim",-30,30,"ylim",-30,-15)

initial("sxx",0,"yvar",k0*g1,"xlim",-30,30,"ylim",-5,0)
initial("sxx",k0*off2,"yvar",k0*g2,"xlim",-30,30,"ylim",-15,-5)
initial("sxx",k0*off3,"yvar",k0*g3,"xlim",-30,30,"ylim",-30,-15)

initial("szz",0,"yvar",k0*g1,"xlim",-30,30,"ylim",-5,0)
initial("szz",k0*off2,"yvar",k0*g2,"xlim",-30,30,"ylim",-15,-5)
initial("szz",k0*off3,"yvar",k0*g3,"xlim",-30,30,"ylim",-30,-15)

// Apply foundation load
applybc("syy",-100000,"xlim",-2,2,"ylim",-0.1,0.1)

solve()

plot("contour","ydisp")
plot("contour","syy")
```

## Template 6: Anchored Sheet Pile Wall

```javascript
// Sheet pile wall with tiebacks
newmodel()
set("unit","stress-pa")

// Domain
rect("startPoint",0,-18,"endPoint",30,0)
line("startPoint",0,-4,"endPoint",30,-4)
line("startPoint",0,-10,"endPoint",10,-10)
line("startPoint",0,-8,"endPoint",10,-8)
crack("startPoint",10,-10,"endPoint",10,0)

discretize("maxedge",0.5)
gmsh("maxedge",0.5,"elemtype","Q4")

material("create","Mohr-Coulomb","matid",1,"matname","Sand",
  "density",1735,"shear",8e6,"bulk",1.33e7,"coh",1000,"fric",35,"dil",0,"tens",0)
material("create","Mohr-Coulomb","matid",2,"matname","Clay",
  "density",2041,"shear",6e6,"bulk",1e7,"coh",10000,"fric",25,"dil",0,"tens",0)

material("assign","matid",1,"region",28,-2)
material("assign","matid",1,"region",9,-1.5)
material("assign","matid",2,"region",7,-7)
material("assign","matid",2,"region",9,-9)

applybc("xfix","xlim",-0.1,0.1,"ylim",-18.1,0.1)
applybc("xfix","xlim",29.9,30.1,"ylim",-18.1,0.1)
applybc("xyfix","xlim",-0.1,30.1,"ylim",-18.1,-17.9)

// Temporary excavation for wall installation
excavate("region",3,-2,"reset","off")
excavate("region",5,-6,"reset","off")

// Install sheet pile
structure("drawliner","beamid",1,"ifid1",1,"ifid2",2,
  "xlim",9.9,10.1,"ylim",-10.1,0.1)
structure("material","beamid",1,"area",0.2,"I",6.7e-4,"ymod",3.125e10)
imaterial("assign","Mohr-Coulomb","ifid",1,"matname","Int1",
  "jkn",1e8,"jks",1e7,"friction",20)
imaterial("assign","Mohr-Coulomb","ifid",2,"matname","Int2",
  "jkn",1e8,"jks",1e7,"friction",20)

// Backfill
fill("region",3,-2,"reset","off")
fill("region",5,-6,"reset","off")

// Initial stress
initial("syy",0,"yvar",17003,"xlim",0,30,"ylim",-4,0)
initial("syy",11956,"yvar",19992,"xlim",0,30,"ylim",-18,-4)
initial("sxx",0,"yvar",8501.5,"xlim",0,30,"ylim",-4,0)
initial("sxx",5978,"yvar",9996,"xlim",0,30,"ylim",-18,-4)
initial("szz",0,"yvar",8501.5,"xlim",0,30,"ylim",-4,0)
initial("szz",5978,"yvar",9996,"xlim",0,30,"ylim",-18,-4)

set("gravity",0,9.8)
solve()
initial("xydisp",0)

// Excavate and install tiebacks
excavate("region",6.5,-2.5)
structure("drawtieback","tieid",1,"frompoint",10.0,-1.0,
  "topoint",18,-4,"grouted",0.4,"segnum",5)
structure("material","tieid",1,"area",0.002,"ymod",2e11,"kbond",1e7,"sbond",5e6)
solve()

excavate("region",6.5,-6.0)
structure("drawtieback","tieid",2,"frompoint",10.0,-5.0,
  "topoint",18,-8,"grouted",0.4,"segnum",5)
structure("material","tieid",2,"area",0.002,"ymod",2e11,"kbond",1e7,"sbond",5e6)
solve()

plot("contour","xdisp")
plot("struc","tieback","axialforce")
```

## Template 7: P-Hardening Model (EXPERIMENTAL)

> **WARNING**: This template is EXPERIMENTAL. The P-Hardening model requires initial principal effective stresses (`sig1`, `sig2`, `sig3`) to be set. Representative values are provided in the `material("create",...)` call below, but you MUST verify the correct convention and order of principal stresses in ADONIS Help → Scripting Language before using this template for any analysis. The commented pseudocode below shows a possible depth-dependent sig1/sig2/sig3 concept, but `setelem("prop",...)` is NOT confirmed in the API reference. Do not uncomment it unless verified in ADONIS Help / official manual.

```javascript
// Deep excavation with P-Hardening model (EXPERIMENTAL)
// COORDINATE CONVENTION A: y=0 at surface, negative y = depth
newmodel()
set("unit","stress-pa")

rect("startPoint",0,0,"endPoint",60,-40)
line("startPoint",0,-5,"endPoint",60,-5)
line("startPoint",0,-15,"endPoint",60,-15)

discretize("maxedge",1.5)
gmsh("maxedge",1.5,"elemtype","T3","useNMD","on")

// P-Hardening material
// Representative initial principal stresses included to satisfy required parameters.
// VERIFY: Check ADONIS Help for correct sig1/sig2/sig3 convention and ordering.
// These values represent approximate K0 stress at mid-depth (~20m).
material("create","P-Hardening","matid",1,"matname","Clay",
  "density",1800,
  "E50_ref",3e7,
  "Eur_ref",1.2e8,
  "p_ref",100000,
  "m",0.7,
  "ocr",1.5,
  "Eoed_ref",3e7,
  "cohesion",15000,
  "friction",25,
  "dilation",0,
  "sig1",-60000,
  "sig2",-60000,
  "sig3",-100000)
material("assign","matid",1)

applybc("xfix","xlim",-0.1,0.1,"ylim",-40.1,0.1)
applybc("xfix","xlim",59.9,60.1,"ylim",-40.1,0.1)
applybc("xyfix","xlim",-0.1,60.1,"ylim",-40.1,-39.9)

set("gravity",0,9.81)
var gamma = 1800*9.81
initial("syy",0,"yvar",gamma,"xlim",0,60,"ylim",-40,0)
initial("sxx",0,"yvar",0.6*gamma,"xlim",0,60,"ylim",-40,0)
initial("szz",0,"yvar",0.6*gamma,"xlim",0,60,"ylim",-40,0)

// EXPERIMENTAL / NOT VERIFIED:
// The following concept may be used only after confirming setelem("prop",...) support in ADONIS.
// API reference only documents setelem("stress",...) and setelem("pp",...).
// var elist = getelem("allid")
// for (i = 0; i < elist.length; i++) {
//   var eid = elist[i]
//   var gp = getelem("gausspointpos",eid,1)
//   var depth = -gp[1]
//   var sv = gamma * depth
//   var sh = 0.6 * gamma * depth
//   // setelem("prop","sig1",eid,1,-sh)
//   // setelem("prop","sig2",eid,1,-sh)
//   // setelem("prop","sig3",eid,1,-sv)
// }

solve()
initial("xydisp",0)

excavate("region",30,-3)
solve()

plot("contour","ydisp")
```
