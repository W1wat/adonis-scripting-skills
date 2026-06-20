# Example: Basic Circular Tunnel

## User Prompt

```
สร้างโมเดลอุโมงค์วงกลม รัศมี 5 เมตร ที่ระดับความลึก 20 เมตร 
ดินเป็น sand ความหนาแน่น 1800 kg/m3, friction 35 องศา
```

**Translation**: "Create a circular tunnel model, radius 5m, at depth 20m. Soil is sand with density 1800 kg/m3, friction 35 degrees."

## Generated Script

```javascript
// Basic Circular Tunnel in Sand
// Coordinate convention: y=0 at surface, negative y = depth
// Generated for: Circular tunnel, r=5m, depth=20m, sand (density=1800, fric=35)
// Assumptions: 
//   - Domain: 100m wide, 50m deep (5x tunnel diameter clearance)
//   - Material: Mohr-Coulomb (default for soil with friction)
//   - K0 = 0.4 (typical for sand)
//   - No groundwater
//   - Single-stage excavation

newmodel()
set("unit","stress-pa")

// Domain: 100m wide, 50m deep
rect("startPoint",-50,-50,"endPoint",50,0)

// Tunnel: radius 5m, center at depth 20m
circle("centerPoint",0,-20,"radius",5,"numSeg",20)

// Mesh
discretize("maxedge",1.5)
gmsh("maxedge",1.5,"elemtype","T3","useNMD","on")

// Material: Sand (Mohr-Coulomb)
material("create","Mohr-Coulomb","matid",1,"matname","Sand",
  "density",1800,"shear",2e7,"bulk",4e7,
  "coh",0,"fric",35,"dil",0,"tens",0)
material("assign","matid",1)

// Boundary conditions
applybc("xfix","xlim",-50.1,-49.9,"ylim",-50.1,0.1)
applybc("xfix","xlim",49.9,50.1,"ylim",-50.1,0.1)
applybc("xyfix","xlim",-50.1,50.1,"ylim",-50.1,-49.9)

// Initial stress (K0 = 0.4 for sand)
set("gravity",0,9.81)
var gamma = 1800*9.81
var k0 = 0.4
initial("syy",0,"yvar",gamma,"xlim",-50,50,"ylim",-50,0)
initial("sxx",0,"yvar",k0*gamma,"xlim",-50,50,"ylim",-50,0)
initial("szz",0,"yvar",k0*gamma,"xlim",-50,50,"ylim",-50,0)

// Initial equilibrium
solve()

// Excavate tunnel
initial("xydisp",0)
excavate("region",0,-20)
solve()

// Plot results
plot("contour","totdisp")
plot("contour","syy")
plot("element","state")
```

## Review Notes

✅ **Validation Checklist Passed:**
- No placeholder values
- Coordinate convention A documented
- Material model matches analysis (Mohr-Coulomb for soil with friction)
- Domain size: 50m each side = 5x tunnel diameter (10m) ✓
- Initial equilibrium solved before excavation ✓
- Displacements reset before construction ✓
- Boundary conditions prevent rigid body motion ✓
- Plot commands show displacement, stress, and plastic state ✓

⚠️ **Assumptions Made:**
- Used typical shear/bulk modulus for sand (not specified by user)
- Used K0 = 0.4 (typical for sand, not specified)
- No groundwater (not mentioned by user)
- Single-stage excavation (not specified)

📝 **Suggested Modifications:**
- Add groundwater if water table is known
- Use P-Hardening model for more accurate sand behavior
- Add shotcrete liner if support is needed
- Refine mesh near tunnel (maxedge = 0.5-1.0)

---

## Typical Qualitative Checks

After running the script in ADONIS, verify these qualitative behaviors:

- **Settlement pattern**: Maximum settlement should concentrate above the tunnel crown
- **Plastic zone**: If present, should localize around the excavation boundary (not extend to domain edges)
- **Stress redistribution**: Vertical stress should show arching effect around the tunnel opening
- **Symmetry**: Results should be approximately symmetric about the tunnel centerline (x=0)
- **Convergence**: Unbalance force ratio should drop below equilibrium ratio limit

> **Note**: Numerical values depend on stiffness, K0, mesh density, constitutive model, and excavation method. Run ADONIS and check convergence to obtain actual results.
