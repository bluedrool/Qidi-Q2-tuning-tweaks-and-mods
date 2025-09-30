# How to use the Fan 2 header in toolhead in Klipper

Information thanks to weaw on Qidi discord. 

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

Now you can use the additional fan head for 2 pin 24V fan.
