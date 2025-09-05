# Installing Klipper TMC Autotune

Extension web page: 

https://github.com/andrewmcgr/klipper_tmc_autotune

Qidi Q2 comes with an unknown version of this so I first renamed old files. 

```
cd
mv klipper/klippy/extras/autotune_tmc.py klipper/klippy/extras/autotune_tmc.py.old
mv klipper/klippy/extras/motor_constants.py klipper/klippy/extras/motor_constants.py.old
mv klipper/klippy/extras/motor_database.cfg klipper/klippy/extras/motor_database.cfg.old
```

Then I installed current version:

```
cd
wget -O - https://raw.githubusercontent.com/andrewmcgr/klipper_tmc_autotune/main/install.sh | bash
```

Add this to moonraker.conf for automatic updates:

```
[update_manager klipper_tmc_autotune]
type: git_repo
channel: dev
path: ~/klipper_tmc_autotune
origin: https://github.com/andrewmcgr/klipper_tmc_autotune.git
managed_services: klipper
primary_branch: main
install_script: install.sh
```

I had these lines commented out in my printer.cfg:

```
#[autotune_tmc stepper_x]
#motor: qidi_x_y
#tuning_goal: auto
#sgt: 1

#[autotune_tmc stepper_y]
#motor: qidi_x_y
#tuning_goal: auto
#sgt: 1
```

So Qidi installed autotune but commented it ou. Sgt value is "sensorless homing treshold" and it's default value is 1. 

Unfortunately there are no motor values for "qidi_x_y" in the old motor_database.cfg. Qidi Q2 XY steppers are labeled "Stepping motor BJ42D29-28V27. I received the correct values from the manufacturer:

resistance: 1.4Ω
inductance:  2.6mH
holding_torque: ≥0.41Nm
max_current: Rated current:1.5A
Steps_per_revolution: 200pps
Rotor inertia: 76gcm²
```

Now add this to motor_database.cfg:

```
[motor_constants BJ42D29-28V27]
resistance: 1.4
inductance: 0.0026
holding_torque: 0.41
max_current: 1.5
steps_per_revolution: 200
```

Then in printer.cfg replace old lines with this:

```
[autotune_tmc stepper_x]
motor: BJ42D29-28V27
[autotune_tmc stepper_y]
motor: BJ42D29-28V27

#[autotune_tmc stepper_z]
#motor: 
#[autotune_tmc stepper_z1]
#motor: 

#[autotune_tmc extruder]
#motor: 
```

We will do the Z motors and extruder later when we get the values.

Now you can save printer.cfg and restart Klipper. 

Autotune should be enabled. 

Results:


I ran Shake&Tune before and after enabling 

I also printed a test print
