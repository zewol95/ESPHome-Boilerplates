# Collegamento del Sensore HLK-LD2450 su ESP32

## Introduzione
Il sensore **HLK-LD2450** è un radar a onde millimetriche ad alta precisione, ideale per il rilevamento della presenza e del movimento. Rispetto al modello **LD2410C**, il **LD2450** offre un'area di rilevamento più ampia e una maggiore precisione nella determinazione della posizione degli oggetti. Questo documento illustra il collegamento e la configurazione del sensore con un **ESP32**.

## Requisiti
### Hardware
- **ESP32**
- **HLK-LD2450**
- Cavi di collegamento **(jumper wires)**

### Software
- EspHome

## Collegamenti
| HLK-LD2450 | ESP32 |
|-------------|-------|
| VCC 5V | 5V |
| GND | GND |
| TX | GPIO16 (RX) |
| RX | GPIO17 (TX) |

> **Nota:** Verificare il livello logico del TX del sensore per garantire la compatibilità con l'ESP32 (3.3V sulle linee RX/TX).

## Configurazione del Sensore in ESPHome
Il sensore HLK-LD2450 permette il rilevamento preciso della distanza e della posizione degli oggetti in un'area definita. Questo permette di configurare zone di rilevamento multiple con sensibilità variabile, fornendo dati dettagliati su **energia di movimento** e **energia statica**.
I dati raccolti possono essere utilizzati per migliorare automazioni in **Home Assistant** e altre piattaforme IoT.

## Codice ESPHome
```cpp

external_components:
  - source: github://TillFleisch/ESPHome-HLK-LD2450@main

# GPIO16
# GPIO17

uart:
  id: uart_bus
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

LD2450:
  uart_id: uart_bus
  flip_x_axis: true
  fast_off_detection: true
  max_detection_tilt_angle:
    name: "Max Tilt Angle"
    initial_value: 40°
  min_detection_tilt_angle:
    name: "Min Tilt Angle"
    initial_value: -40°
  max_detection_distance:
    name: "Max Distance"
    initial_value: 4m
  max_distance_margin: 30cm
  
  restart_button:
    name: "Restart Sensor"
  factory_reset_button:
    name: "Factory Reset Sensor"

  tracking_mode_switch:
    name: "Multiple Target Tracking"
  bluetooth_switch:
    name: "Sensor Bluetooth"

  baud_rate_select:
    name: "Sensor Baud Rate"

  occupancy:
    name: Occupancy
  target_count:
    name: Target Count

  targets:
    - target:
        name: "T1"
        id: t1
        debug: true
        x_position:
            id: t1_xpos
        y_position:
            id: t1_ypos
        speed:
            id: t1_speed
        distance_resolution:
            id: t1_res
        angle:
            id: t1_angle
        distance:
            id: t1_distance
    - target:
        id: t2
        x_position:
            id: t2_xpos
        y_position:
            id: t2_ypos
        speed:
            id: t2_speed
        distance_resolution:
            id: t2_res
        angle:
            id: t2_angle
        distance:
            id: t2_distance
    - target:
        id: t3
        x_position:
            id: t3_xpos
        y_position:
            id: t3_ypos
        speed:
            id: t3_speed
        distance_resolution:
            id: t3_res
        angle:
            id: t3_angle
        distance:
            id: t3_distance
  zones:
    - zone:
        name: "Office Right"
        polygon:
          - point:
              x: 0m
              y: 0m
          - point:
              x: 0m
              y: 600cm
          - point:
              x: 6m
              y: 6m
          - point:
              x: 6m
              y: 0m
        occupancy:
          id: z1_occupancy
        target_count:
          id: z1_target_count
    - zone:
        name: "Office Left"
        margin: 0.4m
        polygon:
          - point:
              x: -0m
              y: 0m
          - point:
              x: -0m
              y: 6m
          - point:
              x: -6m
              y: 6m
          - point:
              x: -6m
              y: 0m
        occupancy:
          id: z2_occupancy
        target_count:
          id: z2_target_count

```

## Conclusione
Il **HLK-LD2450** rappresenta un'evoluzione del modello **LD2410C**, offrendo maggiore precisione e funzionalità avanzate nel rilevamento della posizione degli oggetti. Grazie alla sua integrazione con **ESP32** e **ESPHome**, è una soluzione ideale per applicazioni di **rilevamento presenza, sicurezza e automazione domestica avanzata**.

# Costruzione grafico dinamico in Home Assistant

## Requisiti
### Software
- **HACS**
- **plotly-graph**


## Codice Plotly-graph
Nota: Occorre rinominare le entità correttamente

```cpp

type: custom:plotly-graph
title: mmWave Radar Sensor
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
    dtick: 100
    gridcolor: RGBA(200,200,200,0.15)
    zerolinecolor: RGBA(200,200,200,0.15)
    type: number
    fixedrange: true
    range:
      - 400
      - -400
  yaxis:
    dtick: 100
    gridcolor: RGBA(200,200,200,0.15)
    zerolinecolor: RGBA(200,200,200,0.15)
    scaleanchor: x
    scaleratio: 1
    fixedrange: true
    range:
      - 600
      - 0
entities:
  - entity: ''
    name: Target1
    marker:
      size: 12
    line:
      shape: spline
      width: 5
    x:
      - $ex hass.states["sensor.esphome_LD2450_t1_x_position"].state /-10
    'y':
      - $ex hass.states["sensor.esphome_LD2450_t1_y_position"].state /10
  - entity: ''
    name: Target2
    marker:
      size: 12
    line:
      shape: spline
      width: 5
    x:
      - $ex hass.states["sensor.esphome_LD2450_target_2_x_position"].state /-10
    'y':
      - $ex hass.states["sensor.esphome_LD2450_target_2_y_position"].state /10
  - entity: ''
    name: Target3
    marker:
      size: 12
    line:
      shape: spline
      width: 5
    x:
      - $ex hass.states["sensor.esphome_LD2450_target_3_x_position"].state /-10
    'y':
      - $ex hass.states["sensor.esphome_LD2450_target_3_y_position"].state /10
  - entity: ''
    name: Zone1
    mode: lines
    fill: toself
    fillcolor: RGBA(20,200,0,0.1)
    line:
      color: RGBA(20,200,0,0.2)
      shape: line
      width: 2
    x:
      - $ex hass.states["number.mmwave_sensor_zone_1_x1"].state /-10
      - $ex hass.states["number.mmwave_sensor_zone_1_x1"].state /-10
      - $ex hass.states["number.mmwave_sensor_zone_1_x2"].state /-10
      - $ex hass.states["number.mmwave_sensor_zone_1_x2"].state /-10
      - $ex hass.states["number.mmwave_sensor_zone_1_x1"].state /-10
    'y':
      - $ex hass.states["number.mmwave_sensor_zone_1_y1"].state /10
      - $ex hass.states["number.mmwave_sensor_zone_1_y2"].state /10
      - $ex hass.states["number.mmwave_sensor_zone_1_y2"].state /10
      - $ex hass.states["number.mmwave_sensor_zone_1_y1"].state /10
      - $ex hass.states["number.mmwave_sensor_zone_1_y1"].state /10
  - entity: ''
    name: Zone2
    mode: lines
    fill: toself
    fillcolor: RGBA(200,0,255,0.1)
    line:
      color: RGBA(200,0,255,0.2)
      shape: line
      width: 2
    x:
      - $ex hass.states["number.mmwave_sensor_zone_2_x1"].state /-10
      - $ex hass.states["number.mmwave_sensor_zone_2_x1"].state /-10
      - $ex hass.states["number.mmwave_sensor_zone_2_x2"].state /-10
      - $ex hass.states["number.mmwave_sensor_zone_2_x2"].state /-10
      - $ex hass.states["number.mmwave_sensor_zone_2_x1"].state /-10
    'y':
      - $ex hass.states["number.mmwave_sensor_zone_2_y1"].state /10
      - $ex hass.states["number.mmwave_sensor_zone_2_y2"].state /10
      - $ex hass.states["number.mmwave_sensor_zone_2_y2"].state /10
      - $ex hass.states["number.mmwave_sensor_zone_2_y1"].state /10
      - $ex hass.states["number.mmwave_sensor_zone_2_y1"].state /10
  - entity: ''
    name: Zone3
    mode: lines
    fill: toself
    fillcolor: RGBA(200,120,55,0.1)
    line:
      color: RGBA(200,120,55,0.2)
      shape: line
      width: 2
    x:
      - $ex hass.states["number.mmwave_sensor_zone_3_x1"].state /-10
      - $ex hass.states["number.mmwave_sensor_zone_3_x1"].state /-10
      - $ex hass.states["number.mmwave_sensor_zone_3_x2"].state /-10
      - $ex hass.states["number.mmwave_sensor_zone_3_x2"].state /-10
      - $ex hass.states["number.mmwave_sensor_zone_3_x1"].state /-10
    'y':
      - $ex hass.states["number.mmwave_sensor_zone_3_y1"].state /10
      - $ex hass.states["number.mmwave_sensor_zone_3_y2"].state /10
      - $ex hass.states["number.mmwave_sensor_zone_3_y2"].state /10
      - $ex hass.states["number.mmwave_sensor_zone_3_y1"].state /10
      - $ex hass.states["number.mmwave_sensor_zone_3_y1"].state /10
  - entity: ''
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
      - $ex 600 * Math.sin((2 * Math.PI)/360 * 60)
      - 450
      - 400
      - 300
      - 200
      - 100
      - 0
      - -100
      - -200
      - -300
      - -400
      - -450
      - $ex -600 * Math.sin((2 * Math.PI)/360 * 60)
      - 0
    'y':
      - 0
      - $ex 600 * Math.cos((2 * Math.PI)/360 * 60)
      - $ex Math.sqrt( 600**2 - 450**2 )
      - $ex Math.sqrt( 600**2 - 400**2 )
      - $ex Math.sqrt( 600**2 - 300**2 )
      - $ex Math.sqrt( 600**2 - 200**2 )
      - $ex Math.sqrt( 600**2 - 100**2 )
      - 600
      - $ex Math.sqrt( 600**2 - 100**2 )
      - $ex Math.sqrt( 600**2 - 200**2 )
      - $ex Math.sqrt( 600**2 - 300**2 )
      - $ex Math.sqrt( 600**2 - 400**2 )
      - $ex Math.sqrt( 600**2 - 450**2 )
      - $ex 600 * Math.cos((2 * Math.PI)/360 * 60)
      - 0
raw_plotly_config: true

```
