# HARem - Home Assistant Remote Controller

**HARem** is a high-performance, minimalist "Thin Client" remote controller designed for the modern smart home. Built on the **ESPHome** ecosystem and powered by the **ESP32-C3**, it provides a boutique-quality physical interface that prioritizes speed, aesthetics, and zero-maintenance operation.

## 📛 The Name
The name **HARem** is a clever portmanteau of **H**ome **A**ssistant and **Rem**ote, but its inspiration runs deeper into Turkish history and culture.

In the Ottoman Empire, the **Harem** represented the most private, sacred, and protected "inner sanctum" of the household. Just as the historical Harem was the heart of the home, this remote serves as the private, primary interface to control and protect the sanctity of your modern smart home.

## 🏛️ Architecture: The "Thin Client" Philosophy
Unlike traditional smart remotes that require manual configuration of every button, HARem operates as a dynamic viewport into your Home Assistant instance.
- **Service-Driven**: Rooms, devices, and states are streamed in real-time.
- **Zero Maintenance**: New devices added to a Home Assistant Area automatically appear on the remote.
- **Infinite Extensibility**: Complex logic is handled by Home Assistant Blueprints and Automations, keeping the hardware lightweight and responsive.

## ✨ Features
- **Modern UI**: A 5-line OLED interface featuring **Outfit** and **Montserrat** typography.
- **Dynamic Marquee**: Long device names automatically scroll with a smooth marquee effect.
- **Visual Feedback**: Interactive animations for startup, sleep countdowns, and action confirmations.
- **Power Intelligence**: Configurable standby and deep-sleep modes with precise battery voltage monitoring.
- **Global Control**: Toggle animations and power settings directly from a local on-device menu.

## 🚀 Quick Start
1.  **Hardware**: ESP32-C3 + SH1106 OLED + Rotary Encoder.
2.  **Firmware**: Flash `remote_controller.yaml`.
3.  **Home Assistant**: Follow the [Setup Guide](docs/generic_menu_setup.md) to create Helpers and Automation.

## 📖 Documentation
*   **[Setup Guide](docs/generic_menu_setup.md)**: How to configure Home Assistant.
*   **[Project Walkthrough](docs/walkthrough.md)**: Architecture, features, and troubleshooting.
*   **[Roadmap](TODO.md)**: Planned improvements and future features.

## 🛠️ Building
```bash
esphome run remote_controller.yaml
```

---
*Elevate your Home Assistant experience with a remote that feels as premium as your smart home.*
