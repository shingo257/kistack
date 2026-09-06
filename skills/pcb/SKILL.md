---
name: kicad-pcb
description: Review KiCad PCB layouts with DRC, layer renders, and 3D-model interference inspection.
---

# Skill for PCB review

Confirm with kicad-cli that DRC rules pass. 

Use kicad-cli to render SVGs of board layers to validate that their connections look reasonable.

Use kicad-cli to render images of various areas of the PCB to validate interference with 3D models.

### Autoroute / Specctra SES pitfalls (field-tested)

- Prefer applying SES as **tracks and vias only**. KiCad `ImportSpecctraSES` can move footprints; avoid it when placement is already intentional.
- Refuse or quarantine SES files that contain almost no wires (failed freeroute / empty session). A thin SES must not wipe a working board.
- Freeroute often ignores courtyard/keepout intent (antenna keepouts, module courtyards). After autoroute, inspect those regions on SVG/render and hand-fix shorts/crossings.
- Grid engines may leave clearance/unconnected violations even when ratsnest looks "mostly done." DRC + unconnected count is the exit criterion, not "autoroute finished."
- After schematic net changes, sync pad nets / zones before trusting an old routed board. Schematic-parity DRC exists for a reason (`pcb drc --schematic-parity`).
