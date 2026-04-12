# HARem Codebase Review — Maintainability

> **Scope**: `remote_controller.yaml`, `blueprints/harem_remote.yaml`, `home_assistant/harem_package.yaml`
> **Summary**: The project is well-structured and well-commented for its domain. Most issues stem from the constraints of the ESPHome + HA YAML format rather than poor design choices. The key maintainability risks are **display layout magic numbers**, **stale-feedback filter fragility**, **automation duplication**, and the **300-line display lambda**.

---

## 🟢 What Works Well

- **Clear script names** (`idle_timer`, `send_click_event`, `send_clear_overlay_event`, etc.) — intent is obvious without reading the body.
- **`on_boot` resets all UI state** — prevents stuck screens after OTA, crash, or unexpected reboot.
- **`restore_value: no`** on all UI state globals — correct; prevents flicker and stale state on wake.
- **Good inline comments** throughout the display lambda and button handler.
- **Blueprint approach** is the right architectural choice — logic lives in HA, firmware stays dumb.
- **Commit history** is clean and descriptive.

---

## 🔴 High-Priority Issues

### 1. Display Lambda Magic Numbers (Layout Brittleness)
**File**: `remote_controller.yaml` — display lambda

The 5-line layout is hardcoded with raw pixel Y values scattered across ~300 lines:

```cpp
it.printf(10, 6,  ...);  // line 1
it.printf(10, 17, ...);  // line 2
it.print (10, 28, ...);  // line 3 (header font)
it.printf(10, 41, ...);  // line 4
it.printf(10, 52, ...);  // line 5
```

If you ever change a font size, adjust spacing, or add a new UI row, every Y coordinate must be hunted down manually. There's also no source-of-truth diagram.

**Recommendation**: Define constants at the top of the lambda (or as `#define` in a `globals` C++ block):
```cpp
const int LINE_Y[] = {6, 17, 28, 41, 52};
const int LINE_X = 10;
```

---

### 2. `is_stale_feedback` Filter is Text-Prefix Dependent
**File**: `remote_controller.yaml` line ~935

```cpp
bool is_stale_feedback = (!id(control_mode) && (
  text.find("Bright") == 0 || text.find("Vol") == 0 ||
  text.find("MAX") == 0    || text.find("MIN") == 0
));
```

This filter is **tightly coupled** to the exact strings the blueprint sets in `harem_overlay`. If the blueprint's overlay strings change (e.g., "Volume 75%" instead of "Vol 75%"), the firmware silently stops filtering them — and the stale overlay shows up again after sleep. This is the same bug we fixed today, but it will keep recurring every time a new overlay type is added.

**Recommendation**: The protocol should own this, not text matching. Add a second HA entity (`input_text.harem_overlay_type: "control" | "feedback" | ""`) that the blueprint sets alongside the overlay text. The firmware then checks type instead of text content. This makes the contract explicit.

---

### 3. `processed_rotate_hold` Branch is Dead Code
**File**: `remote_controller.yaml` lines 328–338

```cpp
if (id(control_mode)) {
    if (id(processed_rotate_hold)) {
       id(processed_rotate_hold) = false;
       return;  // ← identical
    }
} else {
    if (id(processed_rotate_hold)) {
       id(processed_rotate_hold) = false;
       return;  // ← identical
    }
}
```

Both branches are **byte-for-byte identical**. The `if/else` around `control_mode` does nothing. This suggests an old divergence that was merged but never cleaned up.

**Recommendation**: Collapse to:
```cpp
if (id(processed_rotate_hold)) {
    id(processed_rotate_hold) = false;
    return;
}
```

---

### 4. `harem_package.yaml` Automation is Outdated & Confusing
**File**: `home_assistant/harem_package.yaml`

This file contains a **full copy** of the menu automation — but it's not what HA runs (the blueprint is). Today's session proved this is a real trap: the 10s timeout bug was only in the blueprint, but `harem_package.yaml` had its own 10s timeout that looked "fixed" when it wasn't.

The package file also lacks: `is_instant` logic, `MAX`/`MIN` handling, `clear_overlay` handler, `rotate_cw_hold` / `rotate_ccw_hold`, and Guest Mode guest-entity filtering.

**Recommendation**: Remove the `automation:` block from `harem_package.yaml` entirely. The file should only contain the **helpers** (`input_text`, `input_number`, `input_boolean`). Put a clear comment saying automation is handled by the blueprint.

---

## 🟡 Medium-Priority Issues

### 5. Boot Low-Battery Warning is Dead Code
**File**: `remote_controller.yaml` lines 39–46

```yaml
- lambda: |-
    if (id(battery_percent) < 90.0) {
      id(show_low_battery_warning) = true;
```

`battery_percent` has `initial_value: '100.0'` and `restore_value: no`, so it's always `100.0` at boot — this check never fires. The warning IS correctly shown by the ADC sensor callback (`< 20.0`), but this boot check is dead code.

**Recommendation**: Remove the boot `< 90.0` check, or if there was intention to show it on boot, use `restore_value: yes` on `battery_percent`.

---

### 6. Blueprint Recomputes Entity List Twice Per Action
**File**: `blueprints/harem_remote.yaml`

The full area/entity list is computed in **Phase 1 (HANDLE ACTION)** and then recomputed again in **Phase 2 (UPDATE DISPLAY)** immediately after. HA's Jinja2 template engine is not trivial — this is repeated work on every encoder rotation.

**Recommendation**: Pass the computed list as a variable into Phase 2, or restructure so the list is computed once. This would also make the code shorter.

---

### 7. `queued` Mode with Fast Encoder Rotation
**File**: `blueprints/harem_remote.yaml` line 69–70

```yaml
mode: queued
max: 20
```

Every encoder tick fires a full HA automation run. At fast rotation speeds (5–10 ticks/second), 20 runs can queue quickly. Each run does multiple `input_text.set_value` calls. If HA is under load, the queue drains slowly and the display lags significantly behind the physical encoder.

**Recommendation**: Consider `mode: restart` for `next`/`prev` navigation actions (so only the last position matters) and `queued` only for `click`, `back`, and `toggle_guest`. This would require splitting into two automations or using a `choose`-based dispatch per action type.

---

### 8. Settings Index Mapping Duplicated in Comments
**File**: `remote_controller.yaml`

The settings index legend (`0:Bright, 1:Standby, 2:Sleep, 3:Animations, 4:Guest, 5:LeftHand, 6:ChangePIN, 7:WiFi, 8:IP, 9:Exit`) appears in at least **3 places** as inline comments:
- Clockwise handler comment
- Anticlockwise handler comment
- Settings display loop

If you ever add or reorder a settings item, all three need updating.

**Recommendation**: Define a `#define` or a comment block at the top of the display section as the single canonical definition. Reference that in other places.

---

### 9. No Protocol Versioning Between Firmware and Blueprint
The firmware and blueprint communicate via the `esphome.remote_action` event and `input_text.*` helpers. There's no handshake or version check. If a user updates only the firmware (via OTA from the GitHub release) but not the blueprint, features like `is_instant`, `clear_overlay`, or `boot_clear_done` will silently break or be ignored with no error message.

**Recommendation**: Add a `text_sensor` on the ESPHome side that publishes the firmware version, and have the blueprint check compatibility on `rotate_cw_hold` (where the mismatch is most critical). Alternatively, document the minimum blueprint version required for each firmware release in the CHANGELOG.

---

## 🔵 Low-Priority / Cosmetic

| # | Issue | Location |
|---|---|---|
| 10 | `on_disconnect` logs a message but shows nothing on screen — user sees a frozen display | `remote_controller.yaml` wifi section |
| 11 | `send_clear_overlay_event` also restarts `idle_timer` — two separate concerns in one script | `remote_controller.yaml` scripts |
| 12 | `harem_package.yaml` timeout fix (10s→3s) we made today is unused but should stay in sync | `home_assistant/harem_package.yaml` |
| 13 | `docs/walkthrough.md` and `docs/generic_menu_setup.md` don't reflect current blueprint/state | `docs/` |
| 14 | No local display of WiFi disconnect state (only shown in hotspot/AP mode) | Display lambda |

---

## Summary Table

| Severity | Count | Quick Wins |
|---|---|---|
| 🔴 High | 4 | Dead `if/else` on `processed_rotate_hold` (10 min fix); Remove dead boot battery check (2 min fix) |
| 🟡 Medium | 4 | Strip old automation from `harem_package.yaml` (30 min); Layout constants (1 hr) |
| 🔵 Low | 5 | Cosmetic / documentation |

**Overall verdict**: The codebase is maintainable for its current scale, but has a clear **duplication trap** (`harem_package.yaml` automation) and a **fragility trap** (`is_stale_feedback` text matching). These two are the biggest risks for future bugs. The display lambda length is the biggest readability concern.
