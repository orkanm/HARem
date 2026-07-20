# HARem - Home Assistant Remote Controller

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-Donate-orange?style=for-the-badge&logo=buy-me-a-coffee)](https://buymeacoffee.com/orquitto)


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
- **Modern UI**: A 5-line **1.3" OLED** interface featuring **Outfit** and **Montserrat** typography. Includes a **left-side vertical scrollbar** and **outline-based selection highlight** for a professional feel.
- **Dynamic Marquee**: Long device names automatically scroll with a smooth marquee effect, protected by a hardware-accelerated UI mask.
- **Guest Mode Protection**: Secure, PIN-protected "Guest Mode" to restrict access to specific areas or devices.
- **Visual Feedback**: Interactive animations for startup, sleep countdowns, and action confirmations.
- **Power Intelligence**: Configurable standby and deep-sleep modes with hardened battery voltage monitoring and low-battery alerts.
- **Global Control**: Toggle animations and power settings directly from a local on-device menu.

## 🔋 Power & Hardware Optimization (SuperMini)
- [x] **Battery Calibration**: Corrected `GPIO0` ADC mapping and low-battery thresholds (Fixed in v0.7.0).

### 🔋 Ultra-Low Power Design
HARem is optimized for maximum battery performance:
*   **Deep Sleep**: Leverages ESP32-C3 deep sleep for sub-1mA idle consumption.
*   **Ultra-Low-Power Divider**: Uses a high-impedance **1MΩ/1MΩ** battery voltage divider to eliminate parasitic drain.
*   **Active Management**: Components like the OLED are completely powered down when the device is dormant.

## 🛠️ Hardware Setup

### Wiring Diagram (ASCII)
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

---

### Pin Mapping Table
| Component | ESP32-C3 Pin | Type | Notes |
| :--- | :--- | :--- | :--- |
| **OLED VCC** | GPIO1 | Output | Active Power Management (via Q1 NPN) |
| **OLED SDA/SCL** | GPIO5 / GPIO6 | I2C | SSD1306/SH1106 |
| **Encoder CLK/DT** | GPIO8 / GPIO9 | Input | Navigation |
| **Encoder SW** | GPIO4 | Input | Wakeup Trigger |
| **Battery ADC** | GPIO0 | Analog | 1M/1M Divider + 0.1µF Cap |

### 🛠️ PCB Design
The HARem PCB is designed to be compact and easy to assemble, housing the ESP32-C3, OLED, and Rotary Encoder in a single unit.

![HARem PCB Render](PCB/HARem/HArem_PCB.png)

#### Interactive Visualizers
*   **[Interactive PCB 3D Viewer](https://3dviewer.net/index.html#model=https://raw.githubusercontent.com/orkanm/HARem/main/PCB/HARem/HArem.step)**: Examine the PCB 3D model directly in your browser (STEP format).
*   **[Interactive Case 3D Viewer](https://3dviewer.net/index.html#model=https://raw.githubusercontent.com/orkanm/HARem/main/Case/HARem_Case_2.0.step)**: Examine the 3D case model directly in your browser (STEP format).
*   **[Interactive Schematic](https://kicanvas.org/?github=https://github.com/orkanm/HARem/blob/main/PCB/HARem/HArem.kicad_sch)**: Explore the electrical design with KiCanvas.
*   **[Interactive PCB Layout](https://kicanvas.org/?github=https://github.com/orkanm/HARem/blob/main/PCB/HARem/HArem.kicad_pcb)**: Inspect the physical board routing and components.

#### Design Files
*   **[KiCad Project](PCB/HARem/)**: Source files for schematic and board layout.
*   **[PCB 3D Model (STEP)](PCB/HARem/HArem.step)**: Industrial standard 3D file for enclosure design.

### 🖨️ 3D Printed Case
The repository now includes a fully designed 3D printable case for the HARem remote, designed in FreeCAD.

<p align="center">
  <img src="Case/HARem-1.png" width="32%" />
  <img src="Case/HARem-2.png" width="32%" />
  <img src="Case/HARem-3.png" width="32%" />
</p>

#### Case Files
*   **[Case Files](Case/)**: Contains the source FreeCAD project (`.FCStd`), print-ready `.stl` files for the case, lid, and knob, along with a high-resolution `.step` model.

## 🚀 Quick Start
1.  **Hardware**: ESP32-C3 + 1.3" SH1106 OLED + Rotary Encoder.
2.  **Firmware**: 
    *   **Option A (Recommended)**: Download the latest **`.factory.bin`** from [Releases](https://github.com/orkanm/HARem/releases) and flash it to **offset 0x0**.
    *   **Option B**: Compile locally using `esphome run remote_controller.yaml`.
3.  **Home Assistant**: 
    *   The device will appear as "HARem" in your dashboard.
    *   **IMPORTANT**: If you used the GitHub Release binary, you will be asked for an **Encryption Key**:
        > `7knRB3/EL4Fq8Foul7Yhv+pcABzyLXHSQxa9bTzzQqg=`
    *   > [!WARNING]
        > This key is **publicly known**. For security, you should change it in your own configuration and perform an [OTA update](https://esphome.io/components/ota.html) as soon as the device is connected to your Home Assistant.
    *   Follow the [Setup Guide](docs/generic_menu_setup.md) to create the required Helpers and import the logic Blueprint.

## 📖 Extended Documentation
*   **[Setup Guide](docs/generic_menu_setup.md)**: How to configure Home Assistant.
*   **[Project Walkthrough](docs/walkthrough.md)**: Architecture, features, and troubleshooting.
*   **[Roadmap](TODO.md)**: Planned improvements and future features.

## 🏗️ Building & Local Development
For a detailed guide on setting up your Python environment, configuring secrets, and building the project, please refer to the:

👉 **[Environment Installation & Setup Guide](docs/installation.md)**

---
*Elevate your Home Assistant experience with a remote that feels as premium as your smart home.*

## ☕ Support the Project
If you find this project helpful and would like to support its development, you can buy me a coffee! Your support helps fund new prototypes and keeps the project open-source and free for everyone. 

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-Donate-orange?style=for-the-badge&logo=buy-me-a-coffee)](https://www.buymeacoffee.com/orquitto)
