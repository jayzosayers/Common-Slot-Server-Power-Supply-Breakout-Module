# Common Slot Server Power Supply Breakout Board
## Introduction
This is a breakout module designed to make custom backplanes for common slot power supplies. The PCB is simple in design, and allows you to access the 12V from any compatible server power supply via four pairs of convenient terminals, and provides a breakout of the data pins to be able to communicate with the supply via a microcontroller or other methods.

I designed this module to help me design a custom backplane for a DIY power supply featuring common slot power supplies. I hope others find this useful, either for making their own backplane like me, or for a quick, dirty way of making a module for converting the edge connector into a convenient connector and/or place to solder two wires.

**Note:** The PCB alone will not enable the outputs. See below on how to enable the 12V outputs.
![Assembled PCB](https://i.ibb.co/xDVyD26/DSC-0758.jpg)

## Things you'll need
- Soldering Iron
- 1x PCB (JLCPCB are fast, cheap, and high quality if you are looking for suggestions for places to have them made. The photos here are PCBs made by them) Download the repo for the gerber files.
- 4x Anderson Powerpole PP30 PCB connectors (optional, you can use any connector that fits the footprints, or alternativey just push wires through the holes and solder them directly)
- 2.54mm (0.1") pitch header, 2 x 6 pins, any gender.
- A compatible PSU (see list below or check to see if your PSU uses the same pinouts)
- 1x 64-pin Edge connector slot (Known compatible part numbers: WingTat `S-64M-2.54-5` (Vertical), or WingTat `S-64L-2.54-5` (Right Angle), you can get these off [Aliexpress](https://www.aliexpress.com/item/32971743485.html?spm=a2g0o.9042311.0.0.582f4c4diTB80n))

## Assembly
1. Solder the Edge Connector to the reverse of the board. You'll see the words "PSU OTHER SIDE" and "PSU THIS SIDE" above the footprint for the edge connector. The side with "PSU OTHER SIDE" is the front, and as suggested the Edge connector should be on the other side, so that looking at the front of the PCB, the power supply connects from behind. See the fully assembled photos for reference. This is the only part that has a mandatory orientation as the pinout here matters.
![Unassembled PCB](https://i.ibb.co/MDgb1yY/DSC-0755.jpg) ![Correct Edge connector alignment](https://i.ibb.co/k1z3gsC/DSC-0756.jpg)
3. Solder the Header pins to the board. There's sufficient clearance behind because of the Edge connector to allow a ribbon cable to be connected in that space. So long as you take note of which pins are which, you can solder header to either side, depending on your requirements.
4. Finally, solder your chosen connector or wires to the main output pins. The PCB is designed for Anderson PP15/30/45 connectors, however so long as you can physically solder it to the footprint, you can use practically anything.

## Enabling the Output
There's three ways you can enable output, the "hack" way, the "proper" way, and through Software. This guide will not cover the software side as I'm still investigating how you'd do it.

### The "Hack" Method
Connect a resistor (you can just short it however it's better to use a resistor) between Pin 33 (`EN`) and Pin 36 (`PRE`). If you're looking for a quick and dirty way to enable the output, this is the easiest method.

### The "Proper" Method
By some co-incidence, connecting pins 33 and 36 balances the voltages on the two pins in such a way as to satisfy the requirments to enable the output.

The way these power supplies are designed to be turned on is by pulling Pin 36 (`PRE` aka PRESENT) up to Pin 37 (`12VSB` aka 12V standby). The `12VSB` pin is a low current always on 12V output designed to provide standby power to a control system. If you're using a microcontroller to switch the PSU, I'd power it using this pin.

Next, you pull Pin 33 (`EN` aka #ENABLE) down to the supply `GND`. There's a handy signal GND pin which is Pin 30.

## Pinout
```
01-13, 52-64 12V:   Main Power +12V
14-26, 39-51 GND:   Power Ground
27           ADR:   PMBus Address
28           NC:    Not Used
29           NC:    Not Used
30           GND:   Signal Ground
31           SCL:   PMBus/SMBus/I2C CLock
32           SDA:   PMBus/SMBus/I2C Data
33           EN:    #Enable (Pull Low to Enable main output)
34           IMON:  Current Monitor
35           PSOK:  PSU OK Signal
36           PRE:   Present (Pull High to Enable main output)
37           12VSB: 12V Standby, always on, low current output.
38           PSIN:  PSU Alarm Signal
```

## Known Compatible PSUs.
In theory, all "common slot" PSUs should be compatible, however I've included a list of known working PSUs. This list is not exhaustive. This module is only compatible with supplies using the edge connector or "gold finger" connector.
- DPS-750RB
- DPS-1200FB

## PCB Schematic
![Schematic](https://i.ibb.co/WtMRFzG/Schematic.jpg)

## PCB Design Screenshot
![PCB Design Screenshot](https://i.ibb.co/YT5NgW8/PCB.jpg)

## Credits and Further Reading
This project would not be possible without the previous work of many others. You can additionally read more into the proper usage of the data pins in the sources linked below.

### Colintd
Colin is one of the people who worked out the [pinouts of the PSUs](https://colintd.blogspot.com/2016/10/hacking-hp-common-slot-power-supplies.html)

### Raplin
Raplin has produced a detailed teardown of the DPS-1200FB module which has been documented [here](https://github.com/raplin/DPS-1200FB)

### Slundell
Slundell also has a pinout for the devices, plus has more info relating to the data that can be pulled via PMBus/SMBus/I2C using the two data pins.
https://github.com/slundell/dps_charger

### Datasheet for a similar compatible supply
This datasheet also covers pinouts and explains them in greater detail. It's not for an HP DPS-750 or DPS-1200 Power supply but they apparently use a compatible pinout. https://www.murata.com/products/productdata/8807027081246/d1u86p-w-1600-12-hbxdc.pdf?1583754811000
