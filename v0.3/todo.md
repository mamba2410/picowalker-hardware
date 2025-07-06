# Todo before sending off for fab

## Parts

- Double check size of button contacts (currently 5mm diameter)

## Layout

- Move USB connector further out because its too recessed in the shell.
- USB impedance

## Checks

- DRC and make sure no nets are crossed due to the Big Move (TM).
- Assign and check part numbers and that they are in stock on mouser/partsbox
- Stitching vias
- Panelise
```
kikit panelize -p v0.3-kikit.json .\v0.3.kicad_pcb outputs\panelized\v0.3-panelized.kicad_pcb
```
- Export to JLC



