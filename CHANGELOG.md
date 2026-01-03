# Changelog

All notable changes to the HARem project will be documented in this file.

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
