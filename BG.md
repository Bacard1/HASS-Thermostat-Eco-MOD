# 🌿 Еко Режим за Термостати в Home Assistant

[![PayPal Дарение](https://img.shields.io/badge/PayPal-Дари-синьо?logo=paypal)](https://www.paypal.com/donate/?hosted_button_id=AAWFZVF2XCP5A)
![Скрипт](https://img.shields.io/badge/logo-yaml-green?logo=yaml)
[![English](https://img.shields.io/badge/ENGLISH-език-green?logo=translate&labelColor=gray&style=flat-square&link=https://example.com/en)](README.md)
[![Български](https://img.shields.io/badge/БЪЛГАРСКИ-език-green?logo=translate&labelColor=gray&style=flat-square&link=https://example.com/bg)](BG.md)

Този проект предлага автоматизация за управление на термостати в Home Assistant, която оптимизира енергийната ефективност, като автоматично регулира температурата според присъствието и времето от деня.

## 📦 СЪДЪРЖАНИЕ

- [🌿 Еко Режим за Термостати в Home Assistant](#-еко-режим-за-термостати-в-home-assistant)
  - [📦 СЪДЪРЖАНИЕ](#-съдържание)
  - [🔧 Инсталация](#-инсталация)
    - [Създаване на Template Switch](#създаване-на-template-switch)
      - [Ръчна конфигурация (YAML):](#ръчна-конфигурация-yaml)
      - [Обяснение на параметрите:](#обяснение-на-параметрите)
      - [Графичен интерфейс (GUI):](#графичен-интерфейс-gui)
      - [Допълнителни възможности:](#допълнителни-възможности)
      - [Съвети:](#съвети)

## 🔧 Инсталация

### Създаване на Template Switch

Template Switch е ключов елемент от автоматизацията, който позволява лесно превключване между нормален и еко режим.

#### Ръчна конфигурация (YAML):

```yaml
switch:
  - platform: template
    switches:
      eco_mode_kitchen:
        friendly_name: "Еко Режим Кухня"
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
              message: "Активиран еко режим в кухнята"
        turn_off:
          - service: climate.set_temperature
            target:
              entity_id: climate.thermostat_kitchen
            data:
              temperature: "{{ states('input_number.normal_temperature_kitchen') | float }}"
          - service: notify.mobile_app_phone
            data:
              message: "Деактивиран еко режим в кухнята"
```

#### Обяснение на параметрите:

1. **friendly_name**: Име, което се показва в интерфейса
2. **icon_template**: Автоматично сменя иконата:
   - `mdi:leaf` за активен еко режим
   - `mdi:leaf-off` за изключен еко режим
3. **value_template**: Определя състоянието на превключвателя
4. **turn_on** действия:
   - Задава еко температура
   - Изпраща известие
5. **turn_off** действия:
   - Възстановява нормална температура
   - Изпраща известие

#### Графичен интерфейс (GUI):

1. Отидете в `Настройки` → `Автоматизации и сценарии` → `+ Създай автоматизация`
2. Изберете "Графичен редактор"
3. Добавете Template Switch:
   - Кликнете "+ Добави тригер" → Изберете "Софтуерен"
   - Изберете платформа "Template"
4. Конфигурирайте:
   - **Име**: "Еко Режим Кухня"
   - **Икона**: Използвайте динамична икона
   - **Състояние**: `{{ states('climate.thermostat_kitchen') == 'eco' }}`

#### Допълнителни възможности:

1. **Условия**:
   ```yaml
   turn_on:
     - condition: state
       entity_id: binary_sensor.home_occupied
       state: 'off'
   ```

2. **Логване**:
   ```yaml
   turn_on:
     - service: system_log.write
       data:
         message: "Активиран еко режим в {{ now() }}"
   ```

#### Съвети:

- Тествайте преди интеграция
- Добавете резервни температури
- Използвайте `unique_id` за множество превключватели

[![Добави Template Switch](/img/button%20ADD%20Temlate.svg)](https://my.home-assistant.io/redirect/config_flow_start?domain=template)

---
> [!TIP]
> Ако харесвате този проект, вижте [още проекти тук](https://github.com/Bacard1?tab=repositories).  
> За въпроси и помощ, не се колебайте да се свържете с мен.