# 🌿 Eco Mode for Thermostats in Home Assistant  

[![Home Assistant](https://img.shields.io/badge/🏠_Home_Assistant-41BDF5?logo=homeassistant)](https://www.home-assistant.io/) [![Donate via PayPal](https://img.shields.io/badge/PayPal-Donate-blue?logo=paypal)](https://www.paypal.com/donate/?hosted_button_id=AAWFZVF2XCP5A)
![Script](https://img.shields.io/badge/logo-yaml-green?logo=yaml)
[![Български](https://img.shields.io/badge/BG_Български-език-green?logo=translate&labelColor=gray&style=flat-square&link=https://example.com/bg
)](BG.md)
[![Deutch](https://img.shields.io/badge/DE_Deutsche-sprache-green?logo=translate&labelColor=gray&style=flat-square&link=https://example.com/bg
)](DE.md)
[![English](https://img.shields.io/badge/EN_English-language-green?logo=translate&labelColor=gray&style=flat-square&link=https://example.com/bg)](README.md)

This project provides automation for managing radiator thermostats in **Home Assistant** to optimize energy efficiency. The system automatically lowers the temperature when no one is home or during nighttime, saving energy without compromising comfort. 🌡️💡  

---

## 📦 Contents  

- [🌿 Eco Mode for Thermostats in Home Assistant](#-eco-mode-for-thermostats-in-home-assistant)
  - [📦 Contents](#-contents)
  - [🌟 Features](#-features)
  - [📋 Requirements](#-requirements)
  - [🛠️ Installation](#️-installation)
    - [1. Create Helper Inputs for Temperature](#1-create-helper-inputs-for-temperature)
      - [Example code for manual creation:](#example-code-for-manual-creation)
    - [2. Create a Template Switch](#2-create-a-template-switch)
      - [Example code for manual creation:](#example-code-for-manual-creation-1)
      - [Alternative GUI Setup:](#alternative-gui-setup)
  - [💡 Tips](#-tips)

---

## 🌟 Features  

- **Automatic temperature adjustment** based on presence and time of day.  
- **Eco Mode** using **Template Switch**:  
  - Lowers temperature when no one is home or during nighttime.  
  - Restores the previous temperature setting when turned off.  
- **Integration** with all popular thermostats supported by Home Assistant (Zigbee, Z-Wave, WiFi, etc.).  

---

## 📋 Requirements  

- **Home Assistant** (recommended version 2023.x or newer).  
- Integrated **thermostats** in the system (e.g., Zigbee, Z-Wave, or WiFi thermostats).  

---

## 🛠️ Installation  

### 1. Create Helper Inputs for Temperature  

You need to create two `input_number` helpers:  
- **First helper** – for setting the normal thermostat temperature.  
- **Second helper** – for setting the Eco Mode temperature.  

#### Example code for manual creation:  

```yaml
input_number:
  - normal_temperature_kitchen:
      name: "Normal Temperature (Kitchen)"
      min: 5
      max: 30
      step: 0.5
      unit_of_measurement: "°C"
  - eco_mode_temperature_kitchen:
      name: "Eco Mode Temperature (Kitchen)"
      min: 5
      max: 20
      step: 0.5
      unit_of_measurement: "°C"
```

📌 **Note:** You can also create these helpers via the Home Assistant GUI under **"Helpers"**.  

---

### 2. Create a Template Switch  

The Template Switch will handle switching between normal and Eco Mode.  

#### Example code for manual creation:  

```yaml
switch:
  - platform: template
    switches:
      eco_mode_kitchen:
        friendly_name: "Eco Mode (Kitchen)"
        value_template: "{{ false }}"  # Can be adapted as needed
        turn_on:
          service: climate.set_temperature
          target:
            entity_id: climate.thermostat_kitchen
          data:
            temperature: "{{ states('input_number.eco_mode_temperature_kitchen') | float }}"
        turn_off:
          service: climate.set_temperature
          target:
            entity_id: climate.thermostat_kitchen
          data:
            temperature: "{{ states('input_number.normal_temperature_kitchen') | float }}"
```

#### Alternative GUI Setup:  

1. Click the button:  
   [![Add template](/img/button%20ADD%20Temlate.svg)](https://my.home-assistant.io/redirect/config_flow_start?domain=template)  
2. Select **Switch**.  
   ![img](/img/Template001.png)  
3. Configure the actions for turning on and off:  
   - **Turn On:** Sets the temperature to Eco Mode.  
   - **Turn Off:** Restores the normal temperature.  
   ![img](/img/Template002.png)  
   ![img](/img/Template003.png)  

---
---
## 💡 Tips  

- If you like this project, check out [**more of my repositories here**](https://github.com/Bacard1?tab=repositories).  
- If you encounter issues or have questions, feel free to contact me! 📩  
