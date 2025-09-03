#How to make the printer lower the build plate after printing ends

Change in slicer machine end G-code this line:

```
{if max_layer_z < max_print_height / 2}G1 Z{max_print_height / 2 + 10} F600{else}G1 Z{min(max_print_height, max_layer_z + 3)}{endif}
```

to this

```
G1 Z{max_print_height - 5} F600
```

It will then lower to 5mm from lowest position.
