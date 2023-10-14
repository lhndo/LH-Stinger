


### *This Klipper configuration is set up for the following wiring setup*
Using the parts listed in the [BOM]( https://docs.google.com/spreadsheets/u/2/d/1s8ulLfThmbuy1G_40MvkXXL2oVx9PZhvpAY9hMxqYbg/edit?usp=drive_link)  <br>

![Ebox Wiring](ebox_wiring.png)  

## Contents:
<!--ts-->

- [Prerequisites](#prerequisites)
    - [BTT PI 1.2](#btt-pi-12)
    - [KIAUH](#kiauh)
    - [Octopus PRO Firmware](#octopus-pro-firmware)
    - [xz\_dockable\_probe](#xz_dockable_probe)
    - [ResHelper](#reshelper)
    - [KAMP](#kamp)
- [Klipper Configuration](#klipper-configuration)
- [Bed Origin](#bed-origin)
- [Slicer Start Gcode](#slicer-start-gcode)
  - [Orca Slicer](#orca-slicer)
  - [Prusa Slicer](#prusa-slicer)
<!--te-->
<br>

# Prerequisites

It is recommended to follow the installation in the order listed below

### BTT PI 1.2
OS image: https://github.com/bigtreetech/CB1/releases (Same as CB1)   

### KIAUH

https://github.com/dw-0/kiauh/tree/master

### Octopus PRO Firmware
https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-V1.0/tree/master/Firmware/Klipper#build-firmware-image

### xz_dockable_probe

[Annex-Engineering_User_Mods/Extruders/Sherpa_Mini/Toolheads/Churls-Stiffy_E3
/QuickDraw_klipper_config.cfg](https://github.com/churls5495/Annex-Engineering_User_Mods/blob/main/Extruders/Sherpa_Mini/Toolheads/Churls-Stiffy_E3/QuickDraw_klipper_config.cfg)

### ResHelper

https://github.com/lhndo/ResHelper

### KAMP

https://github.com/kyleisah/Klipper-Adaptive-Meshing-Purging/tree/main

<br>

# Klipper Configuration

* Copy the files from this folder to: **/home/biqu/printer_data/config**

* Open printer.cfg and comment/uncomment the lines in the header according to the instructions if needed. 
<br>
<br>

For more information please consult: https://www.klipper3d.org/Installation.html


For support please join the Klipper Discord: https://discord.klipper3d.org/

<br>

# Bed Origin

The printer supports multiple bed sizes. For ease of use, the bed position is defined in the slicer by changing the Origin Offsets, and not in Klipper's printer.cfg. 

Move the nozzle manually to the front left corner of the bed, and note the position, eg. X18 Y38.  
Add those values in the Origin section as negative values. 

![Slicer Bed Settings](slicer_bed.png)

<br>

# Slicer Start Gcode

## Orca Slicer

In Printer > Machine G-Code set:  

Machine Start G-Code:  

`PRINT_START BED=[bed_temperature_initial_layer_single] HOTEND=[nozzle_temperature_initial_layer] AUTOMESH=1 AUTOPURGE=1`

Machine End G-Code: 

`PRINT_END`  

<br>

## Prusa Slicer

In Printer Settings > Custom G-Code set:  

Start G-Code:  

`PRINT_START BED=[first_layer_bed_temperature] HOTEND=[first_layer_temperature[initial_extruder]] AUTOMESH=1 AUTOPURGE=1`

End G-Code: 

`PRINT_END`
