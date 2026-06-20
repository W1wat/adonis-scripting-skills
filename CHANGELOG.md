# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

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
- `ADONIS_SCRIPTING_API_REFERENCE.md` (1,808 lines) from official User Manual V3.90
- `ADONIS_SKILL_SET_README.md` with overview and usage guide
- `memory/project/adonis-script-generation.md` with workflow and patterns
- `memory/project/adonis-quick-reference.md` with command syntax
- `memory/project/adonis-templates.md` with 7 example templates
- `memory/reference/adonis-scripting-api.md` as API reference pointer

---

## Version History Summary

| Version | Date | Key Changes |
|---------|------|-------------|
| 1.0.0 | 2026-06-19 | Initial release with API reference and templates |
| 1.1.0 | 2026-06-20 | Added SKILL.md, fixed inconsistencies, added disclaimers |
