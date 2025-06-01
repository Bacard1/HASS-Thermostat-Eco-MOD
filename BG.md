# 🌿 Еко Мод за термостати в Home Assistant  

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?color=ff00d8)](https://opensource.org/licenses/MIT)
![GitHub last commit](https://img.shields.io/github/last-commit/Bacard1/HASS-Thermostat-Eco-MOD.svg?color=ff00d8)
[![hacs_badge](https://img.shields.io/badge/HACS-2025.5.3-orange.svg?color=ff00d8)](https://github.com/hacs/integration)

[![Home Assistant](https://img.shields.io/badge/.-HOME_ASSISTANT-blue?logo=homeassistant)](https://www.home-assistant.io/) 
[![Donate via PayPal](https://img.shields.io/badge/PayPal-DONATE-blue?logo=paypal)](https://www.paypal.com/donate/?hosted_button_id=AAWFZVF2XCP5A)
![Script](https://img.shields.io/badge/Script-YAML-blue?logo=yaml)

[![Български](https://img.shields.io/badge/BG-ЕЗИК-gree?logo=translate&labelColor=gray&style=flat-square&link=https://example.com/bg
)](BG.md)
[![Deutch](https://img.shields.io/badge/DE-SPRACHE-gree?logo=translate&labelColor=gray&style=flat-square&link=https://example.com/bg
)](DE.md)
[![English](https://img.shields.io/badge/EN-LANGUAGE-gree?logo=translate&labelColor=gray&style=flat-square&link=https://example.com/bg)](README.md)

Този проект предоставя автоматизация за управление на термоглавки на радиатори в **Home Assistant** с цел оптимизиране на енергийната ефективност. Системата автоматично намалява температурата, когато никой не е в къщи или през нощта, като по този начин спестява енергия без компромис с комфорта. 🌡️💡  

---

## 📦 Съдържание  

- [🌿 Еко Мод за термостати в Home Assistant](#-еко-мод-за-термостати-в-home-assistant)
  - [📦 Съдържание](#-съдържание)
  - [🌟 Характеристики](#-характеристики)
  - [📋 Изисквания](#-изисквания)
  - [🛠️ Инсталация](#️-инсталация)
    - [1. Създаване на помощници за температура](#1-създаване-на-помощници-за-температура)
      - [Примерен код за ръчно създаване:](#примерен-код-за-ръчно-създаване)
    - [2. Създаване на Template Switch](#2-създаване-на-template-switch)
      - [Примерен код за ръчно създаване:](#примерен-код-за-ръчно-създаване-1)
      - [Алтернативно създаване през графичния интерфейс:](#алтернативно-създаване-през-графичния-интерфейс)
  - [💡 Съвети](#-съвети)

---

## 🌟 Характеристики  

- **Автоматично регулиране на температурата** според присъствието и времето от деня.  
- **Режим "Еко"** с помощта на **Template Switch**:  
  - Намалява температурата, когато никой не е в къщи или през нощта.  
  - Възстановява предходно зададената температура при изключване на режима.  
- **Интеграция** с всички популярни термоглавки, поддържани от Home Assistant (Zigbee, Z-Wave, WiFi и др.).  

---

## 📋 Изисквания  

- **Home Assistant** (препоръчително версия 2023.x или по-нова).  
- Интегрирани **термоглавки** в системата (напр. Zigbee, Z-Wave или WiFi термостати).  

---

## 🛠️ Инсталация  

### 1. Създаване на помощници за температура  

Необходимо е да създадете два помощника от тип `input_number`:  
- **Първи помощник** – за задаване на нормалната температура на термоглавката.  
- **Втори помощник** – за задаване на температурата в режим "Еко".  

#### Примерен код за ръчно създаване:  

```yaml
input_number:
  - normal_themperature_kuche:
      name: "Нормална температура (кухня)"
      min: 5
      max: 30
      step: 0.5
      unit_of_measurement: "°C"
  - eco_mod_themperature_kuche:
      name: "Еко мод температура (кухня)"
      min: 5
      max: 20
      step: 0.5
      unit_of_measurement: "°C"
```

📌 **Забележка:** Можете да създадете помощници и през графичния интерфейс на Home Assistant, като използвате менюто **"Помощници"**.  

---

### 2. Създаване на Template Switch  

Template Switch ще управлява превключването между нормален и еко режим.  

#### Примерен код за ръчно създаване:  

```yaml
switch:
  - platform: template
    switches:
      eco_mode_kuche:
        friendly_name: "Еко мод (кухня)"
        value_template: "{{ false }}"  # Може да се адаптира според нуждите
        turn_on:
          service: climate.set_temperature
          target:
            entity_id: climate.thermostat_kuche
          data:
            temperature: "{{ states('input_number.eco_mod_themperature_kuche') | float }}"
        turn_off:
          service: climate.set_temperature
          target:
            entity_id: climate.thermostat_kuche
          data:
            temperature: "{{ states('input_number.normal_themperature_kuche') | float }}"
```

#### Алтернативно създаване през графичния интерфейс:  

1. Натиснете бутона:  
   [![Add template](/img/button%20ADD%20Temlate.svg)](https://my.home-assistant.io/redirect/config_flow_start?domain=template)  
2. Изберете **Switch**.  
   ![img](/img/Template001.png)  
3. Задайте действията при включване и изключване:  
   - **Включване:** Задава температурата на еко режим.  
   - **Изключване:** Възстановява нормалната температура.  
   ![img](/img/Template002.png)  
   ![img](/img/Template003.png)  

---
---
## 💡 Съвети  

- Ако този проект ви е харесал, [**тук**](https://github.com/Bacard1?tab=repositories) ще намерите още интересни проекти от мен.  
- Ако срещате затруднения или имате въпроси, не се колебайте да се свържете с мен! 📩  
