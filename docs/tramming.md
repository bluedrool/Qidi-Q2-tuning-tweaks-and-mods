# Have an easy time tramming the Q2 bed with screws_tilt_adjust. 

This Youtube video has all the instruction you need:

[![This video has all the instruction you need](https://img.youtube.com/vi/APAbl5PGEh0/0.jpg)](https://www.youtube.com/watch?v=APAbl5PGEh0)

This is what you have to paste to printer.cfg:

[screws_tilt_adjust]

```
screw1:30,30
screw1_name: front left screw

screw2: 240,30
screw2_name: front right screw

screw3: 240,240
screw3_name: rear right screw 

screw4: 30,240
screw4_name: rear left screw

horizontal_move_z: 10
speed: 50
screw_thread: CW-M4 
```

You need also 7mm socket for the locking nut and pliers to hold the knob while you tighten the locking nut. Otherwise it is very easy to accidentally turn the knob while you tighten the nut. 

Got to 0.20 variance at 60c. 
