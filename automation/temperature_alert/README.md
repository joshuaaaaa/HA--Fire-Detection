# 🌡️ Temperature Threshold Alert Blueprint

Univerzální blueprint pro monitoring teplot s alarmy pro VYSOKÉ (požár, přehřátí) i NÍZKÉ (mrznutí, podchlazení) teploty.

## ✨ Hlavní funkce

### 🔄 Bidirekcční monitoring
- **Above (Nad prahem)**: Alarm při vysokých teplotách
  - 🔥 Požár, přehřátí
  - 🖥️ Ochrana elektroniky
  - 🏠 Letní přehřátí
- **Below (Pod prahem)**: Alarm při nízkých teplotách
  - ❄️ Mrznutí, zamrzání
  - 🚰 Ochrana vodovodního potrubí
  - 🌡️ Zimní podchlazení

### 🎯 Flexibilní konfigurace
- **Nastavitelný práh**: -40°C až +150°C
- **Multi-sensor**: Sledování libovolného počtu senzorů
- **Minimum duration**: Prevence falešných alarmů
- **Cooldown**: Ochrana před spam notifikacemi

### ⚙️ Automatické akce
- **Turn ON entities**: Automaticky zapnout zařízení (topení, ventilaci, atd.)
- **Turn OFF entities**: Automaticky vypnout zařízení (ventilaci, kohoutky, atd.)
- **Custom actions**: Libovolné vlastní akce

### 📱 Notifikace
- **Telegram**: Detailní zprávy s info o senzorech a teplotách
- **Mobile App**: Push notifikace s vysokou prioritou
- **HA Persistent**: Trvalé notifikace v Home Assistant UI
- **TTS (Media Player)**: Hlasové oznámení přes reproduktory/smart speakers
- **Vlastní zprávy**: Pro každý režim (above/below) zvlášť

## 📦 Instalace

### Import do Home Assistant

1. V Home Assistant přejděte do **Settings** → **Automations & Scenes** → **Blueprints**
2. Klikněte na **Import Blueprint**
3. Vložte URL:
   ```
   https://raw.githubusercontent.com/joshuaaaaa/HA-Hori/claude/fire-detection-blueprint-cx6Ql/blueprints/automation/temperature_alert/temperature_alert.yaml
   ```
4. Klikněte **Preview** a **Import**

## 💡 Příklady použití

### Příklad 1: Základní upozornění na mrznutí

```yaml
alias: Freeze Alert - Basic
use_blueprint:
  path: temperature_alert/temperature_alert.yaml
  input:
    threshold_mode: below
    threshold: 5
    include:
      - sensor.outdoor_temperature
      - sensor.garage_temperature
      - sensor.basement_temperature
    minimum_duration: 300
    cooldown_minutes: 30
    enable_telegram: true
    telegram_message_below: |
      ❄️ VAROVÁNÍ: NÍZKÁ TEPLOTA! ❄️
      ⚠️ Riziko zamrznutí!
```

**Co to dělá:**
- Sleduje teploty venku, v garáži a sklepě
- Alarm když teplota klesne pod 5°C po dobu 5 minut
- Telegram notifikace každých 30 minut

### Příklad 2: Automatická prevence zamrznutí s topením

```yaml
alias: Freeze Prevention - Auto Heating
use_blueprint:
  path: temperature_alert/temperature_alert.yaml
  input:
    threshold_mode: below
    threshold: 3
    include:
      - sensor.basement_temperature
      - sensor.garage_temperature
    minimum_duration: 120

    # Automatické akce
    enable_auto_actions: true
    entities_to_turn_on:
      entity_id:
        - switch.basement_heater
        - switch.garage_heater
        - switch.pipe_heating_cable
    entities_to_turn_off:
      entity_id:
        - fan.basement_ventilation

    # TTS oznámení
    enable_media_player: true
    tts_service: tts.google_translate_say
    tts_alarm_message_below: "Pozor! Nízká teplota! Topení automaticky zapnuto!"
    tts_language: cs
    media_player_entities:
      entity_id:
        - media_player.living_room_speaker

    enable_telegram: true
    telegram_message_below: |
      ❄️ KRITICKÁ TEPLOTA! ❄️
      ✅ Topení automaticky zapnuto
```

**Co to dělá:**
- Při teplotě pod 3°C automaticky zapne topení
- Vypne ventilaci (aby neunikalo teplo)
- TTS hlasové oznámení přes reproduktor
- Telegram notifikace o akci

### Příklad 3: Upozornění na přehřátí serveru

```yaml
alias: Server Room Overheat
use_blueprint:
  path: temperature_alert/temperature_alert.yaml
  input:
    threshold_mode: above
    threshold: 35
    include:
      - sensor.server_room_temperature
      - sensor.network_cabinet_temperature
    minimum_duration: 180

    # Automatické akce
    enable_auto_actions: true
    entities_to_turn_on:
      entity_id:
        - fan.server_room_fan
        - climate.server_room_ac
    entities_to_turn_off:
      entity_id:
        - light.server_room_lights

    enable_telegram: true
    telegram_message_above: |
      🔥 PŘEHŘÁTÍ SERVERU! 🔥
      ✅ Chlazení aktivováno
```

**Co to dělá:**
- Při teplotě nad 35°C zapne ventilátory a klimatizaci
- Vypne světla (méně tepla)
- Telegram notifikace

### Příklad 4: Prevence zamrznutí potrubí (pokročilé)

```yaml
alias: Pipe Freeze Prevention
use_blueprint:
  path: temperature_alert/temperature_alert.yaml
  input:
    threshold_mode: below
    threshold: 2
    include:
      - sensor.basement_pipe_temperature
      - sensor.garage_water_line_temperature
      - sensor.outdoor_pipe_sensor
    minimum_duration: 60

    enable_auto_actions: true
    entities_to_turn_on:
      entity_id:
        - switch.pipe_heating_cable_basement
        - switch.pipe_heating_cable_garage
        - switch.water_circulation_pump
    entities_to_turn_off:
      entity_id:
        - switch.outdoor_water_valve

    enable_telegram: true
    enable_mobile: true
    telegram_message_below: |
      🚨 KRITICKÉ! RIZIKO ZAMRZNUTÍ POTRUBÍ! 🚨
      ✅ Topné kabely zapnuty
      ✅ Cirkulace aktivována
      ✅ Venkovní ventily uzavřeny

      Okamžitě jednejte!

    actions:
      # Zapnout topení na maximum
      - service: climate.set_temperature
        target:
          entity_id: climate.basement_thermostat
        data:
          temperature: 20
```

**Co to dělá:**
- Kritický práh 2°C pro potrubí
- Okamžitá reakce (60s)
- Zapne topné kabely a cirkulaci
- Uzavře venkovní ventily
- Dodatečně nastaví termostat na 20°C

## ⚙️ Konfigurace

### Základní nastavení

| Parametr | Popis | Výchozí | Rozsah |
|----------|-------|---------|--------|
| **threshold_mode** | Režim: above/below | below | above, below |
| **threshold** | Teplotní práh | 5°C | -40 až 150°C |
| **include** | Sledované senzory | - | temperature sensors |
| **minimum_duration** | Min. doba trvání | 30s | 0 - 3600s |
| **cooldown_minutes** | Cooldown notifikací | 15min | 1 - 180min |

### Automatické akce

| Parametr | Popis | Použití |
|----------|-------|---------|
| **enable_auto_actions** | Zapnout auto akce | false |
| **entities_to_turn_on** | Zapnout při alarmu | Topení (freeze), Ventilace (overheat) |
| **entities_to_turn_off** | Vypnout při alarmu | Ventilace (freeze), Spotřebiče (overheat) |

### Notifikace

Každý typ notifikace má **2 sady zpráv**:
- **above**: Zprávy pro vysoké teploty
- **below**: Zprávy pro nízké teploty

| Typ | Parametry |
|-----|-----------|
| **Telegram** | telegram_message_above, telegram_message_below |
| **Mobile** | mobile_title_above/below, mobile_message_above/below |
| **HA** | ha_notification_title_above/below |

## 📊 Případy použití

### ❄️ Nízké teploty (below)

| Scénář | Práh | Akce |
|--------|------|------|
| **Mrznutí potrubí** | 2°C | Topné kabely ON, cirkulace ON |
| **Sklep v zimě** | 5°C | Topení ON, notifikace |
| **Garáž** | 3°C | Topení ON, ventilace OFF |
| **Venkovní senzor** | -5°C | Jen upozornění |
| **Greenhouse** | 10°C | Topení ON |

### 🔥 Vysoké teploty (above)

| Scénář | Práh | Akce |
|--------|------|------|
| **Server room** | 35°C | Ventilace ON, světla OFF |
| **Síťový rozvaděč** | 40°C | Chlazení ON, alarm |
| **Podkroví v létě** | 45°C | Ventilace ON, klimatizace ON |
| **Elektrický rozvaděč** | 50°C | Ventilace ON, vypnout zátěž |
| **Garáž elektronika** | 35°C | Ventilace ON |

## 🔧 Troubleshooting

### Problém: Falešné alarmy

**Řešení:**
1. Prodlužte `minimum_duration` (např. z 30s na 300s)
2. Upravte práh (zvyšte pro above, snižte pro below)
3. Zkontrolujte kalibraci senzorů
4. Odeberte problematické senzory

### Problém: Automatické akce nefungují

**Řešení:**
1. Zkontrolujte že `enable_auto_actions: true`
2. Ověřte že entity existují a jsou dostupné
3. Otestujte manuální zapnutí/vypnutí entit
4. Zkontrolujte logbook pro chybové zprávy

### Problém: Příliš mnoho notifikací

**Řešení:**
1. Zvyšte `cooldown_minutes` (např. z 15 na 30-60 minut)
2. Zkontrolujte že teplota opravdu kolísá kolem prahu
3. Upravte práh aby byl dál od běžných hodnot

### Problém: Topení/ventilace se nezapíná

**Řešení:**
1. Zkontrolujte že entity jsou ve správné sekci (ON vs OFF)
2. Ověřte že mají správná oprávnění
3. Testujte přes Developer Tools → Services
4. Zkontrolujte logbook

## 💡 Tipy a triky

### 1. Kombinace obou režimů
Vytvořte 2 automatizace ze stejného blueprintu:
- První: `threshold_mode: below` s prahem 5°C (mrznutí)
- Druhá: `threshold_mode: above` s prahem 30°C (přehřátí)

### 2. Různé prahy pro různé místnosti
Vytvořte více automatizací:
- Sklep: práh 3°C
- Garáž: práh 5°C
- Podkroví: práh 45°C

### 3. Noční vs denní režim
Použijte `alarm_control` input_boolean:
- Automatizace pro den: práh 5°C
- Automatizace pro noc: práh 2°C (citlivější)

### 4. Postupné akce podle teploty
Vytvořte více automatizací s různými prahy:
- 5°C: Jen notifikace
- 3°C: Zapnout topení
- 1°C: Kritický alarm + maximální topení

### 5. Integrace s počasím
Použijte `alarm_control` který se aktivuje podle předpovědi:
```yaml
automation:
  - alias: Enable Freeze Alert on Cold Forecast
    trigger:
      - platform: numeric_state
        entity_id: sensor.weather_forecast_temp
        below: 0
    action:
      - service: input_boolean.turn_on
        target:
          entity_id: input_boolean.freeze_alert_enabled
```

## 📋 Doporučené hodnoty prahů

### Mrznutí (below)

| Aplikace | Doporučený práh | Poznámka |
|----------|----------------|----------|
| Vodovodní potrubí | 2°C | Kritické! |
| Sklep | 5°C | Prevence vlhkosti |
| Garáž | 3°C | Ochrana nářadí |
| Greenhouse | 10°C | Ochrana rostlin |
| Venkovní senzor | -5°C | Informativní |

### Přehřátí (above)

| Aplikace | Doporučený práh | Poznámka |
|----------|----------------|----------|
| Server room | 25-30°C | Ideální pro elektroniku |
| Síťový rozvaděč | 35-40°C | Kritické |
| Obytné místnosti | 28-30°C | Komfort |
| Podkroví | 40-45°C | Strukturální ochrana |
| Elektrický rozvaděč | 45-50°C | Požární riziko |

## 🔗 Související blueprinty

- [Fire Detection Blueprint](../fire_detection/) - Specializovaný požární alarm
- [Blink/Flash Controller](../blink_flash/) - Vizuální alarmy

## 📜 Licence

MIT License - volně použitelné

## ⚠️ Důležité upozornění

Tento blueprint je určen jako **doplňková ochrana**. Pro kritické aplikace (vodovodní potrubí, server rooms) vždy použijte profesionální monitorovací systémy a mějte záložní plány.

---

**🌡️ Chraňte svůj domov před extrémními teplotami! ❄️🔥**
