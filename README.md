# ADONIS Scripting Skills

ชุด Skill สำหรับสร้าง ADONIS FEM scripts จากภาษามนุษย์ — รองรับอุโมงค์, วิเคราะห์เสถียรภาพลาดดิน, ขุดลึก, และฐานราก

## 🚀 Quick Start

สั่งเป็นภาษามนุษย์ เช่น:

> "สร้างโมเดลอุโมงค์วงกลม รัศมี 5 เมตร ที่ระดับความลึก 20 เมตร ดินเป็น sand ความหนาแน่น 1800 kg/m3, friction 35 องศา"

AI จะอ่าน skill files, เลือก template ที่เหมาะสม, ปรับ parameters, และสร้าง ADONIS script (.ajs) ที่พร้อมใช้งาน

##  โครงสร้าง Repository

```
adonis-scripting-skills/
├── README.md                              ← ไฟล์นี้
├── SKILL.md                               ← AI entrypoint หลัก (trigger, workflow, rules)
├── LICENSE                                ← MIT license + engineering disclaimer
├── CHANGELOG.md                           ← Version tracking
├── ADONIS_SCRIPTING_API_REFERENCE.md      ← API reference ครบถ้วน (1,808 lines)
├── ADONIS_SKILL_SET_README.md             ← ภาพรวมชุด skill
── guide/
│   ├── script-generation-workflow.md      ← Workflow + common patterns
│   ├── quick-reference.md                 ← Command syntax สั้นๆ
│   ├── validation-checklist.md            ← Deterministic validation checks
│   └── limitations-and-disclaimer.md      ← Known limitations + engineering warnings
├── reference/
│   ── adonis-scripting-api.md            ← Pointer ไปยัง API reference
├── templates/
│   └── all-templates.md                   ← 7 complete templates
├── examples/
│   ── tunnel-basic-sand.md               ← Example: prompt → script → review
└── tests/
    └── test-prompts.md                    ← 7 regression test prompts
```

## 📚 เอกสารแต่ละไฟล์

### ไฟล์หลัก

| ไฟล์ | ขนาด | คำอธิบาย |
|------|------|----------|
| `SKILL.md` | 12 KB | **AI entrypoint** — กำหนด trigger, workflow, rules, validation rules, limitations |
| `ADONIS_SCRIPTING_API_REFERENCE.md` | 52 KB | **API reference เต็ม** จาก Official User Manual V3.90 — ทุก command, parameter, material model |
| `ADONIS_SKILL_SET_README.md` | 9 KB | ภาพรวมชุด skill, วิธีใช้งาน, material properties reference |

### Guide

| ไฟล์ | ขนาด | คำอธิบาย |
|------|------|----------|
| `guide/script-generation-workflow.md` | 8 KB | Workflow การสร้าง script, 3 common patterns (tunnel, slope, excavation), material property guidelines |
| `guide/quick-reference.md` | 4 KB | สรุป commands ที่ใช้บ่อยพร้อม syntax |
| `guide/validation-checklist.md` | 6 KB | **15 หมวด deterministic checks** — ต้องผ่านทุกข้อก่อนส่งมอบ script |
| `guide/limitations-and-disclaimer.md` | 5 KB | Known limitations (2D, material models, groundwater, etc.) + engineering disclaimer |

### Templates & Examples

| ไฟล์ | ขนาด | คำอธิบาย |
|------|------|----------|
| `templates/all-templates.md` | 15 KB | **7 templates สมบูรณ์** — circular tunnel (elastic + MC), NATM, slope FOS, deep excavation, foundation, sheet pile, P-Hardening |
| `examples/tunnel-basic-sand.md` | 3 KB | Example: user prompt → generated script → validation review → expected results |

### Testing & Meta

| ไฟล์ | ขนาด | คำอธิบาย |
|------|------|----------|
| `tests/test-prompts.md` | 3 KB | 7 standard prompts สำหรับ regression testing |
| `LICENSE` | 2 KB | MIT license + engineering disclaimer |
| `CHANGELOG.md` | 2 KB | Version history (v1.0.0 → v1.1.0) |
| `reference/adonis-scripting-api.md` | 2 KB | Pointer ไปยัง API reference (relative path) |

## 🎯 Use Cases ที่รองรับ

| Use Case | Template | Material Models |
|----------|----------|----------------|
| อุโมงค์วงกลม (elastic) | 1A | IsoElastic |
| อุโมงค์วงกลม (plastic) | 1B | Mohr-Coulomb |
| อุโมงค์ NATM + shotcrete | 2 | Mohr-Coulomb |
| วิเคราะห์เสถียรภาพลาดดิน (FOS) | 3 | Mohr-Coulomb |
| ขุดลึก + diaphragm wall + tiebacks | 4 | Mohr-Coulomb |
| ฐานรากบนดินหลายชั้น | 5 | Mohr-Coulomb (multi-layer) |
| เขื่อน sheet pile + tiebacks | 6 | Mohr-Coulomb + interfaces |
| ขุดลึกด้วย P-Hardening model | 7 | P-Hardening |

##  Material Models ที่รองรับ

| Model | Use Case | Key Parameters |
|-------|----------|---------------|
| **IsoElastic** | Elastic analysis, preliminary stress | density, shear, bulk |
| **Mohr-Coulomb** | General soil/rock, FOS analysis | + coh, fric, dil, tens |
| **Hoek-Brown** | Rock mass | + sigci, mb, s, a, s3cv |
| **Modified Hoek-Brown** | Rock with tension cut-off | + tension |
| **Cam-Clay** | Soft clay, volume change | + kappa, lambda, mm, mpc |
| **Strain-Softening** | Post-peak behavior | strain-dependent tables |
| **P-Hardening** | Stress-dependent stiffness | + E50_ref, Eur_ref, p_ref, m, ocr |
| **Ubiquitous-Joint** | Jointed rock | + jangle, jcoh, jfric |

##  Coordinate Conventions

### Convention A (Default — Tunnel, Excavation, Foundation)
- `y = 0` ที่ผิวดิน
- `y` เป็นลบ = ลึกใต้ผิวดิน
- ตัวอย่าง: อุโมงค์ลึก 20m → center ที่ `(0, -20)`

### Convention B (Slope Stability Only)
- `y = 0` ที่ก้นโมเดล
- `y` เป็นบวก = สูงขึ้นจากก้น
- ตัวอย่าง: ลาดดินสูง 30m → ยอดที่ `y = 30`

> ทุก template มี comment ระบุ convention ที่ใช้

## ⚠️ Engineering Disclaimer

**Scripts ที่สร้างจาก skill นี้ใช้สำหรับ PRELIMINARY ANALYSIS AND PROTOTYPING เท่านั้น**

ไม่เหมาะสำหรับ:
- Final design verification
- Tender design
- Claim assessment
- Safety-critical decisions
- Regulatory submissions

**ต้องได้รับการตรวจสอบโดย qualified geotechnical engineer ก่อนใช้งานจริง** และต้องใช้ site-specific investigation data แทน typical values

ดูรายละเอียดเพิ่มเติมที่ [`guide/limitations-and-disclaimer.md`](guide/limitations-and-disclaimer.md)

## 🔧 วิธีใช้งานกับ AI

### ขั้นตอนที่ 1: AI อ่าน SKILL.md
ไฟล์ [`SKILL.md`](SKILL.md) เป็น entrypoint หลัก บอก AI ว่า:
- เมื่อไหร่ควรใช้ skill นี้
- ต้องอ่านไฟล์ใดก่อน (ตามลำดับ)
- ต้องถาม user อะไรบ้างก่อน generate
- ต้อง validate อะไรก่อนส่งมอบ

### ขั้นตอนที่ 2: Extract Requirements
AI จะดึงข้อมูลจากคำสั่งภาษามนุษย์:
- Geometry (รูปร่าง, ขนาด, ความลึก)
- Material (ชนิดดิน/หิน, คุณสมบัติ)
- Groundwater (ระดับน้ำใต้ดิน)
- Construction stages (ขั้นตอนการก่อสร้าง)
- Analysis type (static, FOS, excavation)
- Output (ต้องการ plot อะไร)

### ขั้นตอนที่ 3: Ask Before Generate
หากข้อมูลไม่ครบ AI จะถามก่อน เช่น:
- ขนาด domain?
- Material model ที่ต้องการ?
- ระดับน้ำใต้ดิน?
- ขั้นตอนการก่อสร้าง?

### ขั้นตอนที่ 4: Generate + Validate
AI สร้าง script ตาม workflow ใน [`guide/script-generation-workflow.md`](guide/script-generation-workflow.md) แล้วตรวจสอบด้วย [`guide/validation-checklist.md`](guide/validation-checklist.md) ก่อนส่งมอบ

## 🧪 Testing

ใช้ [`tests/test-prompts.md`](tests/test-prompts.md) สำหรับ regression testing — 7 standard prompts ที่ครอบคลุมทุก use case

##  Material Properties Reference (Typical Values)

| Material | density (kg/m³) | shear (Pa) | coh (Pa) | fric (°) |
|----------|----------------|------------|----------|----------|
| Soft Clay | 1600-1800 | 1e6-5e6 | 5,000-20,000 | 15-25 |
| Stiff Clay | 1800-2000 | 1e7-5e7 | 20,000-100,000 | 20-30 |
| Loose Sand | 1600-1800 | 1e7-3e7 | 0-5,000 | 28-35 |
| Dense Sand | 1800-2000 | 3e7-8e7 | 0-10,000 | 35-45 |
| Rock | 2400-2700 | 1e9-1e10 | 1e5-1e7 | 35-50 |

> ⚠️ Typical values สำหรับ prototyping เท่านั้น — ใช้ site-specific data สำหรับงานจริง

**K0 values:**
- Normally consolidated clay: 0.5-0.7
- Overconsolidated clay: 0.7-1.5
- Sand: 0.3-0.5

## 📖 Resources

- **Official Website:** [roozbehgm.com](http://roozbehgm.com)
- **Based on:** ADONIS User Manual V3.90
- **Tutorial scripts:** 11 official tutorials (ในโฟลเดอร์ `scripts/`)

## 📄 License

MIT License — ดู [`LICENSE`](LICENSE) สำหรับรายละเอียดและ engineering disclaimer

## 📋 Changelog

ดู [`CHANGELOG.md`](CHANGELOG.md) สำหรับ version history

| Version | Date | Key Changes |
|---------|------|-------------|
| 1.1.0 | 2026-06-20 | Added SKILL.md, fixed inconsistencies, added disclaimers, restructured repo |
| 1.0.0 | 2026-06-19 | Initial release with API reference and templates |

---

*Created: 2026-06-19 | Updated: 2026-06-20 | Version: 1.1.0*
