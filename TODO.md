# TODO - HARem Project Roadmap

This file tracks planned improvements and premium features for the HARem Remote Controller.

## 📋 System Enhancements
- [ ] **"About" Screen**: Add a settings item to display firmware version, system uptime, and WiFi signal strength.
- [ ] **Haptic Feedback**: Support for a mini vibration motor for tactile feedback during encoder rotation and button clicks.
- [ ] **WiFi Provisioning**: Implement Improv WiFi or a Captive Portal for easier setup of credentials.

## ✨ Visual & UX Refinements
- [ ] **UI Themes**: Support for switching between "Light" (Inverted) and "Dark" display modes.
- [ ] **Quick Favorites**: A dedicated top-level menu for the 3 most frequently used devices.
- [ ] **Icon Library Expansion**: Add specialized icons for common device types (HVAC, Curtains, Media Players).
- [ ] **Smooth Transition Effects**: Implement subtle slide or fade animations when navigating between menu levels.

## 🛠️ Hardware Integration (Future)
- [ ] **Auto-Brightness**: Support for external light sensors (e.g., BH1750) to dim the display in dark environments.
- [ ] **Advanced Power Metrics**: Track deeper battery health and discharge curves using on-device logging.

## 🔋 Power & Hardware Optimization (SuperMini)
- [ ] **Battery Calibration**: Recalibrate `GPIO0` ADC. Current reading `1.48V` is wrong.
    - **Code**: `multiply: 2.0` (for equal resistors).

---
*Maintained by Orkan Murat Celik*
