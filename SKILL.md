---
name: ADONIS FEM Script Generation
description: Generate ADONIS FEM geotechnical scripts from natural language. Covers tunnel, slope, excavation, foundation, and sheet pile analysis.
---

# ADONIS FEM Script Generation Skill

## When to Use This Skill

Use this skill when the user asks to:
- Create ADONIS scripts (`.ajs` files) for geotechnical analysis
- Generate finite element models for tunnels, slopes, excavations, or foundations
- Convert natural language descriptions into ADONIS scripting commands
- Build geotechnical FEM models with specific material models, boundary conditions, or construction stages

**Do NOT use this skill for:**
- HYRCAN slope stability scripts (different program)
- 3D FEM modeling (ADONIS is 2D plane strain)
- Dynamic/seismic analysis (requires additional verification)
- User-defined constitutive model (UDCM) C++ development

## Required Files

Read these files IN ORDER before generating any script:

1. **`reference/ADONIS_SCRIPTING_API_REFERENCE.md`** — Complete API reference (1,808 lines)
   - All commands, parameters, material models
   - Element/node getter/setter reference
   - Python API reference

2. **`guide/script-generation-workflow.md`** — Generation workflow and patterns
   - Step-by-step script structure
   - Common patterns (tunnel, slope, excavation)
   - Material property guidelines

3. **`guide/validation-checklist.md`** — Mandatory validation before delivery
   - Deterministic checks (units, sign convention, domain size)
   - Engineering sanity checks
   - Script completeness verification

4. **`templates/`** — Ready-to-use templates
   - 7 complete templates for common use cases

5. **`guide/limitations-and-disclaimer.md`** — Known limitations and engineering disclaimer
   - 2D plane strain assumption
   - Material model limitations
   - Not for design verification without site-specific parameters

## Workflow

### Step 1: Extract Requirements

From the user's natural language, extract:
- **Geometry**: shape, dimensions, depth, domain size
- **Material**: type (clay, sand, rock), layers, properties (or use typical values with disclaimer)
- **Groundwater**: water table depth (if mentioned)
- **Construction stages**: sequential excavation? support installation?
- **Analysis type**: static, FOS (slope stability), tunnel excavation, deep excavation
- **Output**: what to plot (displacements, stresses, structural forces)

### Step 2: Ask Before Generate (If Information Missing)

**MUST ask the user** if any of these are unclear:
- [ ] Domain size (width and depth of model)
- [ ] Material model preference (Mohr-Coulomb, P-Hardening, etc.)
- [ ] Groundwater conditions (water table depth)
- [ ] Construction sequence (single-stage vs multi-stage)
- [ ] Unit system (SI Pa, kPa, MPa, or Imperial)
- [ ] Boundary condition preferences

**MAY use defaults** (with explicit disclaimer) for:
- Material properties (use typical values from guide)
- K0 values (use standard ranges)
- Mesh size (use rule of thumb: domain/20 to domain/50)
- Relaxation factor for tunnel (default 0.6)

### Step 3: Select Template

Match user request to closest template:
- Circular tunnel → `templates/tunnel-circular-mohr-coulomb.ajs.md`
- NATM tunnel with shotcrete → `templates/tunnel-natm-shotcrete.ajs.md`
- Slope stability → `templates/slope-fos.ajs.md`
- Deep excavation with support → `templates/deep-excavation-tieback.ajs.md`
- Foundation on layered soil → `templates/foundation-layered-soil.ajs.md`
- Sheet pile wall → `templates/sheet-pile-wall.ajs.md` (if available)
- P-Hardening model → `templates/p-hardening-model.ajs.md` (if available)

If no template matches, use the workflow in `guide/script-generation-workflow.md`.

### Step 4: Generate Script

Follow the mandatory script structure:

```javascript
// 1. Initialize
newmodel()
set("unit", "stress-pa")  // or specified unit

// 2. Create Geometry
// Use y=0 as ground surface, negative y for depth (CONVENTION A)
// OR use y=0 as bottom, positive y for height (CONVENTION B - slope only)

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
```

### Step 5: Validate (Mandatory)

Run ALL checks from `guide/validation-checklist.md` before delivering.

**Critical checks (must pass):**
- [ ] No placeholder values remain (`...`, `x1`, `value`, `model_type`)
- [ ] All coordinates use consistent convention (document which one)
- [ ] Material model matches analysis type (elastic → IsoElastic, plastic → Mohr-Coulomb, etc.)
- [ ] Domain boundaries are at least 3-5x tunnel diameter away from excavation
- [ ] Initial equilibrium solved before construction stages
- [ ] Displacements reset (`initial("xydisp", 0)`) before construction stages
- [ ] Boundary conditions prevent rigid body motion
- [ ] Plot commands show relevant results for the analysis type

**Engineering sanity checks:**
- [ ] Material properties are within typical ranges for the material type
- [ ] K0 values are appropriate for soil type and OCR
- [ ] Mesh size is fine enough near excavations/structures (typically domain/50 to domain/100)
- [ ] Relaxation factor is between 0.4-0.8 for tunnel excavation
- [ ] Structural element properties are physically reasonable (E, I, area)

### Step 6: Deliver

Present the script with:
1. **Brief explanation** of what the script does
2. **Assumptions made** (especially if using default values)
3. **Disclaimer** if using typical material values
4. **Instructions** for running in ADONIS
5. **Suggested modifications** if user wants to refine

## Output Format

```javascript
// [Script title and brief description]
// Generated for: [user's request summary]
// Assumptions: [list key assumptions]

[Complete ADONIS script]

// --- End of script ---
// To run: Open ADONIS → File → Load/Call Script → Select this file
// Or from command line: ADONIS.exe <filename.ajs>
```

## Known Limitations

1. **2D Plane Strain Only**: ADONIS is a 2D program. Cannot model 3D effects.
2. **Material Models**: Limited to 8 built-in models. Custom models require C++ DLL.
3. **Groundwater**: Simplified water table. No coupled flow-deformation analysis.
4. **Structural Elements**: Beam, liner, cable, tieback, strip only. No complex structural systems.
5. **Dynamic Analysis**: Not covered by this skill. Requires additional verification.
6. **Typical Values**: Material properties from guide are for prototyping only. NOT for design verification, tender design, claim assessment, or safety-critical decisions without project-specific geotechnical parameters.

## Coordinate Conventions

**CONVENTION A (Default for tunnel, excavation, foundation):**
- `y = 0` at ground surface
- Negative `y` for depth below surface
- Example: tunnel at 20m depth → center at `(0, -20)`

**CONVENTION B (Slope stability only):**
- `y = 0` at bottom of model
- Positive `y` for height above bottom
- Example: 30m high slope → top at `y = 30`

**MUST document which convention is used in each script.**

## Engineering Disclaimer

️ **IMPORTANT**: Scripts generated by this skill are for **preliminary analysis and prototyping only**. They are NOT suitable for:
- Final design verification
- Tender design
- Claim assessment
- Safety-critical decisions
- Regulatory submissions

All scripts MUST be reviewed by a qualified geotechnical engineer before use in professional practice. Material properties, boundary conditions, and analysis parameters must be validated against site-specific investigation data.

## Version

- **Skill Version**: 1.1
- **Based on**: ADONIS User Manual V3.90
- **Last Updated**: 2026-06-20
- **Changelog**: See `CHANGELOG.md`
