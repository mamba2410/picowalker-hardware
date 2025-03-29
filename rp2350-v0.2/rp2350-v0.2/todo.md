# Todo before sending off for fab

Estimated shipping and fab is 8 days, so send off before weds 2nd april at latest.

Checks:

- [x] Doubile check pico 2 inductor
    - [x] footprint
    - [x] layout
- [x] Double check rp2350 xtal
    - [x] footprint
    - [x] layout
- [x] Double check rp2350
    - [x] footprint
    - [x] via hole size
- [x] Double check pmic
    - [x] layout
- [x] Double check screen
    - [x] power planes
    - [x] pinout
    - [x] qspi routing
    - [x] measure distance from screen centre to mezannine (28mm, will be too far left in v0.2 but that's fine as connector would be almost on the edge o (ignoring R&D costs)therwise)
- [x] USB C
    - [x] Footprint
    - [x] transmission lines
- [x] bootsel and run buttons
    - [x] make sure part number is correct
    - [x] footprint, 3d model doesn't sit on footprint, compare to datasheet
- [x] lposc xtal
    - [x] footptint
    - [ ] voltage - `DCC` means LVCMOS and rp2350 says cmos. Still don't know what CMOS vs LVCMOS means but both cover 3.3V so I think I'm fine.
- [x] 3v3 reg
    - [x] passive placement
    - [x] footprint
    - [x] inductor value - LBR2012T2R2M = 2.2uH, 0805 (2012) 260 mA max load, unshielded but not near any high-speed signals
- [x] irda
    - [x] footprint (pay attention to pins)
    - [x] passives location, filter etc.
- [x] uart and swd
    - [x] pin numbering
    - [x] check I have the right parts, both in stock and in spec - I actually have BM03* version which is top mount but I can squish the pins to make it side mount, probably.
- [x] battery connector
    - [x] compare to side-mount option and rotate to allow both orientations
- Assign and check part numbers and that they are in stock on mouser/partsbox
- Stitching vias
- Panelise
```
kikit panelize -p rp2350-v0.2-kikit.json .\rp2350-v0.2.kicad_pcb outputs\panelized\rp2350-v0.2-panelized.kicad_pcb
```
- Export to JLC



