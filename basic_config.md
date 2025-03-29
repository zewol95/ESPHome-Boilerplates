# Base Configuration for ESPHome

## Overview
ESPHome provides an easy-to-use platform for creating custom firmware for ESP8266 and ESP32 devices. By writing simple YAML configuration files, you can integrate sensors, switches, and other IoT devices into **Home Assistant** seamlessly. This guide will help you set up a basic configuration for your ESP32 device with ESPHome.

## Basic Configuration Steps
To get started, you need a basic configuration file that defines your ESP32 device and connects it to **Home Assistant**. Below is the core configuration required to set up your device with **ESPHome**.

### Configuration Breakdown
- **esphome**: Defines the name and friendly name of your device.
- **esp32**: Specifies the board type and the framework (Arduino in this case).
- **logger**: Enables logging for debugging purposes.
- **api**: Enables communication with **Home Assistant** and includes an encryption key.
- **ota**: Enables Over-The-Air (OTA) updates for easy firmware updates.
- **wifi**: Defines the Wi-Fi network the device will connect to.

## Base Configuration Code
Below is the basic configuration for your ESP32 device:

```yaml
esphome:
  name: <CHANGE_MY_NAME>
  friendly_name: <CHANGE_MY_NAME>

esp32:
  board: esp32dev
  framework:
    type: arduino

# Enable logging
logger:

# Enable Home Assistant API
api:
  encryption:
    key: "CHANGE_ME_AND_COPY_ME_IN_HOME_ASSISTANT"

ota:
  - platform: esphome
    password: "CHANGE_ME_AND_COPY_ME"

wifi:
  ssid: "<CHANGE_MY_NAME>"
  password: "<CHANGE_MY_NAME>"
