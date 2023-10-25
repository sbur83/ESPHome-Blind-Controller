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
```
esphome:
  name: dining-blind
  friendly_name: dining-blind
  platform: ESP8266
  board: d1_mini

logger:
ota:
  password: !secret ESP_FallBack_Pass

wifi:
  ssid: !secret MyFi_SSID
  password: !secret MyFi_Pass

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "blinds_hotspot"
    password: !secret ESP_FallBack_Pass

captive_portal:
    
web_server:
  port: 80

api:
  encryption:
    key: "IkzZCeNyrBjd1NszGeCrCMchb9U+RdA944lpmC0R0wM="
  services:
    - service: control_blind
      variables:
        target: int
      then:
        - stepper.set_target:
            id: my_stepper
            target: !lambda 'return target;'
        - sensor.template.publish:
            id: position
            state: !lambda 'return target;'
cover:
  - platform: template
    name: "My Blind"
    id: my_blind
    device_class: blind
    open_action:
      - delay: .05s
      - stepper.set_target:
          id: my_stepper
          target: 6050
      - sensor.template.publish:
          id: position
          state: !lambda return id(my_stepper).target_position;
      - delay: 60s
    close_action:
      - delay: .05s
      - stepper.set_target:
          id: my_stepper
          target: 0
      - delay: 60s
      - sensor.template.publish:
            id: position
            state: !lambda return id(my_stepper).target_position;
    stop_action:
      - stepper.set_target:
          id: my_stepper
          target: !lambda return id(my_stepper).current_position;
      - sensor.template.publish:
            id: position
            state: !lambda return id(my_stepper).current_position;
    optimistic: true
    assumed_state: true
 
sensor:
  - platform: template
    name: "My Blind Position"
    id: position
 
stepper:
  - platform: uln2003
    id: my_stepper
    pin_a: D1
    pin_b: D2
    pin_c: D5
    pin_d: D6
    max_speed: 250 steps/s
    sleep_when_done: true

binary_sensor:
  - platform: gpio
    pin:
      number: D3
      inverted: TRUE
      mode:
        input: TRUE
        pullup: TRUE
    name: Open Button Sensor
    filters:
      - delayed_on: 10ms
    on_multi_click:
#SINGLE CLICK
      - timing:
          - ON for at most 0.5s
          - OFF for at least 0.5s
        then:
          - delay: .05s
          - stepper.set_target:
              id: my_stepper
              target: 0
          - delay: 60s
          - sensor.template.publish:
                id: position
                state: !lambda return id(my_stepper).target_position;
  - platform: gpio
    pin:
      number: D4
      inverted: TRUE
      mode:
        input: TRUE
        pullup: TRUE
    name: Close Button Sensor
    filters:
      - delayed_on: 10ms
    on_multi_click:
#SINGLE CLICK
      - timing:
          - ON for at most 0.5s
          - OFF for at least 0.5s
        then:
          - delay: .05s
          - stepper.set_target:
              id: my_stepper
              target: 6050
          - sensor.template.publish:
              id: position
              state: !lambda return id(my_stepper).target_position;
```



