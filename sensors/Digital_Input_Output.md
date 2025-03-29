# Digital Input & Digital Output

## Overview
This guide demonstrates how to set up basic **digital input** and **digital output** configurations using **ESPHome** on an ESP32 device. These configurations allow you to read the state of a button (digital input) and control a relay (digital output) through Home Assistant or ESPHome.

## Digital Input (Button)
A **digital input** can be used to read the state of a button, switch, or sensor. In this case, we'll configure a button with an internal pull-up resistor. When the button is pressed (closed to GND), the state will be `ON`. When the button is released (opened), the state will be `OFF`.

### Digital Input Code

```yaml
binary_sensor:
  - platform: gpio
    pin:
      number: CHANGE_GPIO_NUMBER_HERE
      mode: INPUT_PULLUP  # Enables internal pull-up resistor; CLOSED TO GND = ON; OPEN = OFF
    name: "Button Input"
    device_class: "opening"
    on_press:
      - logger.log: "Button pressed!"
    on_release:
      - logger.log: "Button released!"
```



# Digital Output


### Digital Output Code

```yaml
# GPIO output configuration for the relay controlling the boiler
output:
  - platform: gpio
    pin: GPIO21  # Choose the GPIO pin to which the relay is connected
    id: relay_boiler

# Switch configuration to control the output (relay) via Home Assistant or ESPHome
switch:
  - platform: output
    name: "Boiler"
    output: relay_boiler
    id: switch_boiler
    inverted: false  # The pin will go high with switch ON (logic high or 3.3V), activating the relay. The pin will go low with switch OFF (GND).
