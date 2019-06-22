# Marlin 2.0

## Printer: Hictop_i3_3DP12

The purpose of this fork is to keep track of the firmware on my printer and any other notes I may have on it.

Branched from Marlin `bugfix-2.0.x`.

### Source:

[Configuring Marlin 1.1](http://marlinfw.org/docs/configuration/configuration.html#configuring-marlin-1.1)

### Note:

I'm flashing Marlin 2.0, but will follow the official 1.1 guide because that's what's available.

These sections are ordered just as they are in `configuration.h`, but I am skipping sections where the default value is correct, and sections that are not relevant to my setup.

So far, I'm only concerned with `configuration.h`. For more detailed customization options, see the source guide linked above.

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

Some settings can be changed and saved to **EEPROM**, while fewer still can be changed from the **LCD controller**.

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

_**Always**_ give a unique machine name for each flash. This will make it much easier to keep track of which file was flashed. The LCD will display "`Machine Name` is ready".

I made the mistake before of keeping the machine name consistent between different `configuration.h` files. It ended in me having no idea which config file was actually running on my printer. 

For this version, I named the machine `Scuzzlebutt`.

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

## Thermal Settings

`#define TEMP_SENSOR_0 1` (hotend)

`#define TEMP_SENSOR_BED 1` (bed)

Come back to this section if temperature readings are drastically off.

### PID

#### Hotend PID

Enable `PID_AUTOTUBE_MENU` to add an autotune cycle option to the LCD menu.

Added PID values for the **Hictop i3** from some [random old forum post](https://reprap.org/forum/read.php?406,656276).

```C++
// HICTOP i3
#define  DEFAULT_Kp 11.95
#define  DEFAULT_Ki 0.49
#define  DEFAULT_Kd 72.55
```

#### Bed PID

Disable for now. Come back to this section if there are issues.

## Safety

### Prevent Lengthy Extrude

Reduced `EXTRUDE_MAXLENGTH` to **50**. (Default was 200)

## Movement

Leave default for now

## Z Probe

    #define X_PROBE_OFFSET_FROM_EXTRUDER 10  // X offset: -left  +right  [of the nozzle]
    #define Y_PROBE_OFFSET_FROM_EXTRUDER 10  // Y offset: -front +behind [the nozzle]
    #define Z_PROBE_OFFSET_FROM_EXTRUDER 0   // Z offset: -below +above  [the nozzle]

The **Z offset** should be as exact as possible. Can be overridden with `M851 Z` or LCD controller. Save to EEPROM with `M500`.

## Motor Direction

If possible, set these before belt is attached, to prevent damage.

Starter values:
    
    // @section machine

    // Invert the stepper direction. Change (or reverse the motor connector) if an axis goes the wrong way.
    #define INVERT_X_DIR false
    #define INVERT_Y_DIR true
    #define INVERT_Z_DIR false

    // @section extruder

    // For direct drive extruder v9 set to true, for geared extruder set to false.
    #define INVERT_E0_DIR false
    #define INVERT_E1_DIR false
    #define INVERT_E2_DIR false
    #define INVERT_E3_DIR false
    #define INVERT_E4_DIR false
    #define INVERT_E5_DIR false

## Movement Bounds

Set bed size and XYZ limits:
    
    // @section machine

    // The size of the print bed
    #define X_BED_SIZE 205
    #define Y_BED_SIZE 270

    // Travel limits (mm) after homing, corresponding to endstop positions.
    #define X_MIN_POS 0
    #define Y_MIN_POS 0
    #define Z_MIN_POS 0
    #define X_MAX_POS X_BED_SIZE
    #define Y_MAX_POS Y_BED_SIZE
    #define Z_MAX_POS 160

## Bed Leveling

### Style

Below bullets copy/pasted from source.

With Bed Leveling enabled:

* `G28` disables bed leveling, but leaves previous leveling data intact.
* `G29` automatically or manually probes the bed at various points, measures the bed height, calculates a correction grid or matrix, and turns on leveling compensation. Specific behavior depends on configuration and type of bed leveling.
* `M500` saves the bed leveling data to EEPROM. Use `M501` to load it, `M502` to clear it, and `M503` to report it.
* `M420 S<bool>` can be used to enable/disable bed leveling. For example, `M420 S1` must be used after `M501` to enable the loaded mesh or matrix, and to re-enable leveling after `G28`, which disables leveling compensation.
* A “Level Bed” menu item can be added to the LCD with the `LCD_BED_LEVELING` option.

For now, I've gone with `AUTO_BED_LEVELING_BILINEAR`. Good for large or uneven beds.

## SD Card

Enable `#define SDSUPPORT`

