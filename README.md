# Cartographer3d-FLsun-V400
Cartographer3d probe configuration files for FLSUN V400.

Cartographer firmware 5.1.

Klipper version v0.12.0-396-gb7233d11

Cartographer3d probably wasn't designed with delta printers in mind, but with a little work I was able to make it work.
Before you start consult the official documentation carefully.
Remember that Cartographer touch survey needs a metal bed under the probe to work properly.
I use the Guilouz Klipper version as base, link: https://github.com/Guilouz/Klipper-Flsun-Speeder-Pad/wiki

> :warning: The setup work is not finished yet, there are still some small problems with the automatic offset calculation, before launching a print make sure you are close to the 3d printer and correctly set the z-offset to a conservative value not to damage the printing plate

Once the probe is properly installed, do not launch the machine configuration macros, but first make sure the probe correctly reads the metal bed below it with command CARTOGRAPHER_QUERY.

> :warning: Remember to update the "delta_calibrate" and "bed_mesh" sections by updating the radius with a radius that allows the probe to read the metal bed, the standard radius is 147 but to work with this radius you need a 350mm radius plate (I bought it), with the standard 310 plate use a 120 radius or minus.

Check if proble read the bed with command "CARTOGRAPHER_QUERY", remember that the probe is set back from the nozzle, check that the coil has the metal plate underneath it.
Controls the limit positions of the printhead by moving it to different positions and use command "CARTOGRAPHER_QUERY".

Cartographer output query must be like this:
```
Last reading: 3055985.84Hz, 21.11C, 2.32431mm
```
If the probe are outside of the metal bed the output is like this:
```
Last reading: 3003686.43Hz, 21.20C, infmm
```

Radius values into printer.cnf
```
[delta_calibrate]
radius: 147 #120 #147
horizontal_move_z: 10 #30
speed: 100

[bed_mesh]
speed: 300
horizontal_move_z: 5
mesh_radius: 120 #precartographer
mesh_origin: 0,0 #0,0
round_probe_count: 7
algorithm: bicubic
mesh_pps: 2,2
zero_reference_position: 0,0
```

Once the probe is properly installed, you can proceed to calibration.

> :warning: Always stay close to the printer so you can push the emergency stop if something should go wrong.

This is the sequence I use for calibration:

Import or edit macro and printer configuration file using my files as reference

Home the 3d printer
Edit set backlash to 0.5 (default value) into scanner section of printer.cnf file
launch command 
```
SET_GCODE_OFFSET Z=0
```
launch command
```
SAVE_CONFIG or SAVE button macro (v400 Guilouz)
```
Home the 3d printer
launch command 
```
G1 Z10 F1500
```
//Calibration cartographer
launch command 
```
CARTOGRAPHER_CALIBRATE METHOD=manual (i use a feeler gauge 0.2mm)
```
Once finished remove the paper/gauge and click "Accept" the position.
launch command
```
SAVE_CONFIG or SAVE button macro (v400 guilooz)
```
Home the 3d printer
launch command
```
G1 Z15 F1500
PROBE_ACCURACY
CARTOGRAPHER_ESTIMATE_BACKLASH
```
Takenote of value delta and move from center to left column and launch commands
```
G1 Y-50 F6000
G1 X-115 F6000
CARTOGRAPHER_ESTIMATE_BACKLASH
```
Takenote of value delta and move to back calumn and launch commands
```
G1 Y-120 F6000
G1 X0 F6000
CARTOGRAPHER_ESTIMATE_BACKLASH
```
Takenote of value delta and move to right column and launch commands
```
G1 Y-60.00 F6000
G1 X115 F6000
CARTOGRAPHER_ESTIMATE_BACKLASH
```
Calculate media of all 4 values and update backlash value in "scanner" section into printer.cfg file.
Launch commands
```
SAVE and restart
BED_MESH_CALIBRATE
SAVE and restart
G1 Z15 F1500
CARTOGRAPHER_THRESHOLD_SCAN 
SAVE_CONFIG
G1 Z10 F1500
CARTOGRAPHER_CALIBRATE 
SAVE_CONFIG
 ```
//end carto calibration

//calibrate v400
launch commands
```
G1 Y0 F6000
G1 X0 F6000
G1 Z10 F1500
CARTOGRAPHER_TOUCH
ENDSTOPS_CALIBRATION
SAVE_CONFIG
DELTA_CALIBRATION
SAVE_CONFIG
BED_LEVELING
SAVE_CONFIG
CARTOGRAPHER_TOUCH
SAVE_CONFIG
PID_BED TEMP=65
SAVE_CONFIG
PID_HOTEND TEMP=220
SAVE_CONFIG
```
//end calibration v400

> :warning: Remember to set z-offset value adding at least 0.15 mm, just use the video below as an example:

https://docs.cartographer3d.com/cartographer-probe/installation-and-setup/installation/first-print
