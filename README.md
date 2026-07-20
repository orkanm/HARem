# HARem - Home Assistant Remote Controller

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-Donate-orange?style=for-the-badge&logo=buy-me-a-coffee)](https://buymeacoffee.com/orquitto)

**HARem** is a high-performance, minimalist "Thin Client" remote controller designed for the modern smart home. Built on the **ESPHome** ecosystem and powered by the **ESP32-C3**, it provides a boutique-quality physical interface that prioritizes speed, aesthetics, and zero-maintenance operation.

<p align="center">
  <img src="Case/HARem-1.png" width="32%" />
  <img src="Case/HARem-2.png" width="32%" />
  <img src="Case/HARem-3.png" width="32%" />
</p>

---

## ✨ Features & Architecture

Unlike traditional smart remotes that require manual configuration of every button, HARem operates as a **dynamic viewport** into your Home Assistant instance. Complex logic is handled by Home Assistant Blueprints and Automations, keeping the hardware lightweight and responsive.

- **Zero Maintenance**: Rooms, devices, and states are streamed in real-time. New devices added to a Home Assistant Area automatically appear on the remote.
- **Modern UI**: A 5-line **1.3" OLED** interface featuring Outfit and Montserrat typography, a vertical scrollbar, and hardware-accelerated dynamic marquee for long names.
- **Smart Power**: Deep sleep optimized for sub-1mA idle consumption, utilizing a high-impedance (1MΩ/1MΩ) battery divider and active OLED power management.
- **Guest Mode Protection**: Secure, PIN-protected "Guest Mode" to restrict access to specific areas or devices.
- **Visual Feedback & Control**: Interactive animations, sleep countdowns, and a local on-device menu for contrast and standby settings.

---

## 🚀 Quick Start

1. **Hardware**: ESP32-C3 SuperMini + 1.3" SH1106 OLED + EC11 Rotary Encoder.
2. **Firmware**: 
    * **Option A (Recommended)**: Download the latest **`.factory.bin`** from [Releases](https://github.com/orkanm/HARem/releases) and flash it to **offset 0x0**.
    * **Option B**: Compile locally using `esphome run remote_controller.yaml`.
3. **Home Assistant**: 
    * The device will appear as "HARem" in your dashboard.
    * **IMPORTANT**: If you used the GitHub Release binary, you will be asked for an **Encryption Key**:
        > `7knRB3/EL4Fq8Foul7Yhv+pcABzyLXHSQxa9bTzzQqg=`
    * > [!WARNING]
      > This key is **publicly known**. For security, you should change it in your own configuration and perform an [OTA update](https://esphome.io/components/ota.html) as soon as possible.
    * Follow the [Setup Guide](docs/generic_menu_setup.md) to create the required Helpers and import the logic Blueprint.

---

## 🛠️ Hardware & Assembly

Building your own HARem is straightforward. The project includes custom PCB files and 3D printable case models.

### Interactive Visualizers
Explore the design files directly in your browser:
* **[Interactive PCB 3D Viewer](https://3dviewer.net/index.html#model=https://raw.githubusercontent.com/orkanm/HARem/main/PCB/HARem/HArem.step)** (STEP format)
* **[Interactive Case 3D Viewer](https://3dviewer.net/index.html#model=https://raw.githubusercontent.com/orkanm/HARem/main/Case/HARem_Case_2.0-Case.stl,https://raw.githubusercontent.com/orkanm/HARem/main/Case/HARem_Case_2.0-Lid.stl,https://raw.githubusercontent.com/orkanm/HARem/main/Case/Encoder_Knob.stl)** (STL format)
* **[Interactive Schematic](https://kicanvas.org/?github=https://github.com/orkanm/HARem/blob/main/PCB/HARem/HArem.kicad_sch)**
* **[Interactive PCB Layout](https://kicanvas.org/?github=https://github.com/orkanm/HARem/blob/main/PCB/HARem/HArem.kicad_pcb)**

### Design Files
* **[KiCad Project](PCB/HARem/)**: Source files for schematic and board layout.
* **[PCB 3D Model](PCB/HARem/HArem.step)**: Industrial standard 3D file for enclosure design.
* **[3D Printed Case Files](Case/)**: Source FreeCAD project (`.FCStd`), print-ready `.stl` files for the case, lid, and knob, along with a high-resolution `.step` model.

<details>
<summary><b>View Wiring Diagram & Pin Mapping (Click to expand)</b></summary>

```text
       ESP32-C3 Super Mini                 Peripherals
    ┌───────────────────────┐            ┌───────────────┐
    │                       │   Q1(NPN)  │ 1.3" OLED     │
    │          5V / 3.3V ───┼────▶[C]    │               │
    │  GPIO1 (OLED PWR) ────┼────▶[B]    │               │
    │                       │     [E]────┼─▶ [VCC]       │
    │                GND ───┼────────────┼─▶ [GND]       │
    │         GPIO5 (SDA)───┼────────────┼─▶ [SDA]       │
    │         GPIO6 (SCL)───┼────────────┼─▶ [SCL]       │
    │                       │            └───────────────┘
    │                       │            
    │                       │            ┌───────────────┐
    │                       │            │ ROTARY (EC11) │
    │         GPIO8 (CLK) ◀─┼───[100Ω]───┼─ [CLK]        │
    │          GPIO9 (DT) ◀─┼───[100Ω]───┼─ [DT]         │
    │          GPIO4 (SW) ◀─┼───[100Ω]───┼─ [SW]         │
    │                GND  ◀─┼────────────┼─ [GND]        │
    │                       │            └───────────────┘
    │                       │            ┌───────────────┐
    │                       │            │ BAT MONITOR   │
    │                       │            │  [+] BATTERY  │
    │                       │            │   │           │
    │                       │            │  [1MΩ]        │
    │         GPIO0 (ADC) ◀─┼────────────┼───┤           │
    │                       │            │  [1MΩ] [0.1µF]│
    │                       │            │   │      │    │
    │                GND  ◀─┼────────────┼──GND────GND   │
    └───────────────────────┘            └───────────────┘
```

| Component | ESP32-C3 Pin | Type | Notes |
| :--- | :--- | :--- | :--- |
| **OLED VCC** | GPIO1 | Output | Active Power Management (via Q1 NPN) |
| **OLED SDA/SCL** | GPIO5 / GPIO6 | I2C | SSD1306/SH1106 |
| **Encoder CLK/DT** | GPIO8 / GPIO9 | Input | Navigation |
| **Encoder SW** | GPIO4 | Input | Wakeup Trigger |
| **Battery ADC** | GPIO0 | Analog | 1M/1M Divider + 0.1µF Cap |

</details>

---

## 📖 Documentation & Development

* **[Behavior & Interaction Map](docs/behavior_map.md)**: Visual state machines and logic flows for the device interaction.
* **[Project Walkthrough](docs/walkthrough.md)**: Architecture, features, and troubleshooting.
* **[Home Assistant Setup Guide](docs/generic_menu_setup.md)**: How to configure the Home Assistant side.
* **[Environment Installation & Setup Guide](docs/installation.md)**: Guide for setting up your local Python environment and secrets.
* **[Roadmap](TODO.md)**: Planned improvements and future features.

---

## 📛 Trivia: The Name HARem

The name **HARem** is a clever portmanteau of **H**ome **A**ssistant and **Rem**ote, but its inspiration runs deeper into Turkish history and culture. 

In Turkish and Ottoman history, the **Harem** represented the most private, sacred, and protected part of the household. By adopting this name, the project acknowledges the smart home as the modern individual's "inner sanctum." Just as the historical Harem was the heart of the home, this remote serves as the private, primary interface to control and protect the sanctity of your living space.

---

## ☕ Support the Project

If you find this project helpful and would like to support its development, you can buy me a coffee! Your support helps fund new prototypes and keeps the project open-source and free for everyone. 

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-Donate-orange?style=for-the-badge&logo=buy-me-a-coffee)](https://www.buymeacoffee.com/orquitto)
