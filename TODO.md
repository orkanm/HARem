# TODO - HARem Project Roadmap

This file tracks planned improvements and premium features for the HARem Remote Controller.

## 📋 System Enhancements
- [ ] **"About" Screen**: Add a settings item to display firmware version, system uptime, and WiFi signal strength.
- [ ] **Low Battery Overlay**: Trigger a centered "BATTERY LOW" warning overlay when the percentage drops below 10%.
- [ ] **Visual OTA Progress**: Display a real-time progress bar on the remote screen during wireless firmware updates.
- [ ] **Restrict PIN Change**: Ensure PIN can only be changed when Guest Mode is OFF (Full Access Mode).

## ✨ Visual & UX Refinements
- [ ] **Animated Font Transitions**: Investigate cross-fading or sliding door transitions between menu screens.
- [ ] **Dynamic Marquee Speed**: Auto-adjust scrolling speed based on the length of the device name.
- [ ] **Lock Navigation on Warning**: Prevent encoder from scrolling the device list while the "Not Supported !" warning is active.

## 🛠️ Hardware Integration (Future)
- [ ] **Auto-Brightness**: Support for external light sensors to dim the display in dark environments.
- [ ] **Advanced Power Metrics**: Track deeper battery health and discharge curves.

## 🔋 Power & Hardware Optimization (SuperMini)
- [ ] **Battery Calibration**: Recalibrate `GPIO0` ADC. Current reading `1.48V` is wrong.
    - **Action**: Add Voltage Divider to SuperMini (Battery -> R1 -> GPIO0 -> R2 -> GND).
    - **Values**: Use **2x 100kΩ** (Standard) or **2x 1MΩ + 0.1µF Cap** (Ultra Low Power).
    - **Code**: `multiply: 2.0` (for equal resistors).
- [ ] **Antenna Logic**: Monitor WiFi stability. The "SuperMini" has a weaker ceramic antenna; if connection drops recur, investigate external antenna mod or `wifi_power_save: LIGHT` tuning.
- [ ] **Extreme Sleep**: Investigate if the SuperMini's onboard LDO/LED can be desoldered to drop deep sleep current from **0.9mA** to **<50µA**.

---
*Maintained by Orkan Murat Celik*
