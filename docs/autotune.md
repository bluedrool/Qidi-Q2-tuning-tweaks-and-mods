# Installing Klipper TMC Autotune

Extension web page: 

https://github.com/andrewmcgr/klipper_tmc_autotune

## 1) Autotune installation 

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

## 2) Inputting stepper parameters

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

So Qidi installed autotune but commented it out. Sgt value is "sensorless homing treshold" and it's default value is 1. 

Then replace old lines with this:

```
[autotune_tmc stepper_x]
motor: BJ42D29-28V27
sgt: 1
[autotune_tmc stepper_y]
motor: BJ42D29-28V27
sgt: 1

[autotune_tmc stepper_z]
motor: 42HD2037-47
sg4_thrs: 140

[autotune_tmc stepper_z1]
motor: 42HD2037-47
sg4_thrs: 140

[autotune_tmc extruder]
motor: Q2-extruder

```

I added the sgt- and sg4_thrs-value just in case. Using previous values here.

Qidi Q2 XY steppers are labeled "Stepping motor BJ42D29-28V27 and Z steppers 42HD2037-47. I received the correct values from Qidi and the manufacturer (these values are only for reference, do not add them to config files):

```
X and Y stepper BJ42D29-28V27
resistance: 1.4Ω
inductance:  2.6mH
holding_torque: ≥0.41Nm
max_current: Rated current:1.5A
Steps_per_revolution: 200pps
Rotor inertia: 76gcm²

Z stepper 42HD2037-47
resistance: 2.0Ω
inductance: 3.4mH
holding_torque: 0.28Nm
max_current: 1.5
steps_per_revolution: 200 

Extruder stepper
resistance: 1.7Ω
inductance: 0.9mH
holding_torque: 0.1Nm
max_current: 1.2
steps_per_revolution: 200 
```

You can add these to printer.cfg or motor_database.cfg. 

I used Nano to edit motor_database.cfg:

```
cd
nano ./klipper/klippy/extras/motor_database.cfg
```

Add this to motor_database.cfg (or printer.cfg):

```
# Qidi Q2 motors

[motor_constants BJ42D29-28V27]
resistance: 1.4
inductance: 0.0026
holding_torque: 0.41
max_current: 1.5
steps_per_revolution: 200

[motor_constants 42HD2037-47]
resistance: 2.0
inductance: 0.0034
holding_torque: 0.28
max_current: 1.5
steps_per_revolution: 200

[motor_constants Q2-extruder]
resistance: 1.7
inductance: 0.0009
holding_torque: 0.1
max_current: 1.2
steps_per_revolution: 200
```

Control + O and then enter to confirm saves file. Control + X exits.

## 3) Modifying driver parameters

Then you need to modify the driver configurations according to Autotune instructions:

```
Your driver configurations should contain:

    Pins
    Currents (run current, hold current, homing current if using a Klipper version that supports the latter)
    interpolate: true
    Comment out any other register settings and sensorless homing values (keep them for reference, but they will not be active)
```

So we modify our stepper configurations from these: 

```
[tmc2240 stepper_x]
spi_software_sclk_pin:PA5
spi_software_miso_pin:PA6
spi_software_mosi_pin:PA7
spi_speed:200000
cs_pin:PC12
diag0_pin:!PB8
interpolate:true
run_current: 1.07
stealthchop_threshold:0
driver_SGT:1
#driver_SLOPE_CONTROL:2

[tmc2240 stepper_y]
spi_software_sclk_pin:PA5
spi_software_miso_pin:PA6
spi_software_mosi_pin:PA7
spi_speed:200000
cs_pin:PD2
diag0_pin:!PC0
interpolate:true
run_current: 1.07
stealthchop_threshold:0
driver_SGT:1
#driver_SLOPE_CONTROL:2

[tmc2209 stepper_z1]
uart_pin: PB7
run_current: 1.07
#hold_current: 0.17
interpolate: True
stealthchop_threshold: 9999999999
diag_pin:^PA14
driver_SGTHRS:140

[tmc2209 stepper_z]
uart_pin: PC5
run_current: 1.07
#hold_current: 0.17
interpolate: True
stealthchop_threshold: 9999999999
diag_pin:^PC1
driver_SGTHRS:140

[tmc2209 extruder]
uart_pin:THR:PC13
interpolate: True
run_current: 0.857
stealthchop_threshold: 0

```

To these:

```
[tmc2240 stepper_x]
spi_software_sclk_pin:PA5
spi_software_miso_pin:PA6
spi_software_mosi_pin:PA7
spi_speed:200000
cs_pin:PC12
diag0_pin:!PB8
interpolate:true
run_current: 1.07
#stealthchop_threshold:0
#driver_SGT:1
#driver_SLOPE_CONTROL:2

[tmc2240 stepper_y]
spi_software_sclk_pin:PA5
spi_software_miso_pin:PA6
spi_software_mosi_pin:PA7
spi_speed:200000
cs_pin:PD2
diag0_pin:!PC0
interpolate:true
run_current: 1.07
#stealthchop_threshold:0
#driver_SGT:1
#driver_SLOPE_CONTROL:2

[tmc2209 stepper_z1]
uart_pin: PB7
run_current: 1.07
hold_current: 0.17
interpolate: True
#stealthchop_threshold: 9999999999
diag_pin:^PA14
#driver_SGTHRS:140

[tmc2209 stepper_z]
uart_pin: PC5
run_current: 1.07
hold_current: 0.17
interpolate: True
#stealthchop_threshold: 9999999999
diag_pin:^PC1
#driver_SGTHRS:140

[tmc2209 extruder]
uart_pin:THR:PC13
interpolate: True
run_current: 0.857
#stealthchop_threshold: 0
```

Now you can save printer.cfg and restart Klipper. 

## 4) Autotune should be enabled. 

Now, it is recommended by Autotune documentation to tune sensorless homing. I tested the default values and they work. So I didn't do the homing tuning. It would be helpful if somebody did the homing and published the values. 

If you want to disable Autotune, just comment it out in printer.cfg. 

## 5) Results after enabling Autotune on X and Y:

1) The steppers seem more quiet.  

2) I ran Shake&Tune before and after enabling. I was curious if it would be affected. I don't see an effect. 

<img width="1350" height="1044" alt="autotune" src="https://github.com/user-attachments/assets/2a7163a7-1691-4510-9dfd-7867cc5f5a49" />

3) I also printed an Orca VFA tower. I think there is less VFA, but not a large difference. Sorry, no pictures

4) Driver temperatures (while printing PLA) have gone down. I was at high 60's before. 

<img width="1089" height="667" alt="image" src="https://github.com/user-attachments/assets/f5e9f0fc-0e8d-4e4b-9df2-8b55914c46ae" />

## *Update:

After writing this I noticed there are motor values in printer.cfg:

```
[motor_constants qidi_x_y]
# Coil resistance, Ohms
resistance: 3.7
# Coil inductance, Henries
inductance: 0.004
# Holding torque, Nm
holding_torque: 0.73
# Nominal rated current, Amps
max_current: 2.0
# Steps per revolution (1.8deg motors use 200, 0.9deg motors use 400)
steps_per_revolution: 200
```

These are different from manufacturer provided values. Should we believe these values or manufacturer provided values? These could be some remnants from development with different steppers. I choose to believe the manufacturer. 


