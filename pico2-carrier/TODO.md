# TODO - Hardware

- Finalise power circuitry
    - What battery connector to use? I have LP503035 battery which uses a 2-pin JST-P male connector.
    - Assuming we can hard-wire the thermistor to something within acceptible range since we don't have a battery with a thermistor.
- Double check voltage/power requirements for components
    - Screen has `VBAT` pins which expect 3.8V (range 2.9V - 4.5V)
    - Do we really need jumpers for `IM0`/`IM1` selection? Select between QSPI mode and SPI 3-wire mode.
- Do screen circuitry
    - Assuming we can just leave `TP_VCC` floating if we aren't using it.
- Choose chip packages
- Layout PCB
