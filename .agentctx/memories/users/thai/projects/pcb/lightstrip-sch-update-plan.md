---
name: lightstrip-sch-update-plan
description: Decisions + findings for the LightStripSensor schematic update via kicad-mcp (2026-09-27), and MCP issues logged so far
type: project
author: thai
created: 2026-10-01
updated: 2026-10-01
---

Schematic update started 2026-09-27; **done 2026-09-27 (session 2)**, ERC clean except 3 C_Small lib_symbol_mismatch warnings.

State after update:
- U1 now `nhtlib:ESP32-C3-SuperMini` (fixed in lib 2026-09-28 by direct file edit, user-approved: power pins numbered+named 5V/GND/3V3 (KiCad 10 renders "~" names literally), body rect shrunk to ±8.89 so pins sit outside, 3V3 power_out, fp nhtlib:ESP32-C3-SuperMini-DIP). Temp `ESP32-C3-SuperMini-DIP` symbol deleted from lib (footprint of same name kept).
- Q1 low-side (D=Out1, S=GND). J1/J2 = Connector:Barrel_Jack_Switch, fp BarrelJack_Horizontal, pin3 NC. J1 pin1=V_in, pin2=Out1.
- J3 = Conn_01x05 'HLK-LD2410B' (1.27mm header). GPIO4=SENSOR_OUT, GPIO3=LD_TX (ESP RX), GPIO1=LD_RX (ESP TX), GPIO10=CH1 PWM. PWR_FLAG on V_in + GND.
- Open items: C/R/D1 have no footprint; U2 value LM7805_TO220 but fp TO-263; no fuse / reverse-polarity; only 100uF bulk.

Decisions by user:
- Q1 IRLR7843 → **low-side** (S=GND, D=Out-). Output jack: center=+12V (V_in), sleeve=Out- switched. D1 stays cathode V_in / anode Out-.
- Input + output = **DC jack 5.5x2.1** (user accepts ~5A real limit vs 10A load, just note it).
- Sensor HLK-LD2410B on VDD (5V). Pinout 1 OUT, 2 TX, 3 RX, 4 GND, 5 VCC; 1.27mm pitch; UART 256000; OUT 3.3V.

Findings:
- (FIXED 2026-09-28) U1 `nhtlib:ESP32-C3-SuperMini` (/Users/thainguyen/Desktop/electronics/kicad_lib/nhtlib.kicad_sym): 5V/GND/3V3 pins have EMPTY pin numbers → merged, shorts 5V/GND/3V3. Footprint pads are named `5V`/`GND`/`3V3` (nhtlib:ESP32-C3-SuperMini-DIP). Symbol footprint field points to nonexistent `ESP32C3-SuperMini:ESP32-C3-SuperMini`.
- CH1 global label (PWM) not connected to ESP. Missing PWR_FLAG on V_in/GND. LM7805 12→5V ~2W dissipation.
- Avoid ESP32-C3 strapping GPIO2/8/9 for PWM/sensor.

MCP issues logged (for kicad-mcp dev, see [[kicad-mcp-dev]]):
- list_schematic_labels ignores global labels
- run_erc positions not in mm (58.42mm → 0.5842); writes -erc.json into project dir by default
- duplicate_pins from empty pin numbers not diagnosed
- no tool to edit existing lib symbol pins / update schematic symbol from lib
- no check for footprint field pointing at missing fp lib
- live reload only via env read at import; desktop app caches MCP config per session, so enabling needs a new session
- (session 2) no way to modify/replace an existing lib symbol; add_symbol only adds -> had to create a new symbol name
- (session 2) no "swap symbol keeping connections"; remove_component leaves dangling wires silently, rewire by hand
- (session 2) wire_pins_to_net only makes local labels; no option for global label / power symbol
- (session 2) add_power_symbol has no value param (can't make power:+12C valued VDD) and always adds a PWR_FLAG (dup flags)
- (session 2) no net-list query tool; had to export_netlist XML and parse it to verify connectivity
- (session 2) nothing flags wires crossing mid-segment (visually ambiguous, not connected)
- (session 2) no feedback whether live reload actually reached the open editor
- (session 2) run_erc pos still /100 scaled (still unfixed)

Session 3 (2026-09-27): user flagged readability (fields over bodies, flags stacked, labels over wires). Fixed in kicad-mcp (uncommitted in repo, entry in changelogs/unreleased.md, write-up + probes in dev_context/schematic-layout-2026-09-27/): label justify, field autoplace, move_component carries fields, rotated-pin direction bug, add_power_symbol value/pwr_flag + offset flag, wire_pins_to_net label_type (auto: power symbol > global on flat sheet > local). Schematic re-laid out through a fresh server via dev_context/mcp-comparison-2026-09-27/run.py (the desktop-app MCP process still runs old code until restarted). J3 moved to x=200.66. Pre-relayout backup was only in the session scratchpad.
- (2026-09-28) U1 pin-number/name overlap fixed via name "~". MCP gaps hit: no tool to delete a lib symbol or edit a lib symbol property; modify_symbol can't address pins with empty numbers; change_symbol silently reuses a stale embedded lib_symbol copy; update_schematic_symbols can't pin_map empty numbers.
