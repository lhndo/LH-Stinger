
## Hammer Stop Cut
*by Dan329*

Servo controlled filament cutter stopper for printers with restricted X travel

![](Images/1.png)



## BOM

Item | Quantity
-|- 
SG90 Servo or equivalent  | 1
Screw Button Head M3 12mm  | 1
Screw Countersunk M3 8mm  | 1
Screw Button Head M2/2.5 12mm+  | 1
2020 M3 T-Nut Insert   | 2



## Print

Solid

![](Images/2.png)


## Gcode
- Flip Servo is defined as **flipstop_servo** in the config  

- Replaces `_SP_TIP_FORM` in `sp_mmu.cfg`  

```
[gcode_macro _SP_TIP_FORM]   ## Filament Cut Macro
variable_cut_position_x: 230
variable_cut_position_y: -99  ## -99 means no movement
variable_clear_position_x: 200 
variable_clear_position_y: -99 ## -99 means no movement
variable_approach_speed: 400
variable_cut_speed: 140
variable_filament_retract_speed: 130
variable_z_hop: 0.4

gcode:  
  {% set sp = printer['gcode_macro _SP_VARS'] %}
  {% set hop = [printer.toolhead.axis_maximum.z, printer.gcode_move.gcode_position.z+z_hop] | min  %} 
  {% set fil_cut_position = sp.dist_extruder_to_meltzone - sp.dist_filament_park - sp.tip_length_below_cut %}
  
  RESPOND MSG="SP: Filament Cut"

  {% if not "xy" in printer.toolhead.homed_axes %}
    G28 X Y
  {% endif %}
  G90
  M83

  {% if "z" in printer.toolhead.homed_axes and z_hop > 0 %}
    G0 Z{hop}    ## Z Hop
  {% endif %}
  

  {% set clear_move = "" %}
  {% if clear_position_x != -99 %} {% set clear_move = clear_move + 'X' ~ clear_position_x ~ ' ' %} {% endif %}
  {% if clear_position_y != -99 %} {% set clear_move = clear_move + 'Y' ~ clear_position_y ~ ' ' %} {% endif %}

  {% set cut_move = "" %}
  {% if cut_position_x != -99 %} {% set cut_move = cut_move + 'X' ~ cut_position_x ~ ' ' %} {% endif %}
  {% if cut_position_y != -99 %} {% set cut_move = cut_move + 'Y' ~ cut_position_y ~ ' ' %} {% endif %}

  G0 {clear_move} F{60*approach_speed}                      ## Approach clear position
  
  SET_SERVO SERVO=flipstop_servo ANGLE=90                   ## Servo In
  G4 P1200
  
  G0 E-{fil_cut_position} F{60*filament_retract_speed}      ## Retract filament close to the cutter blade
  G0 {cut_move} F{60*cut_speed}                             ## Move to cut
  G0 {clear_move} F{60*cut_speed}                           ## Move back to clear position
  
  M400
  SET_SERVO SERVO=flipstop_servo ANGLE=0                    ## Servo Out
  G4 P1200
  SET_SERVO SERVO=flipstop_servo WIDTH=0


```