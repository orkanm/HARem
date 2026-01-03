# HARem - Home Assistant Remote Controller

This project implements a "Thin Client" smart home remote using **ESPHome** on an **ESP32-C3**. It features a 5-line OLED display with scrolling text and a rotary encoder to navigate Home Assistant Areas dynamically.

## 🚀 Quick Start

1.  **Hardware**: ESP32-C3 + SH1106 OLED + Rotary Encoder.
2.  **Firmware**: Flash `remote_controller.yaml`.
3.  **Home Assistant**: Follow the [Setup Guide](docs/generic_menu_setup.md) to create Helpers and Automation.

## 📖 Documentation

*   **[About HARem](ABOUT.md)**: Naming inspiration, philosophy, and detailed features.
*   **[Setup Guide](docs/generic_menu_setup.md)**: How to configure Home Assistant.
*   **[Project Walkthrough](docs/walkthrough.md)**: Architecture, features, and troubleshooting.
*   **[Roadmap](TODO.md)**: Planned improvements and future features.

## ✨ Features

-   **Zero Maintenance**: Automatically syncs rooms and devices from Home Assistant.
-   **5-Line Display**: Shows previous/next items and detailed selection context.
-   **Scrolling Text**: Long names automatically scroll (Marquee effect).
-   **Robust Navigation**: Reliable "Back" (Long Press) and cyclic scrolling.

## 🛠️ Building

```bash
esphome run remote_controller.yaml
```

See [walkthrough.md](docs/walkthrough.md) for detailed architecture diagrams.
