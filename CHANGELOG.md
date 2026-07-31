# Changelog

All notable changes to the HARem project will be documented in this file.

## [v0.7.1] - 2026-06-30

### Fixed
- **Linear Scrolling**: Fixed a bug where short menus (fewer than 5 items) padded the display with empty lines that wrapped improperly, causing blank gaps. Changed the logic to clamp list elements in short menus while retaining infinite circular scrolling for larger ones.
- **Dimming Status Evaluation**: Fixed an issue where the remote reported dimmable lights as "Not Supported !" because the entity was missing a `friendly_name` and the status template scoping prevented the dimming support asterisk (`*`) from rendering properly. Jinja loops scoping was simplified to directly fetch by modulus. Added a fallback to `object_id` when `friendly_name` is missing to prevent invisible elements.
- **Overlay Click Passthrough**: Fixed an issue where pressing the button while an active feedback overlay (e.g., "Living Room ON", "Living Room OFF", "Turning Off...") was displayed caused the remote to navigate into the room menu and toggle a device inside that room. Button presses while an active feedback overlay is showing now cleanly dismiss the overlay without triggering menu navigation or entity state changes.
- **Stale Status String Overwrite**: Fixed an issue where adjusting entity percentage (brightness/volume) caused the status string on the right of the entity to revert to an old value upon exiting control mode. The Blueprint now preserves the optimistic status string during `refresh` events and prevents active (`on`/`playing`) entities from falling back to `(off)` when Home Assistant attribute updates are delayed.
- **Line 5 Rendering Lag**: Fixed a visual delay where Line 5 appeared slightly after Lines 1–4 when returning from overlay screens in room menus. Removed premature `line_5` clearing service calls during overlay dismissal so that ESPHome renders all 5 menu lines simultaneously.
- **Bulk Room Power Timeout & Long Press Exit**: Extended bulk power confirmation prompt timeout to 5 seconds of inactivity and added support for long pressing the dial (`action: back`) on `ROOT` to immediately dismiss the prompt without executing the action.
- **Device Dimming & Volume Overlay Timeout**: Added a 5-second inactivity timeout for individual device brightness and volume adjustments. When dial rotation ceases for 5 seconds (or upon click/long-press), the overlay automatically clears and ESPHome exits control mode.


## [v0.7.0] - 2026-05-02

### Added
- **UI Redesign**: Moved the vertical scrollbar to the left side and replaced the `>` bullet with a full-width outline selection box for a cleaner, modern look.
- **UI Masking**: Implemented a vertical hardware-accelerated mask at `x=0-5` to ensure marquee text doesn't spill over into the scrollbar area.
- **Synchronized Refresh**: Added a dedicated `refresh` event handler to the Blueprint and Remote to ensure the menu redraws instantly after adjustment overlays disappear.

### Fixed
- **Battery Logic**: Re-mapped battery percentage calculation to the correct voltage sensor and fixed the low-battery alert trigger threshold. Added hysteresis logic to prevent the percentage from incorrectly increasing during small voltage bounce-backs.
- **Battery Persistence**: Configured battery percentage and its baseline voltage to persist in RTC memory across deep sleep cycles.
- **Overlay Rendering**: Fixed an issue where the scrollbar and selection box were incorrectly drawn over local overlay warnings (like "Not Supported !").
- **UI Alignment**: Adjusted the top bar spacing to prevent the WiFi signal from being hidden behind the scrollbar mask and removed hyphens from the settings title to save space.
- **Scrollbar Stability**: Hardened the scrollbar logic with fallback values for `menu_total` to prevent the thumb from disappearing during transient network drops.
- **UI Artifacts**: Fixed "empty pixels" on the scrollbar corners by adjusting the selection box padding and drawing order.

---

## [v0.6.3] - 2026-04-12

### Fixed
- **Dimming Payload Rejections**: Bypassed a severe bug in the IKEA Home Smart integration where dimming commands caused fatal cache crashes by implicitly mapping `hs` capabilities against exclusively white-spectrum bulbs. Solved by dynamically injecting `color_temp` fallback variables explicitly into all `light.turn_on` action dictionaries natively within the Blueprint.
- **Pending Payload Interruptions ("Screen Stuck")**: Pressing the physical dial to confirm a dimming adjustment actively aborted any queued `250ms` payload payloads because the Home Assistant Blueprint ran in `mode: restart` during the event stream. The ESPHome firmware was refactored to bypass the Blueprint entirely and hit the underlying `input_text.set_value` HA Service natively. Additionally mitigated a C++ serialization fault in ESPHome by using invisible spaces (`" "`) instead of empty strings (`""`) to prevent board panic reboots.
- **Tracking Math Artifacts & Rubber-Banding**: Transitioned the visual dimming percentage tracker away from `Line 5` into the active parenthetical display string on `Line 3 Status` (e.g., `(30%) *`). Added rigorous 10% grid-snapping bounds, preventing confusing Zigbee physical approximations (like `29%`) from drifting into the UI during physical reads, and fixed an active-polling zero-bounds bug that stripped tracking strings aggressively when overshooting `(off)`.
- **Stale Room Name on Line 5**: Lines 4 and 5 were rendered whenever `.has_state()` returned true, which persists even after HA sends an empty string. Added `state != ""` guard matching the existing behaviour of line 3.
- **'Activating...' Stuck for 10 Seconds (Blueprint)**: The blueprint had its own separate 10s timeout. Reduced to **3 seconds**. Additionally, `scene`, `script`, and `button` entities never produce a detectable state change after activation, so the `wait_template` always timed out and wrongly showed **Failed!**. These instant-action entities now skip the wait and clear the overlay after 1 second instead.
- **Control Screen Persists After Deep Sleep Wake**: When the device entered deep sleep during a dimming (control mode) session, HA retained the stale `overlay` (e.g. `Bright 75%`, `MAX 100%`) and `line_5` brightness values. On wake-up, these were pushed back to the display. Fixed by (1) firing `clear_overlay` to HA on the first API reconnect after a deep sleep wake, and (2) extending the `is_stale_feedback` filter to also suppress `MAX`/`MIN` overlay texts, not just `Bright`/`Vol`.
- **False 'Failed!' Output on Slower Networks**: Increased the blueprint `wait_template` timeout safely up from 3 seconds to 5 seconds to completely eliminate frustrating "Failed!" overlay messages when interacting with gracefully fading Zigbee lights or Matter-over-Thread networks, which occasionally take ~3.5s to formally report a state change.
- **Outdated Device States on Wakeup**: Patched a UI desync vector where waking the screen (standby or deep sleep) would display stale cached information from hours prior if devices were modified elsewhere (like the HA App). Implemented an implicit `refresh` event boundary to force an unprompted state synchronization on wakeup.

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
