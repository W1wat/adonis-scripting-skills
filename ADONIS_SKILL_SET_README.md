# ADONIS Scripting Skills — Overview

ชุด Skill สำหรับสร้าง ADONIS FEM scripts จากภาษามนุษย์

## ไฟล์หลักในชุด

| ไฟล์ | คำอธิบาย |
|------|----------|
| `README.md` | คู่มือหลักของ repo |
| `SKILL.md` | AI entrypoint — trigger, workflow, rules, validation |
| `ADONIS_SCRIPTING_API_REFERENCE.md` | API reference ครบถ้วนจาก Official User Manual V3.90 (1,808 lines) |
| `guide/script-generation-workflow.md` | Workflow + 3 common patterns + material guidelines |
| `guide/quick-reference.md` | สรุป commands ที่ใช้บ่อยพร้อม syntax |
| `guide/validation-checklist.md` | 15 หมวด deterministic checks |
| `guide/limitations-and-disclaimer.md` | Known limitations + engineering disclaimer |
| `templates/all-templates.md` | 8 template entries (7 use-case groups) |
| `examples/tunnel-basic-sand.md` | Example: prompt → script → review |
| `tests/test-prompts.md` | 7 regression test prompts |

## วิธีใช้งาน

1. อ่าน `SKILL.md` เป็น entrypoint หลัก
2. AI จะอ่านไฟล์ตามลำดับที่กำหนดใน SKILL.md
3. สั่งเป็นภาษามนุษย์ เช่น "สร้างโมเดลอุโมงค์วงกลม รัศมี 5 เมตร ลึก 20 เมตร"

## Version

- **Version**: 1.1.4
- **Based on**: ADONIS User Manual V3.90
- **Last Updated**: 2026-06-20
