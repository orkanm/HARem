# Generic Menu Setup Guide

This guide turns your Remote into a universal browser for your Home Assistant "Areas".
It requires **no maintenance**—if you add a device to a Room in Home Assistant, it appears on the remote automatically.

## 1. Create Helpers (Home Assistant)
Go to **Settings > Devices & Services > Helpers** and create these:

| Type | Name | Purpose |
| :--- | :--- | :--- |
| **Text** | `harem_menu_path` | Stores current location. Default: `ROOT`. |
| **Number** | `harem_menu_index` | Tracks scroll position. Min: 0, Max: 100, Step: 1. |
## 1. Create Helpers
In Home Assistant > Settings > Devices & Services > Helpers:
1.  **Text Helper**: `input_text.harem_menu_path` (Name: "Remote Menu Path", Max: 255)
2.  **Number Helper**: `input_number.harem_menu_index` (Name: "Remote Menu Index", Min: 0, Max: 1000, Step: 1)
3.  **Text Helper**: `input_text.harem_line_1` (Name: "Remote Line 1")
4.  **Text Helper**: `input_text.harem_line_2` (Name: "Remote Line 2")
5.  **Text Helper**: `input_text.harem_line_3` (Name: "Remote Line 3")
6.  **Text Helper**: `input_text.harem_line_4` (Name: "Remote Line 4")
7.  **Text Helper**: `input_text.harem_line_5` (Name: "Remote Line 5")
8.  **Text Helper**: `input_text.harem_line_3_status` (Name: "Remote Line 3 Status")

## 2. Create Automation
**Name**: "Remote: Generic Menu Controller"
**Mode**: "Queued" (Max: 10)

```yaml
alias: "Remote: Generic Menu Controller"
description: "Handles generic menu navigation for ESPHome Remote (5 Lines)"
mode: queued
max: 10
trigger:
  - platform: event
    event_type: esphome.remote_action
condition: []
action:
  # 1. HANDLE ACTION
  - variables:
      action: "{{ trigger.event.data.action }}" # next, prev, click, back
      path: "{{ states('input_text.harem_menu_path') }}" 
      idx: "{{ states('input_number.harem_menu_index') | int }}"
      
  - variables:
      list_size: >
        {% if path == 'ROOT' or path == '' %} {{ areas() | length }}
        {% else %} {{ area_entities(path) | select('match', '^(light|switch|input_boolean|scene|script|button|cover|fan|lock|media_player)[.]') | list | length }} {% endif %}

  - choose:
      # ROTATE -> Change Index
      - conditions: "{{ action == 'next' and list_size > 0 }}"
        sequence:
          - service: input_number.set_value
            target: {entity_id: input_number.harem_menu_index}
            data: {value: "{{ (idx + 1) % list_size }}"}
            
      - conditions: "{{ action == 'prev' and list_size > 0 }}"
        sequence:
          - service: input_number.set_value
            target: {entity_id: input_number.harem_menu_index}
            data: {value: "{{ (idx - 1) % list_size }}"}

      # CLICK -> Enter/Toggle
      - conditions: "{{ action == 'click' and list_size > 0 }}"
        sequence:
          - if: "{{ path == 'ROOT' or path == '' }}"
            then:
              - variables:
                  areas_list: "{{ areas() | sort }}"
                  selected_area: "{{ areas_list[idx % list_size] }}"
              - service: input_text.set_value
                target: {entity_id: input_text.harem_menu_path}
                data: {value: "{{ selected_area }}"}
              - service: input_number.set_value
                target: {entity_id: input_number.harem_menu_index}
                data: {value: 0}
            else:
              - variables:
                  entities: "{{ area_entities(path) | select('match', '^(light|switch|input_boolean|scene|script|button|cover|fan|lock|media_player)[.]') | sort }}"
                  selected_entity: "{{ entities[idx % list_size] }}"
              - service: homeassistant.toggle
                target: {entity_id: "{{ selected_entity }}"}
              - delay: "00:00:00.5" # Wait for state change to propagate

      # BACK -> Exit Room
      - conditions: "{{ action == 'back' and path != 'ROOT' }}"
        sequence:
          - variables:
              sorted_areas: "{{ areas() | sort }}"
              # Find index of current room to restore position
              restore_idx: "{{ sorted_areas.index(path) if path in sorted_areas else 0 }}"
          - service: input_number.set_value
            target: {entity_id: input_number.harem_menu_index}
            data: {value: "{{ restore_idx }}"}
          - service: input_text.set_value
            target: {entity_id: input_text.harem_menu_path}
            data: {value: "ROOT"}

  # 2. UPDATE DISPLAY (5 LINES)
  - variables:
      path: "{{ states('input_text.harem_menu_path') }}" 
      idx: "{{ states('input_number.harem_menu_index') | int }}"
  
  - if: "{{ path == 'ROOT' or path == '' }}"
    then:
      # AREAS LIST
      - variables:
          list: "{{ areas() | sort }}"
      - if: "{{ list | length > 0 }}"
        then:
          - service: input_text.set_value
            target: {entity_id: input_text.harem_line_1}
            # Hide Line 1 if list size <= 2
            data: {value: "{{ area_name(list[(idx - 2) % list|length]) if list|length > 2 else '' }}"}
          - service: input_text.set_value
            target: {entity_id: input_text.harem_line_2}
            # Hide Line 2 if list size == 1
            data: {value: "{{ area_name(list[(idx - 1) % list|length]) if list|length > 1 else '' }}"}
          - service: input_text.set_value
            target: {entity_id: input_text.harem_line_3}
            data: {value: "> {{ area_name(list[idx % list|length]) }}"}
          - service: input_text.set_value
            target: {entity_id: input_text.harem_line_3_status}
            data: {value: ""}
          - service: input_text.set_value
            target: {entity_id: input_text.harem_line_4}
            # Hide Line 4 if list size == 1
            data: {value: "{{ area_name(list[(idx + 1) % list|length]) if list|length > 1 else '' }}"}
          - service: input_text.set_value
            target: {entity_id: input_text.harem_line_5}
            # Hide Line 5 if list size <= 2
            data: {value: "{{ area_name(list[(idx + 2) % list|length]) if list|length > 2 else '' }}"}
        else:
           - service: input_text.set_value
             target: {entity_id: input_text.harem_line_3}
             data: {value: "No Areas Found"}
           - service: input_text.set_value
             target: {entity_id: input_text.harem_line_3_status}
             data: {value: ""}

    else:
      # DEVICES LIST
      - variables:
          list: "{{ area_entities(path) | select('match', '^(light|switch|input_boolean|scene|script|button|cover|fan|lock|media_player)[.]') | sort }}"
      - if: "{{ list | length > 0 }}"
        then:
          - service: input_text.set_value
            target: {entity_id: input_text.harem_line_1}
            # Hide Line 1 if list size <= 2
            data: {value: "{{ state_attr(list[(idx - 2) % list|length], 'friendly_name') if list|length > 2 else '' }}"}
          - service: input_text.set_value
            target: {entity_id: input_text.harem_line_2}
            # Hide Line 2 if list size == 1
            data: {value: "{{ state_attr(list[(idx - 1) % list|length], 'friendly_name') if list|length > 1 else '' }}"}
          - service: input_text.set_value
            target: {entity_id: input_text.harem_line_3}
            data: {value: "> {{ state_attr(list[idx % list|length], 'friendly_name') }}"}
          - service: input_text.set_value
            target: {entity_id: input_text.harem_line_3_status}
            data: {value: "({{ states(list[idx % list|length]) | truncate(12, True, '..') }})" }
          - service: input_text.set_value
            target: {entity_id: input_text.harem_line_4}
            # Hide Line 4 if list size == 1
            data: {value: "{{ state_attr(list[(idx + 1) % list|length], 'friendly_name') if list|length > 1 else '' }}"}
          - service: input_text.set_value
            target: {entity_id: input_text.harem_line_5}
            # Hide Line 5 if list size <= 2
            data: {value: "{{ state_attr(list[(idx + 2) % list|length], 'friendly_name') if list|length > 2 else '' }}"}
        else:
           - service: input_text.set_value
             target: {entity_id: input_text.harem_line_3}
             data: {value: "Empty Room"}
           - service: input_text.set_value
             target: {entity_id: input_text.harem_line_3_status}
             data: {value: ""}
```

## How to use
1.  **Start**: Ensure `input_text.harem_menu_path` is set to `ROOT`.
2.  **Navigate**: Rotate to scroll through Rooms.
3.  **Enter**: Click to enter a Room (lists devices in that Area).
4.  **Control**: Click to toggle a device.
5.  **Exit**: Double-Click the knob to go back to the Room list.
