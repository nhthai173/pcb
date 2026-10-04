---
name: project-location
description: LightStripSensor + nhtlib moved into pcb git repo on 2026-09-28; lib paths are ${KIPRJMOD}-relative
type: project
author: thai
created: 2026-10-01
updated: 2026-10-01
---

Since 2026-09-28 the project lives at `~/Desktop/electronics/pcb/LightStripSensor` and the library at `~/Desktop/electronics/pcb/kicad_lib` (git repo github.com/nhthai173/pcb). Project fp/sym-lib-table and nhtlib footprint 3D models use `${KIPRJMOD}/../kicad_lib/...`, so projects must sit directly under `pcb/`. Global KiCad sym-lib-table (10.0, 10.99) points to the absolute new path.

**Why:** old paths `~/Desktop/electronics/{LightStripSensor,kicad_lib}` no longer exist.
**How to apply:** pass explicit paths to kicad-mcp tools (its defaults still point at the old location). Related: [[nhtlib-ld2410b-part]], [[lightstrip-sch-update-plan]].
