# Validation Checklist

Mandatory checks before delivering any ADONIS script. **All critical checks must pass.**

---

## Critical Checks (Must Pass)

### 1. No Placeholder Values
- [ ] No `...` remaining in script
- [ ] No `x1`, `x2`, `y1`, `y2` as literal values (should be numbers)
- [ ] No `value`, `model_type`, `matid` as literal strings (should be actual values)
- [ ] No undefined variables used before declaration

### 2. Coordinate Convention Consistency
- [ ] All geometry uses same convention (A or B)
- [ ] Convention is documented in script header comment
- [ ] No mixing of conventions within same script

**Convention A** (Tunnel/Excavation): y=0 at surface, negative y = depth
**Convention B** (Slope): y=0 at bottom, positive y = height

### 3. Material Model Matches Analysis Type
- [ ] Elastic analysis → `IsoElastic` material (no cohesion, friction, tension)
- [ ] Plastic analysis → `Mohr-Coulomb` or other plastic model
- [ ] FOS analysis → `Mohr-Coulomb` (FOS only works with MC)
- [ ] Template name matches material model used

### 4. Domain Size Adequacy
- [ ] Domain boundaries are at least **3x tunnel diameter** away from excavation
- [ ] For slopes: domain extends at least **2x slope height** beyond toe and crest
- [ ] For foundations: domain extends at least **3x foundation width** beyond edges
- [ ] Domain depth is at least **2x excavation depth** below excavation bottom

### 5. Initial Equilibrium Before Construction
- [ ] `solve()` called before any excavation
- [ ] `initial("xydisp", 0)` called after initial solve and before construction stages
- [ ] No excavation before initial equilibrium is established

### 6. Boundary Conditions Prevent Rigid Body Motion
- [ ] At least one node fixed in x-direction
- [ ] At least one node fixed in y-direction
- [ ] No unconstrained rigid body modes
- [ ] Boundary conditions are physically reasonable (not over-constrained)

### 7. Plot Commands Show Relevant Results
- [ ] At least one displacement plot (`xdisp`, `ydisp`, or `totdisp`)
- [ ] At least one stress plot (`syy`, `sxx`, or `sxy`)
- [ ] For structural elements: plot structural forces (`moment`, `axialforce`)
- [ ] For FOS analysis: `solve("fos")` is last command (FOS value displayed)

---

## Engineering Sanity Checks

### 8. Material Properties Within Typical Ranges

| Material | density (kg/m³) | shear (Pa) | coh (Pa) | fric (°) |
|----------|----------------|------------|----------|----------|
| Soft Clay | 1600-1800 | 1e6-5e6 | 5000-20000 | 15-25 |
| Stiff Clay | 1800-2000 | 1e7-5e7 | 20000-100000 | 20-30 |
| Loose Sand | 1600-1800 | 1e7-3e7 | 0-5000 | 28-35 |
| Dense Sand | 1800-2000 | 3e7-8e7 | 0-10000 | 35-45 |
| Rock | 2400-2700 | 1e9-1e10 | 1e5-1e7 | 35-50 |

- [ ] Density is within typical range for material type
- [ ] Shear modulus is within typical range
- [ ] Cohesion is within typical range (or zero for sand)
- [ ] Friction angle is within typical range
- [ ] Dilation angle ≤ friction angle (typically 0 to friction/3)
- [ ] Tension limit ≤ cohesion (typically 0 to cohesion)

### 9. K0 Values Appropriate
- [ ] Normally consolidated clay: K0 = 0.5-0.7
- [ ] Overconsolidated clay: K0 = 0.7-1.5
- [ ] Sand: K0 = 0.3-0.5
- [ ] K0 × γ × depth gives reasonable horizontal stress

### 10. Mesh Size Appropriate
- [ ] Mesh size is **domain/50 to domain/100** for general areas
- [ ] Mesh size is **excavation_size/20 to excavation_size/50** near excavations
- [ ] T3 elements use NMD (`"useNMD", "on"`) to prevent volumetric locking
- [ ] Q4 elements do not need NMD

### 11. Relaxation Factor Reasonable (Tunnel Only)
- [ ] Relaxation factor is between **0.4-0.8** (typical 0.6)
- [ ] Relaxation steps ≥ 100 (typical 250)
- [ ] Relaxation region encompasses excavation boundary

### 12. Structural Element Properties Physical
- [ ] Elastic modulus E is positive and reasonable (concrete: 20-30 GPa, steel: 200 GPa)
- [ ] Cross-sectional area A is positive
- [ ] Moment of inertia I is positive
- [ ] For shotcrete: E divided by (1-ν²) for plane strain
- [ ] Interface stiffness (jkn, jks) is 10-100× adjacent material stiffness

---

## Script Completeness Checks

### 13. Required Commands Present
- [ ] `newmodel()` at start
- [ ] `set("unit", ...)` specified
- [ ] Geometry creation commands present
- [ ] `discretize()` and `gmsh()` called
- [ ] At least one material created and assigned
- [ ] Boundary conditions applied
- [ ] `solve()` called at least once
- [ ] At least one `plot()` command at end

### 14. Gravity Loading (If Applicable)
- [ ] `set("gravity", 0, 9.81)` or similar present
- [ ] Initial stresses use `yvar` gradient for gravity-dependent stress
- [ ] Stress gradient = γ = density × gravity
- [ ] sxx and szz use K0 × syy gradient

### 15. Construction Sequence Logical
- [ ] Excavation → Support installation → Solve sequence is correct
- [ ] No support installed before excavation reaches that level
- [ ] Tiebacks attached to existing structural nodes (`fromstrucnodeatpoint`)
- [ ] Multiple construction stages each have `solve()` after changes

---

## Quick Reference: Common Mistakes

| Mistake | Fix |
|---------|-----|
| Using Mohr-Coulomb for "elastic analysis" | Use `IsoElastic` material |
| Mixing coordinate conventions | Pick one convention, document it |
| No `initial("xydisp", 0)` before construction | Add displacement reset |
| Domain too small (boundaries close to excavation) | Extend domain to 3-5x excavation size |
| FOS analysis with non-MC material | Use `Mohr-Coulomb` for FOS |
| Missing NMD for T3 elements | Add `"useNMD", "on"` to gmsh |
| Placeholder values in final script | Replace all placeholders with actual values |
| No plot commands | Add at least displacement and stress plots |

---

## Version

- **Checklist Version**: 1.1
- **Last Updated**: 2026-06-20
