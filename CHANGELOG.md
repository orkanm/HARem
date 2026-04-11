# Changelog

All notable changes to the HARem project will be documented in this file.

## [v0.6.3] - 2026-04-12

### Fixed
- **Stale Room Name on Line 5**: Lines 4 and 5 were rendered whenever `.has_state()` returned true, which persists even after HA sends an empty string. Added `state != ""` guard matching the existing behaviour of line 3.
- **'Activating...' Stuck for 10 Seconds (Blueprint)**: The blueprint had its own separate 10s timeout. Reduced to **3 seconds**. Additionally, `scene`, `script`, and `button` entities never produce a detectable state change after activation, so the `wait_template` always timed out and wrongly showed **Failed!**. These instant-action entities now skip the wait and clear the overlay after 1 second instead.
- **Control Screen Persists After Deep Sleep Wake**: When the device entered deep sleep during a dimming (control mode) session, HA retained the stale `overlay` (e.g. `Bright 75%`, `MAX 100%`) and `line_5` brightness values. On wake-up, these were pushed back to the display. Fixed by (1) firing `clear_overlay` to HA on the first API reconnect after a deep sleep wake, and (2) extending the `is_stale_feedback` filter to also suppress `MAX`/`MIN` overlay texts, not just `Bright`/`Vol`.

---

## [v0.6.2] - 2026-02-12

### Added
- **Interactive Visualizers**: Integrated KiCanvas for interactive Schematics and PCB Layouts.
- **3D Design**: Added high-quality STEP model for industrial-grade enclosure design.
- **WiFi Provisioning**: Enabled **Improv Serial** and **Captive Portal** for easy setup on new networks.
- **Brand Identity**: Added ESPHome branding and corrected GitHub language detection.
- **Dashboard Integration**: Added `dashboard_import` for seamless ESPHome integration.

### Changed
- **Power Optimization**: Documented ultra-low-power design and standardized 1M/1M battery divider specs.
- **Repository Hygiene**: Optimized `.gitignore` and `.gitattributes` for a cleaner developer experience.
- **Hardware Accuracy**: Aligned all diagrams and specifications with the final PCB design.

---

## [v0.2] - 2026-01-03

### Added
- **One-Click Setup Package**: Added `home_assistant/harem_package.yaml` to automate helper and automation creation.
- **Home Assistant Blueprint**: Added `blueprints/harem_remote.yaml` for a UI-driven setup experience.
- **Emergency Hotspot Display**: The remote now displays WiFi failure alerts with the fallback hotspot name and password.
- **Security Hardening**: Rotated all API keys, OTA, and AP passwords.
- **Improved CI**: Optimized GitHub Actions to use `pip` for faster and more reliable firmware builds.

### Changed
- Refined rotary encoder timing and click sensitivity.
- Updated documentation with streamlined setup paths (Package vs Blueprint vs Manual).
- Synchronized local display passwords with secure secrets.

---

## [v0.1] - 2026-01-01

### Added
- Initial core firmware with 5-line menu system.
- Marquee scrolling for long device names.
- Standby mode and power management.
- Dynamic area and entity discovery via Home Assistant events.
- Initial GitHub Actions implementation for automated builds.
