# HLK-LD2450 on ESP32

## Introduction
The **HLK-LD2450** sensor is a high-precision millimeter-wave radar, ideal for presence and motion detection. Compared to the **LD2410C** model, the **LD2450** offers a wider detection area and greater accuracy in determining the position of objects. This document explains how to connect and configure the sensor with an **ESP32**.

## Requirements
### Hardware
- **ESP32**
- **HLK-LD2450**
- Jumper wires

### Software
- ESPHome

## Connections
| HLK-LD2450 | ESP32 |
|-------------|-------|
| VCC 5V      | 5V    |
| GND         | GND   |
| TX          | GPIO16 (RX) |
| RX          | GPIO17 (TX) |

> **Note:** Check the sensor's TX logic level to ensure compatibility with the ESP32 (3.3V on RX/TX lines).

## Configuring the Sensor in ESPHome
The HLK-LD2450 sensor allows precise distance and position detection of objects within a defined area. This enables the configuration of multiple detection zones with variable sensitivity, providing detailed data on **motion energy** and **static energy**.  
The collected data can be used to enhance automations in **Home Assistant** and other IoT platforms.


## ESPHome Code
```cpp

ld2450:
  id: ld2450_radar
  uart_id: uart_ld2450

uart:
  - id: uart_ld2450
    rx_pin: 
      number: GPIO16
      mode:
        input: true
        pullup: true
    tx_pin: 
      number: GPIO17
      mode:
        input: true
        pullup: true
    baud_rate: 256000
    parity: NONE
    stop_bits: 1
    data_bits: 8




binary_sensor:
  - platform: ld2450
    ld2450_id: ld2450_radar
    has_target:
      name: "Presence"
    has_moving_target:
      name: "Moving Target"
    has_still_target:
      name: "Still Target"

switch:
  - platform: ld2450
    ld2450_id: ld2450_radar
    bluetooth:
      name: "Bluetooth"
    multi_target:
      name: "Multi Target Tracking"

sensor:
  - platform: ld2450
    ld2450_id: ld2450_radar
    target_count:
      name: Presence Target Count
    still_target_count:
      name: Still Target Count
    moving_target_count:
      name: Moving Target Count
    target_1:
      x:
        name: Target-1 X
      y:
        name: Target-1 Y
      speed:
        name: Target-1 Speed
      angle:
        name: Target-1 Angle
      distance:
        name: Target-1 Distance
      resolution:
        name: Target-1 Resolution
    target_2:
      x:
        name: Target-2 X
      y:
        name: Target-2 Y
      speed:
        name: Target-2 Speed
      angle:
        name: Target-2 Angle
      distance:
        name: Target-2 Distance
      resolution:
        name: Target-2 Resolution
    target_3:
      x:
        name: Target-3 X
      y:
        name: Target-3 Y
      speed:
        name: Target-3 Speed
      angle:
        name: Target-3 Angle
      distance:
        name: Target-3 Distance
      resolution:
        name: Target-3 Resolution
    zone_1:
      target_count:
        name: Zone-1 All Target Count
      still_target_count:
        name: Zone-1 Still Target Count
      moving_target_count:
        name: Zone-1 Moving Target Count
    zone_2:
      target_count:
        name: Zone-2 All Target Count
      still_target_count:
        name: Zone-2 Still Target Count
      moving_target_count:
        name: Zone-2 Moving Target Count
    zone_3:
      target_count:
        name: Zone-3 All Target Count
      still_target_count:
        name: Zone-3 Still Target Count
      moving_target_count:
        name: Zone-3 Moving Target Count

number:
  - platform: ld2450
    ld2450_id: ld2450_radar
    presence_timeout:
      name: "Timeout"
    zone_1:
      x1:
        name: Zone-1 X1
      y1:
        name: Zone-1 Y1
      x2:
        name: Zone-1 X2
      y2:
        name: Zone-1 Y2
    zone_2:
      x1:
        name: Zone-2 X1
      y1:
        name: Zone-2 Y1
      x2:
        name: Zone-2 X2
      y2:
        name: Zone-2 Y2
    zone_3:
      x1:
        name: Zone-3 X1
      y1:
        name: Zone-3 Y1
      x2:
        name: Zone-3 X2
      y2:
        name: Zone-3 Y2


button:
  - platform: ld2450
    ld2450_id: ld2450_radar
    factory_reset:
      name: "LD2450 Factory Reset"
    restart:
      name: "LD2450 Restart"

text_sensor:
  - platform: ld2450
    ld2450_id: ld2450_radar
    version:
      name: "LD2450 Firmware"
    mac_address:
      name: "LD2450 BT MAC"
    target_1:
      direction:
        name: "Target-1 Direction"
    target_2:
      direction:
        name: "Target-2 Direction"
    target_3:
      direction:
        name: "Target-3 Direction"

```

## Conclusion
The **HLK-LD2450** is an evolution of the **LD2410C** model, offering greater precision and advanced features in detecting the position of objects. With its integration with **ESP32** and **ESPHome**, it is an ideal solution for **presence detection, security, and advanced home automation** applications.

# Building a Dynamic Graph in Home Assistant

## Requirements
### Software
- **HACS**
- **plotly-graph**

## Plotly-graph Code
Note: Be sure to rename the entities correctly.


```cpp

type: custom:plotly-graph
title: mmWave
refresh_interval: 1
hours_to_show: current_day
layout:
  height: 230
  margin:
    l: 50
    r: 20
    t: 20
    b: 40
  showlegend: true
  xaxis:
    dtick: 1000
    gridcolor: RGBA(200,200,200,0.15)
    zerolinecolor: RGBA(200,200,200,0.15)
    type: number
    fixedrange: true
    range:
      - -4000
      - 4000
  yaxis:
    dtick: 1000
    gridcolor: RGBA(200,200,200,0.15)
    zerolinecolor: RGBA(200,200,200,0.15)
    scaleanchor: x
    scaleratio: 1
    fixedrange: true
    range:
      - 0
      - 7500
entities:
  - entity: ""
    name: Target1
    marker:
      size: 12
    line:
      shape: spline
      width: 5
    x:
      - $ex hass.states["sensor.esphome_sentinella_esterna_target_1_x"].state
    "y":
      - $ex hass.states["sensor.esphome_sentinella_esterna_target_1_y"].state
  - entity: ""
    name: Target2
    marker:
      size: 12
    line:
      shape: spline
      width: 5
    x:
      - $ex hass.states["sensor.esphome_sentinella_esterna_target_2_x"].state
    "y":
      - $ex hass.states["sensor.esphome_sentinella_esterna_target_2_y"].state
  - entity: ""
    name: Target3
    marker:
      size: 12
    line:
      shape: spline
      width: 5
    x:
      - $ex hass.states["sensor.esphome_sentinella_esterna_target_3_x"].state
    "y":
      - $ex hass.states["sensor.esphome_sentinella_esterna_target_3_y"].state
  - entity: ""
    name: zone_1
    mode: lines
    fill: toself
    fillcolor: RGBA(20,200,0,0.06)
    line:
      color: RGBA(20,200,0,0.2)
      shape: line
      width: 2
    x:
      - $ex hass.states["number.esphome_sentinella_esterna_zone_1_x1"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_1_x1"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_1_x2"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_1_x2"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_1_x1"].state
    "y":
      - $ex hass.states["number.esphome_sentinella_esterna_zone_1_y1"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_1_y2"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_1_y2"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_1_y1"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_1_y1"].state
  - entity: ""
    name: zone_2
    mode: lines
    fill: toself
    fillcolor: RGBA(200,0,255,0.06)
    line:
      color: RGBA(200,0,255,0.2)
      shape: line
      width: 2
    x:
      - $ex hass.states["number.esphome_sentinella_esterna_zone_2_x1"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_2_x1"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_2_x2"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_2_x2"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_2_x1"].state
    "y":
      - $ex hass.states["number.esphome_sentinella_esterna_zone_2_y1"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_2_y2"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_2_y2"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_2_y1"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_2_y1"].state
  - entity: ""
    name: zone_3
    mode: lines
    fill: toself
    fillcolor: RGBA(200,120,55,0.06)
    line:
      color: RGBA(200,120,55,0.2)
      shape: line
      width: 2
    x:
      - $ex hass.states["number.esphome_sentinella_esterna_zone_3_x1"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_3_x1"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_3_x2"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_3_x2"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_3_x1"].state
    "y":
      - $ex hass.states["number.esphome_sentinella_esterna_zone_3_y1"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_3_y2"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_3_y2"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_3_y1"].state
      - $ex hass.states["number.esphome_sentinella_esterna_zone_3_y1"].state
  - entity: ""
    name: Coverage
    mode: lines
    fill: tonexty
    fillcolor: rgba(168, 216, 234, 0.15)
    line:
      shape: line
      width: 1
      dash: dot
    x:
      - 0
      - $ex 7500 * Math.sin((2 * Math.PI)/360 * 60)
      - 4500
      - 4000
      - 3000
      - 2000
      - 1000
      - 0
      - -1000
      - -2000
      - -3000
      - -4000
      - -4500
      - $ex -7500 * Math.sin((2 * Math.PI)/360 * 60)
      - 0
    "y":
      - 0
      - $ex 7500 * Math.cos((2 * Math.PI)/360 * 60)
      - $ex Math.sqrt( 7500**2 - 4500**2 )
      - $ex Math.sqrt( 7500**2 - 4000**2 )
      - $ex Math.sqrt( 7500**2 - 3000**2 )
      - $ex Math.sqrt( 7500**2 - 2000**2 )
      - $ex Math.sqrt( 7500**2 - 1000**2 )
      - 7500
      - $ex Math.sqrt( 7500**2 - 1000**2 )
      - $ex Math.sqrt( 7500**2 - 2000**2 )
      - $ex Math.sqrt( 7500**2 - 3000**2 )
      - $ex Math.sqrt( 7500**2 - 4000**2 )
      - $ex Math.sqrt( 7500**2 - 4500**2 )
      - $ex 7500 * Math.cos((2 * Math.PI)/360 * 60)
      - 0
raw_plotly_config: true


```
