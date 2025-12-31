# HARem - Home Assistant Remote Controller

This project implements a smart home remote controller using **ESPHome** on an **ESP32-C3**. It features an OLED display and a rotary encoder to navigate and control Home Assistant entities.

## Hardware Requirements
- **Microcontroller**: ESP32-C3 (e.g., Super Mini)
- **Display**: SH1106 OLED (128x64) 1.3" I2C
- **Input**: Rotary Encoder (EC11)
- **Wiring**:
  - **SDA**: GPIO5
  - **SCL**: GPIO6
  - **Encoder A**: GPIO8
  - **Encoder B**: GPIO9
  - **Encoder Button**: GPIO7

## Configuration

The main configuration file is `remote_controller.yaml`.

### Secrets
You should use a `secrets.yaml` file (referenced in the config) or update the `remote_controller.yaml` directly with your credentials:
- `wifi_ssid` / `wifi_password`
- `api_key` (Base64 encryption key for Home Assistant API)

## Features
- **Cyclic Menu**: Rotate the knob to scroll through devices/scenes.
- **Feedback**: Display shows connection status (WiFi signal) and current state of devices (icons).
- **Control**: Click the knob to toggle the selected device.

## Flashing Instructions

The firmware uses the **ESP-IDF** framework.

### Pre-compiled Firmware
After a successful build, the firmware binary is located at:
`.esphome/build/harem/.pioenvs/harem/firmware.factory.bin`

You can flash this binary using `esptool` or the [ESPHome Web Flasher](https://web.esphome.io/).

### Compiling Locally
To compile the firmware yourself:
1. Install ESPHome (e.g., in a venv).
2. Run `esphome compile remote_controller.yaml`.
3. To upload directly: `esphome upload remote_controller.yaml`.
