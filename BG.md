![BANNER]()
# Еко Мод на термостати в Home Assistant
[![PayPal дарение](https://img.shields.io/badge/PayPal-Дари-синьо?logo=paypal)](https://www.paypal.com/donate/?hosted_button_id=AAWFZVF2XCP5A)
![Скрипт](https://img.shields.io/badge/logo-yaml-green?logo=yaml)
[![Български](https://img.shields.io/badge/БЪЛГАРСКИ-език-green?logo=translate&labelColor=gray&style=flat-square&link=https://example.com/bg)](BG.md)
[![English](https://img.shields.io/badge/ENGLICH-language-green?logo=translate&labelColor=gray&style=flat-square&link=https://example.com/en)](README.md)

Този проект предоставя автоматизация за управление на термоглавки на радиатори в Home Assistant с цел оптимизиране на енергийната ефективност. Системата автоматично намалява температурата, когато никой не е в къщи или през нощта, като по този начин спестява енергия без компромис с комфорта.

---

## 📦 СЪДЪРЖАНИЕ

- [Име на проекта](#име на проекта)

---


## Характеристики
Автоматично регулиране на температурата според присъствието и времето от деня
- Режим еко с помочта на Template Switch:
Eco режим (намалена температура, когато никой не е в къщи  и през нощтта) и при изклщчването му възтановява предишно зададената температура Интеграция с всички популярни термоглавки, поддържани от Home Assistant

## Изисквания
Home Assistant (препоръчително версия 2023.x или по-нова)
Интегрирани термоглавки в системата (напр. Zigbee, Z-Wave или WiFi термостати)

## 🛠️ Инсталация

- Създаване на Switch
Ето как той може да бъде създаден ръчно:

```yaml
switch:
  - platform: template
	switches:
	  skylight:
		value_template: ""
		turn_on:
		  action: better_thermostat.set_temp_target_temperature
		  data:
		   temperature: "{{ states('input_number.eco_themperature_kuche') | float }}"
		  metadata: {}
		  target:
		   entity_id: climate.thermostat_kuche
		turn_off:
		  action: better_thermostat.restore_saved_target_temperature
			data: {}
		  metadata: {}
		  target:
			entity_id: climate.thermostat_kuche
		  action: climate.set_temperature
			data:
		  temperature: "{{ states('input_number.normal_themperature_kuche') | float }}"
		  metadata: {}
		  target:
			entity_id: climate.thermostat_kuche
```
Или натиснете бутонът и използвайте графичният интерфейс

[![Add template](/img/button%20ADD%20Temlate.svg)](https://my.home-assistant.io/redirect/config_flow_start?domain=template)

> [!TIP]
> Ако този проект ви е харесъл, [ТУК](https://github.com/Bacard1?tab=repositories) ще намерите още интересни гранилища направени от мен.<br>
> Ако срещате затруднения или имате въпроси не се колебайте да се свържете с мен.


> [!TIP]
> If you like this project, check out [more of my repositories here](https://github.com/Bacard1?tab=repositories).  
> If you need help or have questions, feel free to contact me.