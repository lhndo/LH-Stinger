
## Mini Purge Shute

Tiny passive purge shute that works with the filament cutter endstop.

![](Images/1.png)



## BOM

Item | Quantity
-|- 
Aluminium Foil   | 1
PTFE OD:4mm ID:3mm  | 25mm
[A1 Mini Silicone Brush](https://s.click.aliexpress.com/e/_oCtyVVf) | Cut in half = 14mm
2020 M3 T-Nut Insert   | 2
Screw M3 35mm  | 1
Screw Button Head M3 8mm  | 2


## Print

Pick between the HF and UHF variants

![](Images/2.png)



## Assembly

Glue the foil or use aluminium tape. Add two layers on the bottom surface.  
Position it so that it sits 2mm to the right of the bed, and that the nozzle slightly pushes on the PTFE 


## Gcode
- In **sp_mmu.cfg** enable **variable_use_park** and **variable_park_purge**
- Set **variable_purge_speed** to 18 (or test other values), and **variable_park_x** slightly to the right of the center of the PTFE tube
- Disable the wipe tower in Orca Slicer

- Replace `_SP_PURGE` in `sp_mmu.cfg` and adjust the values if needed  


```
[gcode_macro _SP_PURGE]   ## Called whenever a purging action is performed
gcode:  
  {% set sp = printer['gcode_macro _SP_VARS'] %}
  {% set purge = params.PURGE|default(0) | int %}
  G90

  ## Park with Conditional Homing if enabled
  {% if sp.use_park == 1 %}
    _SP_CONDITIONAL_HOME AXIS=XY
    _SP_PAUSE_PARK SKIP_Z=1   ## Select your park moves depending on your requirements
  {% endif %} 

  ## Verify heating
  _SP_HEAT_HOTEND

  ## Fan
  {% set saved_fan_speed = printer['fan'].speed if printer['fan'] is defined else 0 %}
  M106 S0   ## Set fan speed during purge
  
  ## Purge  
  M400
  RESPOND MSG="SP: Purging {purge}mm of filament"
  G1 E{purge} F{60*(sp.purge_speed/2.4)}  ## Conversion from mm3/s to mm/s 
  G0 E-0.4 F{35*60}   ## Retract

  ## Brush
  G4 P1000 # Wait 1 second 
  {% for i in range(4) %}
      G0 X215 F{60*100}  ## Left side of the brush position
      G0 X{sp.park_x} F{60*100}
  {% endfor %}
  G0 X{sp.park_x+1} F{60*100}
  
  ## Restore Fan
  M106 S{saved_fan_speed}

```