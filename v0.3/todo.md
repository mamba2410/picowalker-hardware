# Todo before sending off for fab

## Parts

- Consolidate pull-ups to be a single value e.g. 10k or 5.1k, maybe also set ILIM to 5.1k for one less value.
- Double check size of button contacts (currently 5mm diameter)

## Layout

- Possibly move everything down a bit so that the 3.3V reg inductor won't be right next to the QSPI lines.
    They run at 48 MHz and the inductor switches at 1 MHz so it shouldn't be too big of an issue.
    There's also about 1.4mm of gap with probably 0.8mm ground pour between the inductor and SD0 line.
- Move USB connector further out because its too recessed in the shell.

## Checks

- Assign and check part numbers and that they are in stock on mouser/partsbox
- Stitching vias
- Panelise
```
kikit panelize -p v0.3-kikit.json .\v0.3.kicad_pcb outputs\panelized\v0.3-panelized.kicad_pcb
```
- Export to JLC



