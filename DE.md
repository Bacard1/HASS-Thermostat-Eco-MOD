# 🌿 Eco-Modus für Thermostate in Home Assistant  

[![Home Assistant](https://img.shields.io/badge/🏠_Home_Assistant-41BDF5?logo=homeassistant)](https://www.home-assistant.io/) [![Donate via PayPal](https://img.shields.io/badge/PayPal-Donate-blue?logo=paypal)](https://www.paypal.com/donate/?hosted_button_id=AAWFZVF2XCP5A)  
![Script](https://img.shields.io/badge/logo-yaml-green?logo=yaml)  
[![Български](https://img.shields.io/badge/BG_Български-език-green?logo=translate&labelColor=gray&style=flat-square&link=https://example.com/bg)](BG.md)  
[![Deutsch](https://img.shields.io/badge/DE_Deutsche-Sprache-green?logo=translate&labelColor=gray&style=flat-square&link=https://example.com/de)](DE.md)  
[![English](https://img.shields.io/badge/EN_English-language-green?logo=translate&labelColor=gray&style=flat-square&link=https://example.com/en)](README.md)  

Dieses Projekt bietet eine Automatisierung zur Steuerung von Heizkörperthermostaten in **Home Assistant** mit dem Ziel, die Energieeffizienz zu optimieren. Das System senkt automatisch die Temperatur, wenn niemand zu Hause ist oder während der Nacht, und spart so Energie ohne Komforteinbußen. 🌡️💡  

---

## 📦 Inhalt  

- [🌿 Eco-Modus für Thermostate in Home Assistant](#-eco-modus-für-thermostate-in-home-assistant)
	- [📦 Inhalt](#-inhalt)
	- [🌟 Funktionen](#-funktionen)
	- [📋 Voraussetzungen](#-voraussetzungen)
	- [🛠️ Installation](#️-installation)
		- [1. Erstellung von Temperatur-Helfern](#1-erstellung-von-temperatur-helfern)
			- [Beispielcode für die manuelle Erstellung:](#beispielcode-für-die-manuelle-erstellung)
		- [2. Erstellung eines Template Switch](#2-erstellung-eines-template-switch)
			- [Beispielcode für die manuelle Erstellung:](#beispielcode-für-die-manuelle-erstellung-1)
			- [Alternative Erstellung über die Benutzeroberfläche:](#alternative-erstellung-über-die-benutzeroberfläche)
	- [💡 Tipps](#-tipps)

---

## 🌟 Funktionen  

- **Automatische Temperaturregelung** basierend auf Anwesenheit und Tageszeit.  
- **Eco-Modus** mit **Template Switch**:  
  - Senkt die Temperatur, wenn niemand zu Hause ist oder nachts.  
  - Stellt die zuvor eingestellte Temperatur beim Deaktivieren des Modus wieder her.  
- **Integration** mit allen gängigen Thermostaten, die von Home Assistant unterstützt werden (Zigbee, Z-Wave, WiFi usw.).  

---

## 📋 Voraussetzungen  

- **Home Assistant** (empfohlen Version 2023.x oder neuer).  
- Integrierte **Thermostate** im System (z. B. Zigbee, Z-Wave oder WiFi-Thermostate).  

---

## 🛠️ Installation  

### 1. Erstellung von Temperatur-Helfern  

Es müssen zwei Helfer vom Typ `input_number` erstellt werden:  
- **Erster Helfer** – für die Einstellung der normalen Thermostattemperatur.  
- **Zweiter Helfer** – für die Einstellung der Temperatur im Eco-Modus.  

#### Beispielcode für die manuelle Erstellung:  

```yaml
input_number:
  - normal_temperatur_kueche:
      name: "Normale Temperatur (Küche)"
      min: 5
      max: 30
      step: 0.5
      unit_of_measurement: "°C"
  - eco_modus_temperatur_kueche:
      name: "Eco-Modus Temperatur (Küche)"
      min: 5
      max: 20
      step: 0.5
      unit_of_measurement: "°C"
```

📌 **Hinweis:** Die Helfer können auch über die Home Assistant Benutzeroberfläche im Menü **"Helfer"** erstellt werden.  

---

### 2. Erstellung eines Template Switch  

Der Template Switch steuert den Wechsel zwischen Normal- und Eco-Modus.  

#### Beispielcode für die manuelle Erstellung:  

```yaml
switch:
  - platform: template
    switches:
      eco_modus_kueche:
        friendly_name: "Eco-Modus (Küche)"
        value_template: "{{ false }}"  # Kann nach Bedarf angepasst werden
        turn_on:
          service: climate.set_temperature
          target:
            entity_id: climate.thermostat_kueche
          data:
            temperature: "{{ states('input_number.eco_modus_temperatur_kueche') | float }}"
        turn_off:
          service: climate.set_temperature
          target:
            entity_id: climate.thermostat_kueche
          data:
            temperature: "{{ states('input_number.normal_temperatur_kueche') | float }}"
```

#### Alternative Erstellung über die Benutzeroberfläche:  

1. Klicken Sie auf die Schaltfläche:  
   [![Add template](/img/button%20ADD%20Temlate.svg)](https://my.home-assistant.io/redirect/config_flow_start?domain=template)  
2. Wählen Sie **Switch**.  
   ![img](/img/Template001.png)  
3. Legen Sie die Aktionen für Ein- und Ausschalten fest:  
   - **Einschalten:** Setzt die Temperatur auf den Eco-Modus.  
   - **Ausschalten:** Stellt die normale Temperatur wieder her.  
   ![img](/img/Template002.png)  
   ![img](/img/Template003.png)  

---

## 💡 Tipps  

- Wenn Ihnen dieses Projekt gefallen hat, finden Sie [**hier**](https://github.com/Bacard1?tab=repositories) weitere interessante Projekte von mir.  
- Bei Fragen oder Problemen können Sie mich gerne kontaktieren! 📩