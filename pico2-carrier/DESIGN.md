# Pico 2 Carrier board design

Already know that eeprom and accelerometer work.
Screen mostly works, but trying to eliminate interference by removing the jumper wires.

Heavily cutting unnecessary pins since we're very limited on the Pico 2 and preferably want to 
keep using the RP2350A version since the B version is physically much larger.

## Design goals

- Keep under the Pico 2 pin limit with minimal jumpers
- Separate battery and rest of board if charge circuit is bad
- Headers for screen in case the AXE534-127 connector is wired wrong
- Preferably 2-layer board but power management needs 4 layers
- Only single-sided components for ease of assembly

## Goals

On top of getting something working and broadly verifying 

- Generally verify schematics
- Get it portable
    - Reliable(?) step detection
- See if battery charging works
    - Needs lots of testing/verification (charge lim, values ok, EMI ok etc)
- Get screen more stable
- See if flash works
- See if PIO IrDA works
- Measure power consumption (from USB VBUS and battery/PMIC output)
    - Identify the most power-hungry things in the board
    - See how low rp2350 can go
    - See how low screen can go
- Check out power saving benefits
    - IrDA `IR_SD`
    - Screen `PWR_EN`
    - Putting things in sleep mode
    - Underclocking rp2350
- Identify pins that aren't strictly needed, so they can be cut in final version
    - `~{SCREEN_RST}` - software might be good enough
    - `~{BAT_CE}` - Software can disable charging even if this is asserted
    - `DBG_XX` - USB stdio works but these might be most useful
        (could disable transceiver and use IrDA pins as software-uart when not in connect mode)
- Using flash-as-eeprom
- Using pico as USB device

Things this WONT help out with

- Sound (had to cut that out because of pin space requirements)
- rp2350 power supply
- Converting VSYS to 3.3V
- Ditching separate flash for unified code/data flash
- USB-C connector, ESD protect and integration
- Fitting in form-factor/enclosure
    - And thermals

## Notes

### BQ25628E Battery charger PMIC

For the battery charging, The BQ25628E chip seems good and small enough to be used in the final version.
There is another chip in the line that's due to be out in the future that's smaller and cheaper 
but this one seems fine enough.

I'm trying to follow the reference design (what bits I can find in the datasheet) as closely as possible
since I'm not knowledgeable enough to deviate from it.
It might need a 4 layer board to have good grounding since there are a lot of crossing wires, especially
for the VBUS/VSYS and interface with the MCU.


### Passives

BQ25628E

- CVBUS 10V 1uF before derating
- CPMID 10V 10uF before derating
- CSYS 10V 20uF before derating
- CBAT 10V 10uF before derating
- LSW max 2.2uH
- CREGN 10V 4.7uF ceramic before derating
- CBTST 10B 47nF ceramic before derating
- Output capacitor 10V >=10uF ceramic X7R or X5R before derating
- Input capacitor (CPMID?) 25V 10uF ceramic X7R or X5R before derating


TFDU4101

- CVIRED rec. tantalum/fast rise 4.7uF/16V (assume before derating). Used as bulk/buffer
- CVCC rec. ceramic 0.1uF. Used in low-pass filter

M95512

- Decoupling capacitor between 10nF-100nF (assume before derating)

BMA400

- two decoupling capacitors 100nF (assume before derating)

W25Q128/IS25LP128

- No mention of capacitors

DO0180FMST08

- No mention of capacitors

