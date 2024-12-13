 
 [:arrow_double_down: Download Folder](https://download-directory.github.io/?url=https%3A%2F%2Fgithub.com%2Flhndo%2FLH-Stinger%2Ftree%2Fmain%2FConfig%2FOrca_Slicer)
 
 # Orca Slicer Printer Profiles
 
 ## Installation 

* Latest version of **Orca Slicer** can be found here: (https://github.com/SoftFever/OrcaSlicer)  
* Copy the profile folders to the user folder found at the location below:   

  * Windows: ```  \%appdata%\OrcaSlicer\user\default```  

  * Linux: ```  ~/.config/OrcaSlicer```  
  
  * macOS: ```  ~/Library/Application Support/OrcaSlicer```  


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
PRINT_START_LHS BED=[bed_temperature_initial_layer_single] HOTEND=[nozzle_temperature_initial_layer] AUTOMESH=1 AUTOPURGE=1 QUIETMODE={if print_preset =~ /.*Quiet.*/ }1{else}0{endif}
`

<br>

* **AUTOMESH** - Klipper Adaptive Mesh Generation
  * if set to **0**, the "**default**" profile will be loaded instead
  * if mymacros.cfg **variable_use_multi_bed_mesh_profiles** is set to **True**, then  it will try to load the bed profile closest to the set bed temperature. (Required profiles: "default", "75", "105" )

* **AUTOPURGE** - uses a line purge feature set by default at the right front corner of the bed. Configurable settings available in the mymacros.cfg header.

* **QUIETMODE** - automatically enables the Qmode macro for quiet printing. Automatically detects if the "Quiet" profile is used.   

<br>

*Please consult the [LH Stinger Wiki / Macros](https://github.com/lhndo/LH-Stinger/wiki/Macros) for more information.*
