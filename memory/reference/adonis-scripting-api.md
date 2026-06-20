---
name: ADONIS Scripting API Reference
description: Comprehensive ADONIS FEM scripting API reference from official User Manual V3.90 - covers JS/Python commands, element/node getter/setter, all 8 material models
type: reference
---

ADONIS FEM scripting API reference at `/Users/wiwat/Downloads/andonis/ADONIS_SCRIPTING_API_REFERENCE.md` (1808 lines).

**Source:** Official ADONIS User Manual V3.90 English (HTML files in `ADONIS_User_Manual_V3.90_English/`).

**Key contents:**
- JavaScript fundamentals (variables, strings, arrays, operators, conditions, loops, functions, math)
- Element getter/setter: `getelem()` (allid, activeid, nodeid, numnode, numgauss, gausspointpos, strain, stress, pp, idatpoint, prop) and `setelem()` (stress, pp)
- Node getter/setter: `getnode()` (allid, activeid, boundid, pos, disp, unbal, idatpoint) and `setnode()` (xforce, yforce, xvel, yvel, xfix, yfix, xfree, yfree)
- File I/O: fopen, fprintf, fclose
- Python API: `adonis` module with `command()`, `set_path()`, `fos()`, element/node classes
- All 8 material models: IsoElastic, Mohr-Coulomb, Hoek-Brown, Modified Hoek-Brown, Cam-Clay, Strain-Softening, P-Hardening, Ubiquitous-Joint
- All structural elements: beam, cable, tieback, strip
- All solve modes: static, elastic, FOS, relax
- All settings: calculation, mechanical, gravity, water table
- All plot commands: contour (20+ quantities), structure, interface, query, chart, element

**How to apply:** Use this reference when generating ADONIS scripts from natural language. It is the authoritative source — prefer it over reverse-engineered knowledge.
