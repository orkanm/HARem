# Generic Menu Setup Guide

This guide turns your Remote into a universal browser for your Home Assistant "Areas".
It requires **no maintenance**—if you add a device to a Room in Home Assistant, it appears on the remote automatically.

> [!TIP]
> **New! One-Click Setup**
> * **[Download One-Click Package](file:///home/orquitto/Workspace/HARem/home_assistant/harem_package.yaml)** - Best for power users. Drop one file in, everything works.
> * **[Import Blueprint](file:///home/orquitto/Workspace/HARem/blueprints/harem_remote.yaml)** - Best for UI users. Pick your helpers from a dropdown.

---

## 1. Create Helpers (Choose Package or Manual)

### ✅ Option A: Drop-in Package (Recommended)
The fastest method. Creates all 11 required helpers instantly.
1. Download [harem_package.yaml](file:///home/orquitto/Workspace/HARem/home_assistant/harem_package.yaml) and place it in your Home Assistant `/config/packages/` folder.
2. In your `configuration.yaml`, ensure you have: `homeassistant: { packages: !include_dir_named packages }`.
3. Restart Home Assistant.

### 🛠 Option B: Manual Creation
If you don't use packages, manually create these in **Settings > Devices & Services > Helpers**:
1.  **Text Helper**: `input_text.harem_menu_path` (Name: "Remote: Menu Path")
2.  **Number Helper**: `input_number.harem_menu_index` (Name: "Remote: Menu Index", Min: 0, Max: 1000, Step: 1)
3.  **Number Helper**: `input_number.harem_menu_total` (Name: "Remote: Menu Total", Min: 0, Max: 1000, Step: 1)
4.  **Text Helper**: `input_text.harem_line_1` (Name: "Remote: Line 1")
5.  **Text Helper**: `input_text.harem_line_2` (Name: "Remote: Line 2")
6.  **Text Helper**: `input_text.harem_line_3` (Name: "Remote: Line 3")
7.  **Text Helper**: `input_text.harem_line_4` (Name: "Remote: Line 4")
8.  **Text Helper**: `input_text.harem_line_5` (Name: "Remote: Line 5")
9.  **Text Helper**: `input_text.harem_line_3_status` (Name: "Remote: Line 3 Status")
10. **Text Helper**: `input_text.harem_overlay` (Name: "Remote: Overlay")
11. **Toggle Helper**: `input_boolean.harem_guest_mode` (Name: "Remote: Guest Mode")

---

## 2. Apply the Blueprint Automation

All menu logic, dimming control, and screen updates are securely handled by the official Blueprint. Do not try to write this automation manually.

1. Copy the content of [harem_remote.yaml](file:///home/orquitto/Workspace/HARem/blueprints/harem_remote.yaml) into your Home Assistant `/config/blueprints/automation/` folder.
2. Go to **Settings > Automations & Scenes > Blueprints** and find "HARem: Universal ESPHome Remote Controller".
3. Click **Create Automation**.
4. Use the dropdowns to select the Helpers you created in Step 1.
5. Select the entities under **[Bulk Control] Included Entities** that should be affected by room bulk power control (e.g. lights, switches; unselected devices like fridges will remain untouched).
6. Click **Save**.

---

## How to use
1. **Start**: Ensure `input_text.harem_menu_path` is set to `ROOT`.
2. **Navigate**: Rotate to scroll through Rooms.
3. **Bulk Control**: While highlighting a Room in the Room list:
   - **Press & Twist Clockwise**: Displays `Turn ON Room?` -> **Click knob to confirm** turning ON all allowed devices in that room.
   - **Press & Twist Counter-Clockwise**: Displays `Turn OFF Room?` -> **Click knob to confirm** turning OFF all allowed devices in that room.
   - *(Rotating away or waiting 4s automatically cancels the prompt without making changes)*
4. **Enter**: Click knob on a room when no prompt is showing to enter the Room (lists devices in that Area).
5. **Control**: Click to toggle a device. Hold & Rotate to adjust brightness / volume.
6. **Exit**: Long Press (>0.8s) the knob to go back to the Room list.
