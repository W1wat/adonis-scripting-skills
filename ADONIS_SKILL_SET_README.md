# ADONIS Scripting Skill Set

ชุด skill สำหรับสร้าง ADONIS FEM scripts จากคำสั่งภาษามนุษย์

## 📚 Files ในชุด Skill

### 1. **ADONIS_SCRIPTING_API_REFERENCE.md** (1,808 lines)
**Location:** `/Users/wiwat/Downloads/andonis/ADONIS_SCRIPTING_API_REFERENCE.md`

คู่มือ API ครบถ้วนจาก Official User Manual V3.90 ประกอบด้วย:
- JavaScript fundamentals (variables, strings, arrays, operators, conditions, loops, functions, math)
- Element getter/setter (`getelem`, `setelem`)
- Node getter/setter (`getnode`, `setnode`)
- File I/O (`fopen`, `fprintf`, `fclose`)
- Python API (`adonis` module)
- **8 Material Models**: IsoElastic, Mohr-Coulomb, Hoek-Brown, Modified Hoek-Brown, Cam-Clay, Strain-Softening, P-Hardening, Ubiquitous-Joint
- Structural elements (beam, cable, tieback, strip)
- Solve commands (static, elastic, FOS, relax)
- Settings (calculation, mechanical, gravity, water table)
- Plot commands (contour, structure, interface, query, chart, element)

**ใช้เมื่อ:** ต้องการดูรายละเอียด command, parameters, หรือ syntax ที่ถูกต้อง

---

### 2. **Script Generation Guide** (Memory)
**Location:** `memory/project/adonis-script-generation.md`

คู่มือการสร้าง scripts จาก natural language ประกอบด้วย:
- **Workflow**: ขั้นตอนการสร้าง script ตั้งแต่ต้นจนจบ
- **Common Patterns**: 
  - Pattern A: Simple Tunnel (Circular)
  - Pattern B: Slope Stability (FOS)
  - Pattern C: Deep Excavation with Support
- **Material Property Guidelines**: ค่า typical สำหรับดิน/หินชนิดต่างๆ
- **Validation Checklist**: รายการตรวจสอบก่อนส่งมอบ script

**ใช้เมื่อ:** ต้องการสร้าง script ใหม่จากคำอธิบายของ user

---

### 3. **Quick Command Reference** (Memory)
**Location:** `memory/project/adonis-quick-reference.md`

สรุป commands ที่ใช้บ่อยพร้อม syntax และ examples:
- Model setup (`newmodel`, `set`)
- Geometry (`rect`, `line`, `circle`, `arc`, `crack`, `joint`)
- Meshing (`discretize`, `segment`, `gmsh`)
- Materials (`material create/assign`)
- Boundary conditions (`applybc`)
- Initial stresses (`initial`)
- Excavation (`excavate`, `fill`)
- Structural elements (`structure drawbeam/drawliner/drawtieback`)
- Solve (`solve`, `solve fos`, `solve relax`)
- Plot (`plot contour/struc/interf`)
- Low-level access (`getelem/setelem`, `getnode/setnode`)

**ใช้เมื่อ:** ต้องการดู syntax เร็วๆ โดยไม่ต้องเปิด manual ทั้งเล่ม

---

### 4. **Example Templates** (Memory)
**Location:** `memory/project/adonis-templates.md`

Templates สมบูรณ์ 7 ตัวอย่าง:
1. **Simple Circular Tunnel** - อุโมงค์วงกลมแบบง่าย (elastic analysis)
2. **NATM Tunnel with Shotcrete** - อุโมงค์ NATM พร้อม shotcrete (sequential excavation)
3. **Slope Stability (FOS)** - วิเคราะห์เสถียรภาพลาดดิน (factor of safety)
4. **Deep Excavation with Diaphragm Wall** - ขุดลึกพร้อมผนัง diaphragm และ tiebacks
5. **Foundation on Layered Soil** - ฐานรากบนดินหลายชั้น
6. **Anchored Sheet Pile Wall** - เขื่อน sheet pile พร้อม tiebacks
7. **P-Hardening Model** - ขุดลึกด้วย P-Hardening model (advanced soil)

**ใช้เมื่อ:** ต้องการ template ที่พร้อมใช้งาน หรือต้องการดูตัวอย่างการใช้งาน commands

---

## 🚀 วิธีใช้งาน

### ตัวอย่างการสั่งงาน (Natural Language)

```
"สร้างโมเดลอุโมงค์วงกลม รัศมี 5 เมตร ที่ระดับความลึก 20 เมตร 
ดินเป็น sand ความหนาแน่น 1800 kg/m3, friction 35 องศา"
```

**AI จะ:**
1. อ่าน Script Generation Guide เพื่อดู workflow
2. เลือก Pattern A (Simple Tunnel) จาก templates
3. ปรับ parameters ตามที่ user บอก (radius=5m, depth=20m, sand properties)
4. สร้าง script ที่สมบูรณ์
5. ตรวจสอบด้วย validation checklist

### ตัวอย่างผลลัพธ์

```javascript
newmodel()
set("unit","stress-pa")

// Domain
rect("startPoint",-50,-50,"endPoint",50,0)
circle("centerPoint",0,-20,"radius",5,"numSeg",20)

discretize("maxedge",1.5)
gmsh("maxedge",1.5,"elemtype","T3","useNMD","on")

material("create","Mohr-Coulomb","matid",1,"matname","Sand",
  "density",1800,"shear",2e7,"bulk",4e7,"coh",0,"fric",35,"dil",0,"tens",0)
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

solve()

// Excavate tunnel
initial("xydisp",0)
excavate("region",0,-20)
solve()

plot("contour","totdisp")
tab("plot")
plot("contour","syy")
```

---

## 📋 Material Properties Reference

### Typical Values (จาก Script Generation Guide)

| Material | density (kg/m³) | shear (Pa) | bulk (Pa) | coh (Pa) | fric (°) | dil (°) | tens (Pa) |
|----------|----------------|------------|-----------|----------|----------|---------|-----------|
| Soft Clay | 1600-1800 | 1e6-5e6 | 2e6-1e7 | 5000-20000 | 15-25 | 0 | 0 |
| Stiff Clay | 1800-2000 | 1e7-5e7 | 2e7-1e8 | 20000-100000 | 20-30 | 0-5 | 0-10000 |
| Loose Sand | 1600-1800 | 1e7-3e7 | 2e7-6e7 | 0-5000 | 28-35 | 0 | 0 |
| Dense Sand | 1800-2000 | 3e7-8e7 | 6e7-1.6e8 | 0-10000 | 35-45 | 5-15 | 0 |
| Rock | 2400-2700 | 1e9-1e10 | 2e9-2e10 | 1e5-1e7 | 35-50 | 0-10 | 1e4-1e6 |

### K0 Values (Lateral Earth Pressure Coefficient)
- Normally consolidated clay: 0.5-0.7
- Overconsolidated clay: 0.7-1.5
- Sand: 0.3-0.5

---

## 🎯 Use Cases ที่รองรับ

### ✅ รองรับ
- ✅ อุโมงค์ (Tunnel) - circular, horseshoe, NATM
- ✅ Slope stability analysis (FOS)
- ✅ Deep excavation พร้อม support (diaphragm wall, tiebacks)
- ✅ Foundation on layered soil
- ✅ Sheet pile walls
- ✅ Advanced soil models (P-Hardening, Cam-Clay, Hoek-Brown)
- ✅ Sequential excavation
- ✅ Groundwater effects
- ✅ Structural elements (beam, liner, cable, tieback, strip)

### ⚠️ ข้อจำกัด
- ⚠️ Complex 3D geometry - ต้องใช้ GUI ช่วย
- ⚠️ Dynamic analysis - ต้องตรวจสอบ commands เพิ่มเติม
- ⚠️ User-defined constitutive model (UDCM) - ต้องเขียน C++ DLL

---

## 📖 การเข้าถึง Skills

Skills ทั้งหมดถูกบันทึกใน:
```
/Users/wiwat/.qwen/projects/-Users-wiwat-Downloads-andonis/memory/
├── MEMORY.md                           (index)
├── reference/
│   └── adonis-scripting-api.md         (API reference pointer)
└── project/
    ├── adonis-script-generation.md     (generation guide)
    ├── adonis-quick-reference.md       (quick reference)
    └── adonis-templates.md             (example templates)
```

**Full API Reference:**
```
/Users/wiwat/Downloads/andonis/ADONIS_SCRIPTING_API_REFERENCE.md
```

---

## 💡 Tips

1. **ระบุข้อมูลให้ครบ**: geometry, material properties, groundwater, construction stages
2. **ใช้ typical values**: ถ้าไม่แน่ใจ AI จะใช้ค่า typical จาก guide
3. **ตรวจสอบ script**: AI จะมี validation checklist ให้ตรวจสอบก่อนส่งมอบ
4. **ปรับแต่งได้**: บอก AI ได้ว่าต้องการ plot อะไร, ใช้ material model ไหน, ฯลฯ

---

## 🔗 Resources

- **Official Website**: roozbehgm.com (geowizard.org offline)
- **User Manual**: V3.90 English (HTML files in project)
- **Tutorials**: 11 tutorial scripts in `/scripts/` folder
- **Forum**: geowizard.org/forum (when online)

---

*Created: 2026-06-19*
*Version: 1.0*
*Based on: ADONIS User Manual V3.90*
