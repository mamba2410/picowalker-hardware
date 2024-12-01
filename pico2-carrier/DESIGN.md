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

## Datasheets and documentation

- Pico 2
    - [Pico 2 Datasheet](https://datasheets.raspberrypi.com/pico/pico-2-datasheet.pdf)
    - [RP2350 Datasheet](https://datasheets.raspberrypi.com/rp2350/rp2350-datasheet.pdf)
    - [RP2350 Hardware Design](https://datasheets.raspberrypi.com/rp2350/hardware-design-with-rp2350.pdf)
- M95512
    - [Datasheet](https://www.st.com/en/memories/m95512-w.html#documentation)
    - [Documentation](https://www.st.com/en/memories/m95512-w.html#documentation).
- BQ25628E
    - [Product Page](https://www.ti.com/product/BQ25628E)
    - [Datasheet](https://www.ti.com/lit/ds/symlink/bq25628e.pdf)
- W25Q128JV
    - [Product Page](https://www.winbond.com/hq/product/code-storage-flash-memory/serial-nor-flash/?__locale=en&partNo=W25Q128JV)
    - [Datasheet](https://www.mouser.com/datasheet/2/949/w25q128jv_revf_03272018_plus-1489608.pdf)
- BMA400
    - [Product Page](https://www.bosch-sensortec.com/products/motion-sensors/accelerometers/bma400/)
    - [Datasheet](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bma400-ds000.pdf)
- TFDU4101
    - [Product Page](https://www.vishay.com/en/product/81288/)
    - [Datasheet](https://www.vishay.com/docs/81288/tfdu4101.pdf)
- DO0180FMST03
    - [Product Page (Module)](https://www.dwo.net.cn/pd.jsp?id=11924#_jcp=3_38)
    - [Product Page (Panel)](https://www.dwo.net.cn/pd.jsp?fromColId=2&id=11921#_pp=2_322)
    - [Module Datasheet](https://18746902.s21i.faimallusr.com/61/1/ABUIABA9GAAgivThrgYols7tsgE.pdf)
    - [Panel Datasheet](https://18746902.s21i.faimallusr.com/61/1/ABUIABA9GAAgm-3hrgYoq5PDogM.pdf)
    - [Driver Datasheet](https://18746902.s21i.faimallusr.com/61/1/ABUIABA9GAAgzvThrgYo8pyQBw.pdf)
    - [Touch Datasheet](https://18746902.s21i.faimallusr.com/61/1/ABUIABA9GAAg0-ThrgYosMjsrAY.pdf)

## Review

### EEPROM

According to ST's application note AN2014, the M95512 needs a (10k) pull-up on
the CSB line and strongly recommends a (100k) pull-down on the SCK line.
This then forces the SPI bus to be run in SPI mode 0.
It also strongly recommends a 100k pull-down on the D line (MOSI).
[Documentation](https://www.st.com/en/memories/m95512-w.html#documentation).

### PMIC

The PFET seemed to not work when OR'ing into the power circuit.
Removing it, then removing the Schottky diode on the pico, and shorting the
source and drain got the battery charging and power path to work.
Probably got the electrical design wrong since the pinouts seem correct.
But looking at the hardware design in the pico 2 datasheet, I have it correct.
Maybe the pull-down on VBUS is what's causing the PFET to work wrongly.

The SYS and PG signals seem to work well enough as LEDs.
Maybe don't need the PG signal, so can leave it floating, but no reason not to
have both.
No need to have them routed to the MCU since can read the status over I2C.

Battery charging works after the PFET situation was resolved.
Power-path also works.

ADC works, as well as interrupts, which is great.

### Flash

Haven't tested the flash, but that apparently also needs a 10k external pull-up
on the CS pin since that needs to follow VCC on power-up.

### IrDA

After some testing, the IR works well on PIO. Very glad I noticed the footprint
needed flipping since that was correct.
PIO needs to run quickly, works when clocked by 150MHz sys PLL, though not much
room for debug printing in between.

The current-limiting resistor hasn't seemed to have blown yet so I think it
seems to be fine with the power dissipation but should be a bit more careful.

Haven't tested the shutdown yet.

### Screen

The screen just works with the shorter traces, even running at 4x scale and
running from the 48MHz USB PLL.
Haven't tested max brightness yet but its probably similar to when on the
breadboard - can't run at panel max.
Screen flickers a lot less at 48MHz but that's the core code which needs to
manage it properly.
Also means that the connector is the correct polarity.

### Accel

Again, it just worked when porting the code over.
Polling for steps worked.
Some testing with interrupts and you can change the polarity and phase of the
interrupts.
Not gotten it to trigger yet, needs some more testing, but int is not really
needed.

### Power saving

Quick testing on the joulescope shows that there's some weirdness with going to
sleep.
First sleep from boot goes down to about 5mA, then subsequent sleeps go to 2mA.
1mA of the second was from the 3.3V rail, the rest was probably from the pico 2.
Good start, but not good enough.
This was without proper CS pull-ups on the flash chip.

Lots more testing to do:

- RP2350-E9
- Screen power saving

