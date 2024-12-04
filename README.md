# Cartographer3d-FLsun-V400
Cartographer3d probe configuration files for FLSUN V400

Cartographer3d probably wasn't designed with delta printers in mind, but with a little work I was able to make it work.
Before you start consult the official documentation carefully.
Remember that Cartographer touch survey needs a metal bed under the probe to work properly.
I use the Guilouz Klipper version as base, link: https://github.com/Guilouz/Klipper-Flsun-Speeder-Pad/wiki

Once the probe is properly installed, do not launch the machine configuration macros, but first make sure the probe correctly reads the metal bed below it with command CARTOGRAPHER_QUERY.

Remember to update the "delta_calibrate" and "bed_mesh" sections by updating the radius with a radius that allows the probe to read the plate, the standard radius is 147 but to work with this radius you need a 350mm radius plate (I bought it), with the standard 310 plate use a 120 radius.

Check if proble read the bed with command "CARTOGRAPHER_QUERY", remember that the probe is set back from the nozzle, check that the coil has the metal plate underneath it.
Controls the limit positions of the printhead by moving it to different positions and probing the probe reading.

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
