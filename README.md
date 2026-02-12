# HARem - Home Assistant Remote Controller

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-Donate-orange?style=for-the-badge&logo=buy-me-a-coffee)](https://www.buymeacoffee.com/orkanm)

**HARem** is a high-performance, minimalist "Thin Client" remote controller designed for the modern smart home. Built on the **ESPHome** ecosystem and powered by the **ESP32-C3**, it provides a boutique-quality physical interface that prioritizes speed, aesthetics, and zero-maintenance operation.

---

## 📛 The Name: HARem
The name **HARem** is a clever portmanteau of **H**ome **A**ssistant and **Rem**ote, but its inspiration runs deeper into Turkish history and culture.

### The Inner Sanctum
In Turkish and Ottoman history, the **Harem** represented the most private, sacred, and protected part of the household. It was a place of extreme importance where the daily life of the family was managed with absolute privacy and care. 

By adopting this name, the project acknowledges the smart home as the modern individual's "inner sanctum." Just as the historical Harem was the heart of the home, this remote serves as the private, primary interface to control and protect the sanctity of your living space. It manages the modern household from a single, intimate point of control, bridging centuries of domestic importance with cutting-edge technology.

---

## 🏛️ Architecture: The "Thin Client" Philosophy
Unlike traditional smart remotes that require manual configuration of every button, HARem operates as a dynamic viewport into your Home Assistant instance.
- **Service-Driven**: Rooms, devices, and states are streamed in real-time.
- **Zero Maintenance**: New devices added to a Home Assistant Area automatically appear on the remote.
- **Infinite Extensibility**: Complex logic is handled by Home Assistant Blueprints and Automations, keeping the hardware lightweight and responsive.

## ✨ Premium Features
- **Modern UI**: A 5-line OLED interface featuring **Outfit** and **Montserrat** typography for a professional, high-end feel.
- **Dynamic Marquee**: Long device names automatically scroll with a smooth marquee effect.
- **Guest Mode Protection**: Secure, PIN-protected "Guest Mode" to restrict access to specific areas or devices.
- **Visual Feedback**: Interactive animations for startup, sleep countdowns, and action confirmations.
- **Power Intelligence**: Configurable standby and deep-sleep modes with precise battery voltage monitoring.
- **Global Control**: Toggle animations and power settings directly from a local on-device menu.

## 🛠️ Hardware Setup

### Wiring Diagram (ASCII)
```text
       ESP32-C3 Super Mini             Peripherals
    ┌───────────────────────┐        ┌───────────────┐
    │          5V / 3.3V [VCC]──────▶[VCC]  OLED     │
    │                GND [GND]──────▶[GND] (SSD1306) │
    │         GPIO5 (SDA)[SDA]──────▶[SDA]           │
    │         GPIO6 (SCL)[SCL]──────▶[SCL]           │
    │                       │        └───────────────┘
    │                       │        ┌───────────────┐
    │     GPIO8 (CLK)[CLK]◀─┼────────[CLK]  ROTARY   │
    │      GPIO9 (DT)[DT]◀──┼────────[DT]   ENCODER  │
    │    GPIO4 (SW)[Wake]◀──┼────────[SW]   (EC11)   │
    │                GND ◀──┼────────[GND]           │
    │                       │        └───────────────┘
    │   GPIO0 (ADC) [BAT]◀──┼──┬─────[+] BATTERY     │
    │                       │ [10k]  └───────────────┘
    └───────────────────────┘  └──▶ 10k Divider to GND
```

### Pin Mapping Table
| Component | ESP32-C3 Pin | Type | Notes |
| :--- | :--- | :--- | :--- |
| **OLED SDA/SCL** | GPIO5 / GPIO6 | I2C | SSD1306/SH1106 |
| **Encoder CLK/DT** | GPIO8 / GPIO9 | Input | Navigation |
| **Encoder SW** | GPIO4 | Input | Wakeup Trigger |
| **Battery ADC** | GPIO0 | Analog | 10k/10k Divider |

## 🚀 Quick Start
1.  **Hardware**: ESP32-C3 + SH1106 OLED + Rotary Encoder.
2.  **Firmware**: Flash `remote_controller.yaml`.
3.  **Home Assistant**: Follow the [Setup Guide](docs/generic_menu_setup.md) to create Helpers and Automation.

## 📖 Extended Documentation
*   **[Setup Guide](docs/generic_menu_setup.md)**: How to configure Home Assistant.
*   **[Project Walkthrough](docs/walkthrough.md)**: Architecture, features, and troubleshooting.
*   **[Roadmap](TODO.md)**: Planned improvements and future features.

## 🏗️ Building
```bash
esphome run remote_controller.yaml
```

---
*Elevate your Home Assistant experience with a remote that feels as premium as your smart home.*

## ☕ Support the Project
If you find this project helpful and would like to support its development, you can buy me a coffee! Your support helps fund new prototypes and keeps the project open-source and free for everyone.

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-Donate-orange?style=for-the-badge&logo=buy-me-a-coffee)](https://www.buymeacoffee.com/orkanm)
