# Todo before sending off for fab

## Parts

- Consolidate pull-ups to be a single value e.g. 10k or 5.1k, maybe also set ILIM to 5.1k for one less value.
- Create battery connector part

## Layout

- Figure out how to route power properly, 3.3V reg can't be behind membrane button because of vias.
- Potentially have PMIC in bottom right, with 3.3V reg top right next to battery connector.


## Checks

- Assign and check part numbers and that they are in stock on mouser/partsbox
- Stitching vias
- Panelise
```
kikit panelize -p rp2350-v0.2-kikit.json .\rp2350-v0.2.kicad_pcb outputs\panelized\rp2350-v0.2-panelized.kicad_pcb
```
- Export to JLC



