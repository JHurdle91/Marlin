# Marlin 2.0

## Printer: Hictop_i3_3DP12

The purpose of this fork is to keep track of the firmware on my printer and any other notes I may have on it.

### Source:

[Configuring Marlin 1.1](http://marlinfw.org/docs/configuration/configuration.html#configuring-marlin-1.1)

### Note:

I'm installing Marlin 2.0, but plan to loosely follow the 1.1 guide and experiment with the code.

Starting file is `Marlin-bugfix-2.0.x.zip`

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
    * Cartesian
    * Delta
    * CoreXY
    * SCARA
* Driver board
    * RAMPS
    * RUMBA
    * Teensy
    * etc...
* Number of extruders
* Steps/mm for XYZ axes and extruders (can tune later)
* Endstop positions
* Thermistors and/or thermocouples
* Probes and probe settings
* LCD controller brand and model
* Add-ons and custom components

# `Configuration.h`

Some settings can**not** be changed after flashing the firmware.

Some settings can be changed and saved to **EEPROM**, while fewer still can be changed from the **LCD controller**

