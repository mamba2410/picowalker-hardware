# Things to do in a future revision

- Consolidate pull-ups to be a single value e.g. 10k or 5.1k, maybe also set ILIM to 5.1k for one less value.


## Second build

- Change USB connector to Amphenol version
- Replace 10k with 5.1k on button debounces
- Remove 0.1u cap from flash
- Try single 4.7u caps on screen VBAT and VDDIO
- Replace I2C pullups with 5.1k
- Replace accel decouples with 4.7u
- Remove eeprom SCL pulldown
- Replace eeprom CSB pullup with 5.1k
- Replace eeprom decouple with 4.7u
- Freestyle IrDA VCC2 filter to be 4.7u cap feeding into 47R resistor, to fix filter and consolidate parts
- Replace R18 and R19 pullups with 0R resistors to see if they actually need to be limiting anything
- Replace ILIM with 5.1k
- Replace battery and VBUS decoupling with 4.7u
- Replace top half of thermistor divider with 5.1k, and bottom with 30k
- Replace PG pull with 5.1k
- Replace R1 on rp2350 input filter with either 22R or 47R, probably 47R.
    - Datasheet and hardware design guide doesn't say much on the values.
      "33R and 4.7u is adequate" is about all thye say, and that its noise sensitive.
      47R and 4.7u is probably fine.

