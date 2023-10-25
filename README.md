# ESPHome-Blind-Controller

*Rerquires Homeassistant with ESPHome addon*

### Parts needed:

D1 Mini, ULN2003AN Stepper Motor Driver, 28BYJ-48 5V Stepper Motor, 2 x Tactile Buttons, 4-6 x M3x8 Countersunk Screws, 2 x M3 Washers and Nuts, 2 x M3*5*4.2 Knurled Brass Heat Set Inserts, 28AWG Wire.
Tools: 3D Printer, Solder iron, solder, solder wick, hot glue, super glue.


### INSTRUCTIONS:

1. 3D print .stl files.
2. Remove the blue cap from the stepper motor and use a heated solder iron to remove the wire loom from the stepper motor.
3. Lift the plastic base on ULN2003 PCB header pins; heat pins with solder iron, when solder is molten gently remove pins from: +5v, -0v, IN1, IN2, IN3, IN4.
4. Clean the pads on the stepper motor and ULN2003 PCB with a hot solder iron and solder wick.
5. Cut wires to length, strip and tin ends and connect components with solder as per diagram.
6. Super glue the tactile buttons in position being careful not to get glue on the moving part of the buttons.
7. Hot Glue the wires in areas that will prevent movement/ stress on soldered connections.
8. Screw the ULN2003 PCB and stepper motor into position.
9. Flash .yaml to D1 mini using ESPHome addon using your wifi credentials and desired device name.

### Credits
This .yaml is a variation but largely taken from Lewis @ Everything Smart Home: https://everythingsmarthome.co.uk/diy-motorised-smart-blinds/

The chain cog was downloaded from metehan.demircioglu and modified in Fusion360: https://www.autodesk.com/community/gallery/project/159243/smart-blind-chasis

