## How to add toolhead and host CPU temperatures to Klipper

Add this to printer.cfg

```
[temperature_sensor Host_CPU]
sensor_type: temperature_host
min_temp: 10
max_temp: 100

[temperature_sensor toolhead]
sensor_type: temperature_mcu
sensor_mcu: THR
min_temp: 0
max_temp: 100
```

Save and restart Klipper. You should see the new temperature readings in Klipper.

<img width="1742" height="1354" alt="image" src="https://github.com/user-attachments/assets/32e547e9-f5ed-49a4-a38b-deaff85535f8" />
