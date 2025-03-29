# HLK-LD2410C on ESP32

## Introduction
The **HLK-LD2410C** sensor is a high-precision millimeter-wave radar, ideal for presence and motion detection. This document explains how to connect and configure the sensor with an **ESP32**.

## Requirements
### Hardware
- **ESP32**
- **HLK-LD2410C**
- Jumper wires

### Software
- ESPHome

## Connections
| HLK-LD2410C | ESP32 |
|-------------|-------|
| VCC 5V      | 5V    |
| GND         | GND   |
| TX          | GPIO16 (RX) |
| RX          | GPIO17 (TX) |

> **Note:** Use a level shifter adapter if the module operates at **3.3V**, as the sensor's TX is at 3.3V, while the ESP32 supports 3.3V on its RX/TX lines.

## Configuring Gates G0-G8 in ESPHome
The HLK-LD2410C sensor allows the configuration of multiple detection gates (G0-G8) in ESPHome, which represent progressive distance zones from the sensor. Each gate provides data on motion energy (move_energy) and static energy (still_energy).  
This configuration enables monitoring of motion in different areas, enhancing the detection's precision and sensitivity. The collected data can be integrated into Home Assistant for advanced automation and security applications.



## Codice EspHome
```cpp

uart:
  tx_pin: GPIO17
  rx_pin: GPIO16
  baud_rate: 256000

ld2410:

binary_sensor:
  - platform: ld2410
    has_target:
      name: Presence
    has_moving_target:
      name: Moving Target
    has_still_target:
      name: Still Target
    out_pin_presence_status:
      name: out pin presence status

sensor:
  - platform: ld2410
    light:
      name: light
    moving_distance:
      name : Moving Distance
    still_distance:
      name: Still Distance
    moving_energy:
      name: Move Energy
    still_energy:
      name: Still Energy
    detection_distance:
      name: Detection Distance
    g0:
      move_energy:
        name: g0 move energy
      still_energy:
        name: g0 still energy
    g1:
      move_energy:
        name: g1 move energy
      still_energy:
        name: g1 still energy
    g2:
      move_energy:
        name: g2 move energy
      still_energy:
        name: g2 still energy
    g3:
      move_energy:
        name: g3 move energy
      still_energy:
        name: g3 still energy
    g4:
      move_energy:
        name: g4 move energy
      still_energy:
        name: g4 still energy
    g5:
      move_energy:
        name: g5 move energy
      still_energy:
        name: g5 still energy
    g6:
      move_energy:
        name: g6 move energy
      still_energy:
        name: g6 still energy
    g7:
      move_energy:
        name: g7 move energy
      still_energy:
        name: g7 still energy
    g8:
      move_energy:
        name: g8 move energy
      still_energy:
        name: g8 still energy


switch:
  - platform: ld2410
    engineering_mode:
      name: "engineering mode"
    bluetooth:
      name: "control bluetooth"

number:
  - platform: ld2410
    timeout:
      name: timeout
    light_threshold:
      name: light threshold
    max_move_distance_gate:
      name: max move distance gate
    max_still_distance_gate:
      name: max still distance gate
    g0:
      move_threshold:
        name: g0 move threshold
      still_threshold:
        name: g0 still threshold
    g1:
      move_threshold:
        name: g1 move threshold
      still_threshold:
        name: g1 still threshold
    g2:
      move_threshold:
        name: g2 move threshold
      still_threshold:
        name: g2 still threshold
    g3:
      move_threshold:
        name: g3 move threshold
      still_threshold:
        name: g3 still threshold
    g4:
      move_threshold:
        name: g4 move threshold
      still_threshold:
        name: g4 still threshold
    g5:
      move_threshold:
        name: g5 move threshold
      still_threshold:
        name: g5 still threshold
    g6:
      move_threshold:
        name: g6 move threshold
      still_threshold:
        name: g6 still threshold
    g7:
      move_threshold:
        name: g7 move threshold
      still_threshold:
        name: g7 still threshold
    g8:
      move_threshold:
        name: g8 move threshold
      still_threshold:
        name: g8 still threshold

button:
  - platform: ld2410
    factory_reset:
      name: "factory reset"
    restart:
      name: "restart"
    query_params:
      name: query params

text_sensor:
  - platform: ld2410
    version:
      name: "firmware version"
    mac_address:
      name: "mac address"

select:
  - platform: ld2410
    distance_resolution:
      name: "distance resolution"
    baud_rate:
      name: "baud rate"
    light_function:
      name: light function
    out_pin_level:
      name: out pin level

```

## Conclusione
Il **HLK-LD2410C** è un sensore radar efficace e facile da integrare con **ESP32**, utile per applicazioni come **rilevamento presenza, sicurezza e automazione domestica**.

