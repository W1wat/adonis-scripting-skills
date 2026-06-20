# ADONIS Scripting Skills

ชุด Skill สำหรับสร้าง ADONIS FEM scripts จากภาษามนุษย์ — รองรับอุโมงค์, วิเคราะห์เสถียรภาพลาดดิน, ขุดลึก, และฐานราก

## 📚 Files ในชุด Skill

### 1. ADONIS_SCRIPTING_API_REFERENCE.md
คู่มือ API ครบถ้วนจาก Official User Manual V3.90 (1,808 lines)
- JavaScript fundamentals, Element/Node getter/setter, File I/O, Python API
- 8 Material Models: IsoElastic, Mohr-Coulomb, Hoek-Brown, Generalized Hoek-Brown, Cam-Clay, Strain-Softening, P-Hardening, Ubiquitous-Joint
- Structural elements, Solve commands, Settings, Plot commands

### 2. adonis-script-generation.md
คู่มือการสร้าง scripts จาก natural language
- Workflow, Common Patterns (Tunnel, Slope, Excavation)
- Material Property Guidelines (typical values)
- Validation Checklist

### 3. adonis-quick-reference.md
สรุป commands ที่ใช้บ่อยพร้อม syntax และ examples

### 4. adonis-templates.md
7 Complete Templates:
1. Simple Circular Tunnel (Elastic Analysis)
2. NATM Tunnel with Shotcrete (Sequential Excavation)
3. Slope Stability (Factor of Safety)
4. Deep Excavation with Diaphragm Wall and Tiebacks
5. Foundation on Layered Soil
6. Anchored Sheet Pile Wall
7. P-Hardening Model (Advanced Soil)

### 5. adonis-scripting-api.md
Pointer ไปยัง API reference เต็ม

##  วิธีใช้งาน

แค่สั่งเป็นภาษามนุษย์ เช่น:

> "สร้างโมเดลอุโมงค์วงกลม รัศมี 5 เมตร ที่ระดับความลึก 20 เมตร ดินเป็น sand ความหนาแน่น 1800 kg/m3, friction 35 องศา"

AI จะอ่าน skill files, เลือก template ที่เหมาะสม, ปรับ parameters, และสร้าง script ที่สมบูรณ์

## 📋 Material Properties Reference

| Material | density (kg/m³) | shear (Pa) | coh (Pa) | fric (°) |
|----------|----------------|------------|----------|----------|
| Soft Clay | 1600-1800 | 1e6-5e6 | 5000-20000 | 15-25 |
| Stiff Clay | 1800-2000 | 1e7-5e7 | 20000-100000 | 20-30 |
| Loose Sand | 1600-1800 | 1e7-3e7 | 0-5000 | 28-35 |
| Dense Sand | 1800-2000 | 3e7-8e7 | 0-10000 | 35-45 |
| Rock | 2400-2700 | 1e9-1e10 | 1e5-1e7 | 35-50 |

## 🎯 Use Cases

- ✅ อุโมงค์ (Tunnel) - circular, horseshoe, NATM
- ✅ Slope stability analysis (FOS)
- ✅ Deep excavation พร้อม support
- ✅ Foundation on layered soil
- ✅ Sheet pile walls
- ✅ Advanced soil models (P-Hardening, Cam-Clay, Hoek-Brown)

## 📖 Resources

- **Official Website**: roozbehgm.com
- **User Manual**: V3.90 English
- **Based on**: 11 official tutorial scripts

---

*Created: 2026-06-19 | Version: 1.0 | Based on ADONIS User Manual V3.90*
