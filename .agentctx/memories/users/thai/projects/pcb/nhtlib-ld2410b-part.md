---
name: nhtlib-ld2410b-part
description: HLK-LD2410B symbol/footprint/3D added to nhtlib 2026-09-28; where the MCP issue log lives
type: project
author: thai
created: 2026-10-01
updated: 2026-10-01
---

2026-09-28: added `nhtlib:HLK-LD2410B` (symbol + footprint) and `kicad_lib/nhtlib.3dshapes/HLK-LD2410B.step`
= user's GrabCAD LD2410B.step (~/Downloads) + 1.27mm header added by `ld2410b_3d.py` next to it (STANDOFF=2.0mm param,
must clear 1.61mm LDO). Footprint = flat mount, antenna up; outline from GrabCAD model. The footprint's
`(model ...)` block is hand-added — create_footprint(overwrite) drops it, re-add after any regen.
Schematic J3 swapped to nhtlib:HLK-LD2410B 2026-09-28 (change_symbol; GND/VDD power syms moved to x=213.36 right of body). ERC unchanged (3 old C_Small warnings).

MCP issue log for this session: `kicad-mcp/dev_context/lib-part-ld2410b-2026-09-28/README.md`
(top: create_footprint has no 3D model param; no tool creates a valid empty PCB; add_symbol fields overlap vertical pins).
See [[lightstrip-sch-update-plan]].
