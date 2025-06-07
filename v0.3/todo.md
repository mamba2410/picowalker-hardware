# Todo before sending off for fab

## Parts

- Consolidate pull-ups to be a single value e.g. 10k or 5.1k, maybe also set ILIM to 5.1k for one less value.
- Create battery connector part
- Double check size of button contacts (currently 5mm diameter)

## Layout

- Shift MCU and PMIC left a bit to fit the PMIC in the bottom right corner.
- Reroute USB lines to not be in the membrane connector pad.
- Probably have a smaller flash chip to fit PMIC in the bottom right (W25Q128JV[E/P]IM for 6x5 or 8x6 mm WSON).
- Add copper zones in PMIC group again.
- Add fills in power layer to route battery and VSYS between PMIC and battery/3.3V reg.
- Move pins on MCU again to have UART on left and I2C on right.

## Checks

- Assign and check part numbers and that they are in stock on mouser/partsbox
- Stitching vias
- Panelise
```
kikit panelize -p rp2350-v0.2-kikit.json .\rp2350-v0.2.kicad_pcb outputs\panelized\rp2350-v0.2-panelized.kicad_pcb
```
- Export to JLC



