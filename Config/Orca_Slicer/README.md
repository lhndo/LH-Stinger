 
 # Orca Slicer Printer Profiles
 
 ## Installation 

* Latest version of **Orca Slicer** can be found here: (https://github.com/SoftFever/OrcaSlicer)  
* Copy the profile folders to:   

  ```  \%appdata%\OrcaSlicer\user\default\```


### Orca Slicer:

* Select **LH Stinger** printer, open **Printer Settings** > **Set** and choose your **Texture** and **Bed Model**.
* Calibrate your **Flow Ratio**, **Pressure Advance** and **Max Volumetric Speed** flow.
* Adjust your cooling settings based your configuration.

<br>

>[!Note]
> The **bed origin** offsets are set according to the configuration documentation. This setup might be something that you are not familiar with, but should work right out of the box with the provided klipper printer.cfg.  
> Please check for [Klipper Config](https://github.com/lhndo/LH-Stinger/tree/main/Config/Klipper_Config#bed-origin) for more information.

<br>

![orca slicer](/Images/orcaslicer.png)

<br>

### Settings


The profiles contain the following Machine Start G-Code:  

`
PRINT_START_LHS BED=[bed_temperature_initial_layer_single] HOTEND=[nozzle_temperature_initial_layer] AUTOMESH=1 AUTOPURGE=1 QUIETMODE=0
`

<br>

* **AUTOMESH** - Klipper Adaptive Mesh Generation
  * if set to **0**, the "**default**" profile will be loaded instead
  * if mymacros.cfg **variable_use_multi_bed_mesh_profiles** is set to **True**, then  it will try to load the bed profile closest to the set bed temperature. (Required profiles: "default", "75", "105" )

* **AUTOPURGE** - uses a line purge feature set by default at the right front corner of the bed. Configurable settings available in the mymacros.cfg header.

* **QUIETMODE** - automatically enables the Qmode macro for quiet printing. Best used with the Quiet profile.   

<br>

*Please consult the mymacros.cfg comments for more information.*
