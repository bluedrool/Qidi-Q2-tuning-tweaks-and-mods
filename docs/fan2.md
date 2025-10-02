# How to use the toolhead fan 2 header in Klipper

Information and thanks to weaw on Qidi discord. 

Add this to printer.cfg

```
[fan_generic fan_2]
pin:THR:PB10
max_power: 1.0
shutdown_speed:0
cycle_time: 0.010
hardware_pwm: False
kick_start_time: 0.100
```

Save and restart Klipper. Now you can use the additional fan head for 2 pin 24V fan.

<img width="1187" height="1201" alt="image" src="https://github.com/user-attachments/assets/19ace641-d3ff-45a8-a812-1d72c3cebc16" />
