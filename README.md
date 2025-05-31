# 🌿 Eco Mode for Thermostats in Home Assistant

[![PayPal Donation](https://img.shields.io/badge/PayPal-Donate-blue?logo=paypal)](https://www.paypal.com/donate/?hosted_button_id=AAWFZVF2XCP5A)
![Script](https://img.shields.io/badge/logo-yaml-green?logo=yaml)
[![Bulgarian](https://img.shields.io/badge/BULGARIAN-language-green?logo=translate&labelColor=gray&style=flat-square&link=https://example.com/bg)](BG.md)
[![English](https://img.shields.io/badge/ENGLISH-language-green?logo=translate&labelColor=gray&style=flat-square&link=https://example.com/en)](README.md)

This project provides an automation solution for smart thermostat control in Home Assistant, optimizing energy efficiency while maintaining comfort. The system automatically adjusts temperatures based on presence detection and time of day, reducing energy consumption without sacrificing comfort.

## 📦 CONTENTS

- [🌿 Eco Mode for Thermostats in Home Assistant](#-eco-mode-for-thermostats-in-home-assistant)
	- [📦 CONTENTS](#-contents)
	- [🔧 Installation](#-installation)
		- [Creating a Template Switch](#creating-a-template-switch)
			- [Manual Configuration (YAML):](#manual-configuration-yaml)
			- [Parameter Explanation:](#parameter-explanation)
			- [Graphical Interface (GUI):](#graphical-interface-gui)
			- [Additional Features:](#additional-features)
			- [Tips:](#tips)

## 🔧 Installation

### Creating a Template Switch

The Template Switch is a key component of this automation, enabling easy switching between normal and eco modes. Here's a detailed guide:

#### Manual Configuration (YAML):

```yaml
switch:
  - platform: template
    switches:
      eco_mode_kitchen:
        friendly_name: "Kitchen Eco Mode"
        icon_template: >-
          {% if is_state('switch.eco_mode_kitchen', 'on') %}
            mdi:leaf
          {% else %}
            mdi:leaf-off
          {% endif %}
        value_template: "{{ states('climate.thermostat_kitchen') == 'eco' }}"
        turn_on:
          - service: climate.set_temperature
            target:
              entity_id: climate.thermostat_kitchen
            data:
              temperature: "{{ states('input_number.eco_temperature_kitchen') | float }}"
          - service: notify.mobile_app_phone
            data:
              message: "Eco mode activated in kitchen"
        turn_off:
          - service: climate.set_temperature
            target:
              entity_id: climate.thermostat_kitchen
            data:
              temperature: "{{ states('input_number.normal_temperature_kitchen') | float }}"
          - service: notify.mobile_app_phone
            data:
              message: "Eco mode deactivated in kitchen"
```

#### Parameter Explanation:

1. **friendly_name**: Displayed in Home Assistant interface
2. **icon_template**: Dynamically changes icon based on state
   - `mdi:leaf` for active eco mode
   - `mdi:leaf-off` for inactive eco mode
3. **value_template**: Determines current switch state
4. **turn_on** actions:
   - Sets eco temperature from input_number helper
   - Sends phone notification
5. **turn_off** actions:
   - Restores normal temperature
   - Sends deactivation notification

#### Graphical Interface (GUI):

1. Go to `Settings` → `Automations & Scenes` → `+ Create Automation`
2. Select "Use visual editor"
3. Add Template Switch:
   - Click "+ Add Trigger" → Select "Software"
   - Choose "Template" as platform
4. Configure:
   - **Name**: "Kitchen Eco Mode"
   - **Icon**: Use dynamic icon as in YAML example
   - **State**: `{{ states('climate.thermostat_kitchen') == 'eco' }}`
5. Set actions:
   - **When turned on**:
     - Call service `climate.set_temperature`
     - Set your thermostat's entity_id
     - Temperature: `{{ states('input_number.eco_temperature_kitchen') | float }}`
     - Add notification (optional)
   - **When turned off**:
     - Call same service
     - Set normal temperature
     - Add deactivation notification

#### Additional Features:

1. **Adding conditions**:
   ```yaml
   turn_on:
     - condition: state
       entity_id: binary_sensor.home_occupied
       state: 'off'
     - service: climate.set_temperature
       ...
   ```
   (Activates eco mode only when home is unoccupied)

2. **Change logging**:
   ```yaml
   turn_on:
     - service: system_log.write
       data:
         message: "Eco mode activated at {{ now() }}"
         level: info
   ```

3. **Calendar integration**:
   ```yaml
   value_template: >-
     {% if states('calendar.work_schedule') == 'on' %}
       {{ true }}
     {% else %}
       {{ states('climate.thermostat_kitchen') == 'eco' }}
     {% endif %}
   ```

#### Tips:

- Test the switch before integrating into automations
- Add backup temperatures in the template for error handling
- Use `unique_id` when creating multiple similar switches
- Consider adding delays before mode changes

[![Add Template Switch](/img/button%20ADD%20Temlate.svg)](https://my.home-assistant.io/redirect/config_flow_start?domain=template)

---
> [!TIP]
> If you like this project, check out [more of my repositories here](https://github.com/Bacard1?tab=repositories).  
> If you encounter issues or have questions, feel free to contact me.