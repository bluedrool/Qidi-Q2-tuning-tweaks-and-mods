# Calibrating e-steps / rotation distance

I'm continuing going through the basic machine calibrations. Here is the procedure I used. 

I recommend printing out this tool:

https://www.printables.com/model/352257-estep-calibration-tool

And the remixed parts for it:

https://www.printables.com/model/483611-remix-e-step-stick

You will need calipers. See the remixed parts for a picture on how to measure using the printed tool.

I refreshed my memory of the procedure from Elli's print tuning guide (recommended reading):

https://ellis3dp.com/Print-Tuning-Guide/articles/extruder_calibration.html

Then do the following steps:

## 1) Home the machine. Lower Z enought to have space to extrude filament. 

## 2) Remove the hotend:

- remove the toolhead front cover

- cut filament 

- remove the hotend by screwing the 2 screws

- there is enough slack in the hotend cables to move it carefully to to the left side

## 3) Edit printer.cfg

- to go [extruder]

- find line "min_extrude_temp: 175" and change value to 0

- check line "max_extrude_only_distance: 1000.0", it should be set to 101 or longer.

- also check line "rotation_distance: 53.7", you will need this later

- save and restart

## 4) Go to you printers Fluidd interface and tell it to extrude 100mm of filament at 2mm/s. Check that the filament doesn't bind anywhere while it is extruding.

## 5) Cut the filament using the toolheads filament cutter

## 6) Measure the filament piece with the tool you have printed and calipers (or other method). Mine was 102.42, 102.42, 102.40 so a mean of 102.41.

## 7) Calculate new value with this formula:

<previous_rotation_distance> * ( <actual_extrude_distance> / 100 ) = <new_rotation_distance>

So for me this was

53.7 * (102.41 / 100) = 54.99417

## 8) Temporarily set the new value in the Fluidd console

Command for me:

SET_EXTRUDER_ROTATION_DISTANCE EXTRUDER=extruder DISTANCE=54.99417

## 9) Run steps 4-8 again

I would say that if you new filament lenght is within 0.1-0.2mm of 100mm (99.8-100.2) the you can move to step 10.

If it is not within 0.1-0.2mm or you want to make sure with repeated measurement, calculate the new rotation distance. 

Mine was (mean of three measurements) 100.40mm. So I calculated a new rotation distance of 55.21414668 and set it:

SET_EXTRUDER_ROTATION_DISTANCE EXTRUDER=extruder DISTANCE=55.21414668

I got it within 0.1mm of 100mm with 6 total extruded samples and stopped there. There is inaccuracy in your measuring, the extruder, cutter, measuring tool and calipers. I think under 0.2-0.3mm variance is about at the practical limit of this method. 

## 10) Save the new value in printer.cfg

-Find [extruder]

-Inpute the new value in line "rotation_distance:"

- find line "min_extrude_temp: 0" and change back to value to 175

-Save and restart

## 11) Move the hotend to the original position and carefully tighten the screws. Check that the hotend seats in place and that you don't tighten it in crooked. Replace toolhead cover. 

## 11) You are done. 



