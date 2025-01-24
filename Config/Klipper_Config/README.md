[:arrow_double_down: Download Folder](https://download-directory.github.io/?url=https%3A%2F%2Fgithub.com%2Flhndo%2FLH-Stinger%2Ftree%2Fmain%2FConfig%2FKlipper_Config)


### *This Klipper configuration is set up for the following wiring setup*
Using the parts listed in the [BOM]( https://docs.google.com/spreadsheets/u/2/d/1s8ulLfThmbuy1G_40MvkXXL2oVx9PZhvpAY9hMxqYbg/edit?usp=drive_link)  <br>

![Ebox Wiring](/Images/ebox_wiring.png)  

## Contents:
<!--ts-->

- [Prerequisites](#prerequisites)
  - [Host](#host)
    - [BTT PI 1.2](#btt-pi-12)
    - [Raspberry PI](#raspberry-pi)
  - [SSH](#ssh)
  - [KIAUH](#kiauh)
  - [Firmware](#firmware)
    - [Octopus PRO](#octopus-pro)
    - [FYSETC Spider 3 H7](#fysetc-spider-3-h7)
- [Configuration](#configuration)
  - [Klipper](#klipper)
- [Klipper Modules](#klipper-modules)
  - [XZ Dockable Probe](#xz-dockable-probe)
  - [ResHelper](#reshelper)
  - [Resonance Holder](#resonance-holder)
- [Themes](#themes)
- [Moonraker](#moonraker)
    - [Power Relay (Optional)](#power-relay-optional)
- [Slicers](#slicers)
  - [Bed Origin](#bed-origin)
  - [Slicer Start Gcode](#slicer-start-gcode)
    - [Orca Slicer](#orca-slicer)
    - [Prusa Slicer](#prusa-slicer)
- [Support](#support)
<!--te-->
<br>

# Prerequisites


<br>

:blue_book: It is recommended to perform the installation in the order listed below.
<br>

## Host

### BTT PI 1.2
Download the OS image: https://github.com/bigtreetech/CB1/releases (Choose **CB1_Debian11_minimal** )  
Use [Balena Etcher](https://etcher.balena.io/) to burn the image onto the PI SD Card.

**Video:** [BTT Pi Overview & Installation Guide](https://www.youtube.com/watch?v=xBpkeKKwu48)

**WIFI configuration:**
1. Re-insert the SD card into your computer, open the **BOOT** drive and add your Wi-Fi credentials to **system.cfg**

<br>

### Raspberry PI

Read the following guide to install the **MainsailOS**
https://docs-os.mainsail.xyz/getting-started/raspberry-pi-os-based


**WIFI configuration:**
1. Set up your WIFI connection in the **Raspberry Pi Imager** before burning the image.  
2. Re-insert the SD card into your computer, open the **BOOT** drive and add your Wi-Fi credentials to **mainsailos-wpa-supplicant.txt**

<br>



## SSH


#### Tools
To communicate with your **Host** you will need a **SSH** and an **SFTP** client  
The easiest way is to install [**MobaXterm**](https://mobaxterm.mobatek.net/features.html) which provides both of these functions. 

#### IP



  If you pre-configured your SSID and WIFI Password correctly, or if you connect though a ethernet cable, you can try the following ways to find your Klipper IP address: 
     
  A. Open your  your ROUTER dashboard and look at the device list
  B. Open command prompt and run : `ping mainsailos.local -4` (Rraspberry Pi)
  C. Install and run [Angry IP Scanner](https://angryip.org)   

<br>

## KIAUH
**KIAUH** is a rich featured script that makes it extremely easy to perform any Klipper related software installation, setup and firmware flashing starting from a bare OS.  
**This should be the first thing to install after finishing your OS installation.**

https://github.com/dw-0/kiauh/tree/master  

KIAUH installation:


1. Install KIAUH
```
sudo apt-get install git -y
cd ~ 
git clone https://github.com/dw-0/kiauh.git
```
2. Run KIAUH 
   
   `~/kiauh/kiauh.sh`

<br>

From **KIAUH** install:
- **Klipper** (Python 3.x)
- **Moonraker**
- **Mainsail**


<br>

## Firmware

:bulb: The firmware setup process can be easily done directly from **KIAUH**

### Octopus PRO 
Follow the steps starting from "Build your own firmware" and make sure to choose the appropriate settings for your own MCU type (The BOM recommendation is the Octopus Pro V1.1 with the H723 chip)

These are also mentioned in the header of **printer.cfg** file above.

https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-V1.0/tree/master/Firmware/Klipper#build-firmware-image  
<br>



### FYSETC Spider 3 H7 

For the FYSETC board included in the LH Stinger kits please follow the following firmware flashing guide:

https://github.com/lhndo/LH-Stinger/wiki/FYSETC-S3-H7-Firmware-Guide


<br>
<br>


# Configuration

<br>

## Klipper

To set up the required Klipper LH Stinger configuration files please follow these steps:

* Download the configuration files onto the Raspberry/BTT Pi by running the following commands though **SSH**

```
rm -rf ~/lhs && git clone --filter=blob:none --sparse https://github.com/lhndo/LH-Stinger.git ~/lhs && cd ~/lhs
git sparse-checkout init --cone
git sparse-checkout set Config/Mainsail_Theme/
git sparse-checkout add Config/Klipper_Config/
git sparse-checkout add KITS/FYSETC/Klipper_Config_FYSETC/
cp -r ~/printer_data/config/ ~/printer_data/config/backup/
mkdir -p ~/printer_data/config/.theme
cp -r ~/lhs/Config/Mainsail_Theme/* ~~/printer_data/config/.theme/
```

<br>

* Copy the configuration files to their correct location with the following commands:

<br>

:large_blue_diamond: Option **A** : **LH Stinger BOM Build - BTT Octopus Pro v1.2** 

```
cp -r ~/lhs/Config/Klipper_Config/* ~/printer_data/config/
```
<br>

:large_orange_diamond: Option **B** : **FYSETC - LH Stinger - Spider H7** 

```
cp -r ~/lhs/KITS/FYSETC/Klipper_Config_FYSETC/* ~/printer_data/config/
```

<br>



For more information please consult:  
[Klipper Configuration Reference](https://www.klipper3d.org/Config_Reference.html)  
[Ebox Guide](https://github.com/lhndo/LH-Stinger/wiki/Ebox)  
[Breakout Guide](https://github.com/lhndo/LH-Stinger/wiki/Breakout-Box)  
[FYSETC Kit Guide](https://github.com/lhndo/LH-Stinger/wiki/FYSETC-Kit)  

<br>
<br>


# Klipper Modules
:children_crossing: *The following modules are required for the LH Stinger setup*


Before installing the modules below, make sure that your klipper installation is up to date.


<br>

## XZ Dockable Probe

The XZ Dockable Probe module incorporates some unique homing features and is required for the QuickDraw probe.

The following commands in the SSH console will install the module:

<pre><code>cd ~/klipper/klippy/extras/
wget https://raw.githubusercontent.com/lhndo/klipper/red/klippy/extras/xz_dockable_probe.py
cd ../..
echo "klippy/extras/xz_dockable_probe.py" >> .git/info/exclude
git update-index --assume-unchanged klippy/extras/xz_dockable_probe.py > /dev/null 2>&1
systemctl restart klipper
</code></pre>

by [Dalegaard](https://github.com/dalegaard) and [Churls](https://github.com/churls5495/Annex-Engineering_User_Mods/tree/main/Extruders/Sherpa_Mini/Toolheads/Churls-Stiffy_E3)

<br>

## ResHelper


ResHelper is an utility script that simplifies and streamlines the resonance testing process. 
Follow the installation steps from the following link or the code commands below.  
https://github.com/lhndo/ResHelper

<br>

**Installation:**
```
cd ~
git clone https://github.com/lhndo/ResHelper.git
cd ResHelper
./install.sh
```

<br>

## Resonance Holder

For debugging resonance peaks 

```
cd ~/klipper/klippy/extras/
wget https://raw.githubusercontent.com/lhndo/danger-klipper/bleeding-edge-v2/klippy/plugins/resonance_holder.py
cd ../..
echo "klippy/extras/resonance_holder.py" >> .git/info/exclude
git update-index --assume-unchanged klippy/extras/resonance_holder.py > /dev/null 2>&1
systemctl restart klipper
```
**Usage:**
Uncomment **[resonance_holder]** in **printer.cfg** and use the [VIBRATION TEST](https://github.com/lhndo/LH-Stinger/wiki/Macros#vibration-test) macro for testing.


<br>
<br>



# Themes



[**Mainsail**](https://github.com/lhndo/LH-Stinger/tree/main/Config/Mainsail_Theme)
[**Fluidd**](https://github.com/lhndo/LH-Stinger/tree/main/Config/Fluidd_Theme)




<br>

# Moonraker 

### Power Relay (Optional)
If using a Power Relay to control the AC supply to the Ebox, then add the following section to your **moonraker.conf** .  
Note: The PI should be on its own independent power supply. 

![Relay](/Images/relay.png)

<pre><code>[power Printer]
type: gpio
pin: gpiochip0/gpio262
off_when_shutdown: True
initial_state: off
restart_klipper_when_powered: True
</code></pre>

<br>

* **BTT Pi 1.2** connect though SSH, run the following commands and restart the Pi:

```
sudo sh -c 'echo "# udev rules for gpio port access through libgpiod
SUBSYSTEM==\"gpio\", KERNEL==\"gpiochip[0-4]\", GROUP=\"biqu\", MODE=\"0660\"" > /etc/udev/rules.d/60-gpiod.rules'
```
```
sudo udevadm control --reload-rules
sudo udevadm trigger
```

For more information on the **BTT Pi GPIO** pinout please consult this table: https://github.com/bigtreetech/CB1#40-pin-gpio


<br>

# Slicers



## Bed Origin

The printer supports multiple bed sizes. For ease of use, the bed position is defined in the slicer by changing the Origin Offsets, and **not in Klipper's printer.cfg** by changing position_endstop and position_min for X/Y axis.  

Move the nozzle manually to the front left corner of the bed, and note the position, eg. X18 Y38.  
Add those values in the Origin section as negative values. 

![Slicer Bed Settings](/Images/slicer_bed.png)

<br>

## Slicer Start Gcode



### Orca Slicer

If not using the provided [**LH Stinger- Orca Profiles**](https://github.com/lhndo/LH-Stinger/tree/main/Config/Orca_Slicer), then set the following G-code in Printer section:  

Machine Start G-Code:  

`
PRINT_START_LHS BED=[bed_temperature_initial_layer_single] HOTEND=[nozzle_temperature_initial_layer] AUTOMESH=1 AUTOPURGE=1 QUIETMODE={if print_preset =~ /.*Quiet.*/ }1{else}0{endif}
`

Machine End G-Code: 

`PRINT_END`  

<br>

### Prusa Slicer

In Printer Settings > Custom G-Code set:  

Start G-Code:  

`
PRINT_START_LHS BED=[first_layer_bed_temperature] HOTEND=[first_layer_temperature[initial_extruder]] AUTOMESH=1 AUTOPURGE=1 QUIETMODE=0
`

End G-Code: 

`PRINT_END`

<br>

:arrow_forward: For more information please visit: [Wiki>Macros>Slicers](https://github.com/lhndo/LH-Stinger/wiki/Macros#slicers)


<br>

# Support

For support please join the [LH Stinger Discord](https://discord.gg/EzssCfnEDS), or the [Klipper Discord](https://discord.klipper3d.org/)  

<br>