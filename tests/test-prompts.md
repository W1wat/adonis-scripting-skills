# Test Prompts for Regression Testing

Use these standard prompts to verify skill consistency across updates.

---

## Test 1: Basic Tunnel (Elastic)

**Prompt:**
```
Create a circular tunnel with radius 5m at 20m depth. Use elastic material with E=1GPa, nu=0.3.
```

**Expected:**
- Template 1A (IsoElastic) should be used
- Material: `IsoElastic` with shear=E/(2*(1+nu)), bulk=E/(3*(1-2*nu))
- Coordinate convention A
- Domain at least 3-5x tunnel diameter

---

## Test 2: Basic Tunnel (Mohr-Coulomb)

**Prompt:**
```
สร้างอุโมงค์วงกลม รัศมี 5 เมตร ลึก 20 เมตร ดินเป็น sand, density 1800, friction 35
```

**Expected:**
- Template 1B (Mohr-Coulomb) should be used
- Material: `Mohr-Coulomb` with coh=0, fric=35
- K0 = 0.3-0.5 (sand)
- Plot should include `plot("element","state")`

---

## Test 3: Slope Stability (FOS)

**Prompt:**
```
Analyze slope stability for a 30m high slope with 45 degree angle. Soil: clay, cohesion 20kPa, friction 25 degrees.
```

**Expected:**
- Template 3 should be used
- Coordinate convention B (y=0 at bottom)
- `solve("fos")` as final command
- Material: `Mohr-Coulomb`

---

## Test 4: Deep Excavation with Support

**Prompt:**
```
Model a 15m deep excavation in clay (c=15kPa, phi=22). Install diaphragm wall and 2 levels of tiebacks.
```

**Expected:**
- Template 4 should be used
- Sequential excavation with support installation
- `structure("drawliner",...)` for diaphragm wall
- `structure("drawtieback",...)` for tiebacks
- Interface materials assigned

---

## Test 5: NATM Tunnel

**Prompt:**
```
Create NATM tunnel with sequential excavation. Top heading then bench. Install shotcrete after each stage.
```

**Expected:**
- Template 2 should be used
- Multiple excavation stages
- `solve("relax",...)` for stress relaxation
- Shotcrete beam properties calculated from thickness

---

## Test 6: Missing Information (Should Ask)

**Prompt:**
```
Create a tunnel model.
```

**Expected:**
- AI should ask for: tunnel size, depth, material type, domain size
- Should NOT generate script with placeholder values

---

## Test 7: P-Hardening Model

**Prompt:**
```
Model deep excavation using P-Hardening soil model. Clay with E50=30MPa, friction=25 degrees.
```

**Expected:**
- Template 7 should be used
- Material: `P-Hardening` with all required parameters
- Initial principal stresses set via `setelem("prop","sig1",...)`
- More complex than Mohr-Coulomb templates

---

## Validation Criteria

For each test:
1. Run the prompt through the skill
2. Check generated script against expected template
3. Verify all validation checklist items pass
4. Confirm no placeholder values remain
5. Verify coordinate convention is documented
6. Check material properties are within typical ranges

---

## Version

- **Test Suite Version**: 1.0
- **Created**: 2026-06-20
