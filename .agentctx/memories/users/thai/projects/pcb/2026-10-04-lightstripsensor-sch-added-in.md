---
name: 2026-10-04-lightstripsensor-sch-added-in
description: 2026-10-04 LightStripSensor sch: added input protection J2→F1 (5A fast, fp Fuse_2512 HandSolder because KiCad has no 241
type: project
author: thai
created: 2026-10-04
updated: 2026-10-04
---

2026-10-04 LightStripSensor sch: added input protection J2→F1 (5A fast, fp Fuse_2512 HandSolder because KiCad has no 2410 fp — confirm part) → Q2 AOD4185 P-MOS reverse-polarity (D=fuse side, S=V_in, G→R3 10k→GND, D2 BZT52C15 G-S) → D3 SMBJ15A (Device:D_Zener symbol, unidirectional) → V_in. U2 now Regulator_Linear:L7805, value 'L7805 (TO-263)', fp TO-263-3_TabPin2 (pin1 IN, 2/tab GND, 3 OUT verified correct). J1 moved to x=160. ERC = 3 old C_Small warnings. MCP issues: kicad-mcp/dev_context/lightstrip-protection-2026-10-04/README.md (change_symbol embeds unresolvable symbol name).
