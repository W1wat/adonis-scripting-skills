# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [1.1.4] - 2026-06-20

### Fixed
- Removed `tab("plot")` from all templates and guides (not documented in API reference)
- Commented out `setelem("prop",...)` loop in Template 7 (not documented in API reference)

## [1.1.3] - 2026-06-20

### Fixed
- Added representative `sig1`, `sig2`, `sig3` values to Template 7 P-Hardening `material("create",...)` call to prevent missing required parameter errors
- Clarified Template 7 P-Hardening warning to emphasize verification of principal stress convention
- Updated template count wording: "8 template entries covering 7 use-case groups"
- Fixed README example description: "expected results" → "qualitative checks"

## [1.1.2] - 2026-06-20

### Fixed (Review Round 3 — Syntax Blockers)
- Fixed `drawtieback` syntax in all templates and guides: removed `fromstrucnodeatpoint` and `pretens` (not in API reference), use `frompoint` per API reference
- Fixed `drawliner` syntax in all templates and guides: removed `iftype`/`bothSides` (not in API reference), use `ifid1`/`ifid2` directly
- Fixed Template 7 P-Hardening: marked as EXPERIMENTAL with warning about `setelem("prop",...)` not being in API reference
- Updated `ADONIS_SKILL_SET_README.md`: removed absolute paths, updated to v1.1.1
- Removed `scripts/` folder reference from README.md (tutorial scripts not included in repo)
- Fixed validation checklist category 13: added FOS exception for plot requirement
- Fixed validation checklist category 15: corrected tieback syntax reference
- Added copyright/trademark notice to API reference header

## [1.1.1] - 2026-06-20

### Fixed (Review Round 2)
- Fixed path mismatch in `SKILL.md`: `reference/ADONIS_SCRIPTING_API_REFERENCE.md` → `ADONIS_SCRIPTING_API_REFERENCE.md` (file is at repo root)
- Fixed template path mismatch in `SKILL.md`: changed from individual files to `templates/all-templates.md` with template numbers (1A, 1B, 2, 3, etc.)
- Fixed coordinate convention conflict in `guide/script-generation-workflow.md`: now explicitly states TWO conventions (A for tunnel/excavation, B for slope)
- Fixed validation checklist FOS vs plot conflict: separated rules for static/construction (require plots) vs FOS (solve("fos") is final, plots optional)
- Fixed example `Expected Results` → `Typical Qualitative Checks` (qualitative behavior, not numerical values)
- Fixed README Quick Start wording: "พร้อมใช้งาน" → "พร้อมนำไปตรวจสอบและปรับแก้ในโปรแกรม ADONIS"

## [1.1.0] - 2026-06-20

### Added
- `SKILL.md` as main AI entrypoint with trigger, workflow, rules, and validation
- `LICENSE` with MIT license and engineering disclaimer
- `CHANGELOG.md` for version tracking
- `guide/limitations-and-disclaimer.md` with known limitations and engineering warnings
- Coordinate convention documentation (Convention A for tunnel/excavation, Convention B for slope)
- Stronger disclaimer for typical material values (not for design verification)
- Ask-before-generate rules for missing information
- Deterministic validation checklist

### Fixed
- Template 1 split into 1A (IsoElastic) and 1B (Mohr-Coulomb) to fix elastic/MC inconsistency
- Absolute path in `reference/adonis-scripting-api.md` changed to relative path
- Added explicit coordinate convention comments to all templates
- Added "Use when" guidance to each template

### Changed
- Improved validation checklist with deterministic checks
- Added engineering disclaimer to material properties table
- Restructured repo layout for clarity

## [1.0.0] - 2026-06-19

### Added
- Initial release
- `ADONIS_SCRIPTING_API_REFERENCE.md` from official User Manual V3.90
- `ADONIS_SKILL_SET_README.md` with overview and usage guide
- Script generation workflow guide with patterns
- Quick reference for command syntax
- 7 example templates for common use cases
- API reference pointer file

---

## Version History Summary

| Version | Date | Key Changes |
|---------|------|-------------|
| 1.0.0 | 2026-06-19 | Initial release with API reference and templates |
| 1.1.0 | 2026-06-20 | Added SKILL.md, fixed inconsistencies, added disclaimers |
| 1.1.1 | 2026-06-20 | Fixed path mismatches, template paths, convention conflicts, FOS validation |
| 1.1.2 | 2026-06-20 | Fixed syntax blockers (drawtieback, drawliner), Template 7 experimental, copyright notice |
| 1.1.3 | 2026-06-20 | Added sig1/sig2/sig3 to P-Hardening, template count fix, version bump |
| 1.1.4 | 2026-06-20 | Removed tab("plot"), commented out unverified setelem("prop",...) pseudocode |
