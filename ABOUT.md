# About HARem

**HARem (Home Assistant Remote)** is a high-performance, minimalist "Thin Client" remote controller designed for the modern smart home. Built on the **ESPHome** ecosystem and powered by the **ESP32-C3**, it provides a boutique-quality physical interface that prioritizes speed, aesthetics, and zero-maintenance operation.

## 📛 The Name: HARem
The name **HARem** is a clever portmanteau of **H**ome **A**ssistant and **Rem**ote, but its inspiration runs deeper into Turkish history and culture.

### Historical Context
In the Ottoman Empire, the **Harem** represented the most private, sacred, and protected "inner sanctum" of the household. It was a place of extreme importance where the daily life of the family was managed with absolute privacy and care. 

### Modern Interpretation
By adopting this name, the project acknowledges the smart home as the modern individual's "inner sanctum." Just as the historical Harem was the heart of the home, this remote serves as the private, primary interface to control and protect the sanctity of your living space. It "manages" the modern household from a single, intimate point of control, bridging centuries of domestic importance with cutting-edge technology.

## 🏛️ Architecture: The "Thin Client" Philosophy
Unlike traditional smart remotes that require manual configuration of every button, HARem operates as a dynamic viewport into your Home Assistant instance.
- **Service-Driven**: Rooms, devices, and states are streamed in real-time.
- **Zero Maintenance**: New devices added to a Home Assistant Area automatically appear on the remote.
- **Infinite Extensibility**: Complex logic is handled by Home Assistant Blueprints and Automations, keeping the remote hardware lightweight and responsive.

## ✨ Premium Features
- **Modern UI**: A 5-line OLED interface featuring **Outfit** and **Montserrat** typography for a professional, high-end feel.
- **Dynamic Marquee**: Long device names automatically scroll with a smooth marquee effect.
- **Visual Feedback**: Interactive animations for startup, sleep countdowns, and action confirmations.
- **Power Intelligence**: Configurable standby and deep-sleep modes with precise battery voltage monitoring.
- **Global Control**: Toggle animations and power settings directly from a local on-device menu.

## 🛠️ Hardware Stack
- **MCU**: ESP32-C3 Super Mini (RISC-V)
- **Display**: SSD1306/SH1106 128x64 OLED
- **Input**: EC11 Rotary Encoder with Push-button
- **Battery**: LiPo/Li-ion support with voltage sensing

---
*Elevate your Home Assistant experience with a remote that feels as premium as your smart home.*
