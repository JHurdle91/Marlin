# Marlin 2.0

## Printer: Hictop_i3_3DP12

The purpose of this fork is to keep track of the firmware on my printer and any other notes I may have on it.

Branched from Marlin `bugfix-2.0.x`

### Source:

[Configuring Marlin 1.1](http://marlinfw.org/docs/configuration/configuration.html#configuring-marlin-1.1)

### Note:

I'm installing Marlin 2.0, but plan to loosely follow the official 1.1 guide because that's what's available.

These sections are ordered just as they are in `configuration.h`, but I am skipping sections that are not relevant to my setup.

**Focus on two main files:**

* `Configuration.h` (core settings)
* `Configuration_adv.h` (optional advanced settings)

**Pre-built configs** available in Marlin's `example_configurations` directory.

## Calibration

### _**Placeholder**_

*Will come back to this section with notes on whatever calibration is needed.*

## Prerequisites

You need to know the following about your printer:

* Printer style
    * **Cartesian**
    * Delta
    * CoreXY
    * SCARA
* Driver board
    * **RAMPS**
    * RUMBA
    * Teensy
    * etc...
* Number of extruders
    * **1**
* Steps/mm for XYZ axes and extruders (can tune later)
    * X: 80
    * Y: 80
    * Z: 2560 steps/mm
    * Extruder: 94.5 **?**
* Endstop positions
    * X: **?**
    * Y: **?**
    * Z: **?**
* Thermistors and/or thermocouples
    * 100K ohm NTC Thermistor
* Probes and probe settings
    * Inductive z-probe
* LCD controller brand and model
    * Ramps v1.4 LCD2004
* Add-ons and custom components

# `Configuration.h`

Some settings can**not** be changed after flashing the firmware.

Some settings can be changed and saved to **EEPROM**, while fewer still can be changed from the **LCD controller**

## Baud Rate

This is the serial communication speed of the printer. Start with `250000`. Only go lower if you get `line number` or `checksum` errors.

### Allowed Baud Rate Values

* 2400
* 9600
* 19200
* 38400
* 57600
* 115200
* **250000**

## Motherboard

Refer to `boards.h` to find your specific board.

**BOARD_RAMPS_14_EFB**

* Ramps 1.4
* Powers **E**xtruder, **F**an, and **B**ed.

## Custom Machine Name

_**Always**_ give a unique machine name for each flash. This will make it much easier to keep track of which file was flashed. The LCD will display "`Machine Name` is ready"

I made the mistake before of keeping the machine name consistent between different `configuration.h` files. It ended in me having no idea which config file was actually running on my printer. 

For this version, I named the machine `Scuzzlebutt`

![Scuzzlebutt](./pics/Scuzzlebutt.png)

## Machine UUID

    #define MACHINE_UUID "00000000-0000-0000-0000-000000000000"

A unique ID seems to only be necessary if working with multiple printers on the same network. Generate a random ID at [uuidgenerator.net](http://www.uuidgenerator.net/version4)

## Extuder(s)

`#define EXTRUDERS 1` defines number of extruders. Can be 1-4.

Refer to the source guide for more options if you plan to use multiple extruders.

### Filament Diameter

Set the "nominal" filament diameter.

Use **1.75mm** even if you measure the diameter to be 1.70mm.

## Power Supply

