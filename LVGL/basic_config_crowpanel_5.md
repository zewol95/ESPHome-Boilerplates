# BASIC CONFIG FOR CROWPANEL 5' #

```

esphome:
  name: elecrow5-panel
  friendly_name: elecrow5_panel
  on_boot:
    priority: -100
    then:
      - delay: 2s
      - component.update: touch0

  platformio_options:
    board_build.esp-idf.memory_type: qio_opi
    board_build.flash_mode: dio

esp32:
  board: esp32-s3-devkitc-1
  framework:
    type: esp-idf

psram:
  mode: octal
  speed: 80MHz

logger:
  level: DEBUG

api:
  encryption:
    key: "<CHANGE_ME>"
  on_client_connected:
    - if:
        condition:
          lambda: 'return (0 == client_info.find("Home Assistant "));'
        then:
          - lvgl.widget.show: lbl_hastatus
  on_client_disconnected:
    - if:
        condition:
          lambda: 'return (0 == client_info.find("Home Assistant "));'
        then:
          - lvgl.widget.hide: lbl_hastatus

ota:
  - platform: esphome
    password: "<CHANGE_ME>"

wifi:
  ssid: "M&A_IOT"
  password: "<CHANGE_ME>"
  ap:
    ssid: "Display-1 Fallback Hotspot"
    password: "<CHANGE_ME>"

captive_portal:

i2c:
  sda: 19
  scl: 20
  scan: true

display:
  - platform: rpi_dpi_rgb
    id: main_display
    color_order: RGB
    invert_colors: True
    auto_clear_enabled: false
    dimensions:
      width: 800
      height: 480
    de_pin: 40
    hsync_pin: 39
    vsync_pin: 41
    pclk_pin: 0
    pclk_frequency: 18MHz #   24mhz
    data_pins:
      red:
        - 45        #r1
        - 48        #r2
        - 47        #r3
        - 21        #r4
        - 14        #r5
      green:
        - 5         #g0
        - 6         #g1
        - 7         #g2
        - 15        #g3
        - 16        #g4
        - 4         #g5
      blue:
        - 8         #b1
        - 3         #b2
        - 46        #b3
        - 9         #b4
        - 1         #b5

touchscreen:
  platform: gt911
  id: touch0
  display: main_display
  update_interval: 20ms
  on_release:
    then:
      - if:                 
          condition: lvgl.is_paused
          then:
            - lvgl.resume:
            - lvgl.widget.redraw:
            - switch.turn_off: switch_antiburn
            - lvgl.page.show: page_home_1
            - light.turn_on: display_backlight
            
output:
  - platform: ledc   # Backlight
    pin: 2
    id: backlight_pin

```
