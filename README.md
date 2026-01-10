# 🔥 Fire Detection Blueprint

Komplexní blueprint pro Home Assistant pro detekci požáru pomocí teplotních senzorů s automatickým vypínáním spotřebičů a pořizováním fotografií z kamer.

## 📋 Obsah

- [Hlavní funkce](#-hlavní-funkce)
- [Instalace](#-instalace)
- [Konfigurace](#-konfigurace)
- [Příklady použití](#-příklady-použití)
- [Troubleshooting](#-troubleshooting)
- [FAQ](#-faq)
- [Changelog](#-changelog)

## ✨ Hlavní funkce

### 🌡️ Pokročilá detekce požáru
- **Time-based polling**: Kontrola každých 10 sekund pro spolehlivou detekci
- **Konfigurovatelný práh teploty**: Nastavte limit v °C pro spuštění alarmu
- **Minimální doba trvání**: Prevence falešných poplachů - teplota musí být nad prahem po stanovenou dobu
- **Multi-sensor monitoring**: Sledování libovolného počtu teplotních senzorů současně
- **Manuální ovládání**: Možnost vypnout alarm pomocí input_boolean

### 🔌 Automatické vypínání spotřebičů - **Bezpečnostní shutdown**: Automatické vypnutí nebezpečných spotřebičů při alarmu
- **Podporované entity**:
  - Vypínače (switch)
  - Světla (light)
  - Klimatizace (climate)
  - Ohřívače vody/bojlery (water_heater)
  - Ventilátory (fan)
  - Zvlhčovače (humidifier)
- **Logování**: Každé vypnutí se zaznamená do Home Assistant logbooku
- **Okamžitá reakce**: Spotřebiče se vypnou ihned po potvrzení alarmu

### 📸 Camera Snapshots - **Automatické snímky**: Pořízení fotografií z vybraných kamer při alarmu
- **Telegram integrace**: Okamžité odeslání fotek přes Telegram
- **Multiple cameras**: Podpora libovolného počtu kamer
- **Konfigurovatelný delay**: Nastavitelná prodleva mezi snímky (0.5-10s)
- **Časové razítko**: Každý snapshot obsahuje datum a čas
- **Popisky**: Fotky obsahují název kamery a typ alarmu

### 🚨 Vizuální a zvukové alarmy
- **Světelný alarm**:
  - 3 režimy: Blikání, Pulzování, Konstantní svícení
  - Volitelná barva (RGB)
  - Nastavitelná rychlost blikání
  - Obnovení předchozího stavu po alarmu
- **Siréna/zvukový alarm**: Podpora switch, siren a alarm_control_panel
- **TTS oznámení**: Hlasové varování přes media player/reproduktory

### 📱 Notifikace
- **Telegram**: Detailní zprávy s informacemi o senzorech, teplotách a čase
- **Mobile app**: Push notifikace s vysokou prioritou
- **Persistent notification**: Trvalé notifikace v Home Assistant UI
- **Cooldown**: Ochrana před spamováním - konfigurovatelná prodleva mezi notifikacemi

### 🎯 Další funkce
- **Vlastní akce**: Přidejte libovolné další akce při alarmu
- **Single mode**: Zamezení duplicitních spuštění
- **Smart templates**: Pokročilé šablony pro detekci a zprávy
- **Čeština**: Plná podpora českého jazyka

## 📦 Instalace

### Metoda 1: Import přes URL (Doporučeno)

1. Otevřete Home Assistant
2. Přejděte do **Settings** → **Automations & Scenes** → **Blueprints**
3. Klikněte na **Import Blueprint** (pravý dolní roh)
4. Vložte URL:
   ```
   https://github.com/joshuaaaaa/HA-Hori/blob/claude/fire-detection-blueprint-cx6Ql/blueprints/automation/fire_detection/fire_detection_v6.yaml
   ```
5. Klikněte na **Preview** a poté **Import**

### Metoda 2: Manuální kopírování

1. Stáhněte soubor `fire_detection_v6.yaml`
2. Zkopírujte ho do složky:
   ```
   /config/blueprints/automation/fire_detection/
   ```
3. Restartujte Home Assistant nebo znovu načtěte blueprinty

## ⚙️ Konfigurace

### 1️⃣ Základní nastavení

#### Teplotní práh
```yaml
threshold: 60  # °C - Teplota pro spuštění alarmu
```
- **Doporučeno pro domácnost**: 50-70°C
- **Pro kuchyň/saunu**: 80-100°C
- **Pro sklady/garáže**: 40-60°C

#### Sledované senzory
```yaml
include:
  - sensor.kitchen_temperature
  - sensor.living_room_temperature
  - sensor.bedroom_temperature
```
Vyberte všechny teplotní senzory, které chcete monitorovat.

#### Minimální doba trvání
```yaml
minimum_duration: 30  # sekund
```
Jak dlouho musí teplota zůstat nad prahem před spuštěním alarmu. Prevence falešných poplachů.

#### Cooldown notifikací
```yaml
cooldown_minutes: 5  # minut
```
Minimální doba mezi opakovanými notifikacemi při trvajícím alarmu.

### 2️⃣ Automatické vypínání spotřebičů

#### Zapnutí funkce
```yaml
enable_appliance_shutdown: true
```

#### Výběr spotřebičů
```yaml
appliance_entities:
  entity_id:
    - switch.electric_kettle
    - switch.space_heater
    - climate.air_conditioner
    - water_heater.boiler
    - switch.coffee_maker
    - fan.ceiling_fan
```

**Doporučené spotřebiče k vypnutí:**
- ☕ Kávovar, rychlovarná konvice
- 🔥 Elektrické kamna, topení
- 💧 Bojler, ohřívač vody
- 🌡️ Klimatizace, tepelné čerpadlo
- 🍳 Varná konvice (pokud je chytrá)
- 💨 Ventilátory (prevence šíření kouře)

### 3️⃣ Camera Snapshots

#### Zapnutí funkce
```yaml
enable_camera_snapshot: true
enable_telegram: true  # Nutné pro odesílání fotek
```

#### Výběr kamer
```yaml
camera_entities:
  - camera.kitchen
  - camera.living_room
  - camera.garage
  - camera.hallway
```

#### Nastavení snímkování
```yaml
camera_snapshot_delay: 1  # sekundy mezi snapshoty
```

**Tip**: Kratší delay (0.5s) pro rychlé pořízení všech fotek, delší delay (2-5s) pokud máte pomalé kamery nebo slabší síť.

### 4️⃣ Telegram notifikace

#### Základní konfigurace
```yaml
enable_telegram: true
telegram_service: notify.telegram  # Váš Telegram service
```

#### Vlastní zpráva
```yaml
telegram_message: |
  🔥 POŽÁRNÍ POPLACH! 🔥
  ⚠️ Detekována vysoká teplota!
  🏠 Kontrolujte prosím dům!
```

Zpráva se automaticky doplní o:
- 🌡️ Seznam senzorů nad prahem s teplotami
- 📈 Počet alarmových senzorů
- 🔥 Maximální naměřenou teplotu
- ⏰ Čas spuštění alarmu
- 📸 Fotky z kamer (pokud je funkce zapnutá)

### 5️⃣ Světelný alarm

#### Zapnutí a režim
```yaml
enable_light_alarm: true
light_mode: blink  # blink | pulse | solid
```

**Režimy:**
- `blink` - Blikání (zapnuto/vypnuto)
- `pulse` - Pulzování (plná jas ↔ 20% jas)
- `solid` - Konstantní svícení

#### Barva a jas
```yaml
light_color: [255, 0, 0]  # RGB - červená
light_brightness: 255     # 0-255
blink_speed: 0.5         # sekundy
```

**Doporučené barvy:**
- 🔴 Červená `[255, 0, 0]` - Požár (výchozí)
- 🟠 Oranžová `[255, 165, 0]` - Varování
- 🟡 Žlutá `[255, 255, 0]` - Pozor

#### Obnovení stavu
```yaml
restore_lights: true  # Vrátit původní stav světel po alarmu
```

### 6️⃣ Zvukové alarmy

#### Siréna
```yaml
enable_sirens: true
siren_entities:
  entity_id:
    - switch.alarm_siren
    - siren.outdoor_alarm
```

#### TTS oznámení
```yaml
enable_media_player: true
tts_service: tts.google_translate_say
tts_language: cs
tts_alarm_message: "Pozor! Požární poplach! Detekována vysoká teplota!"

media_player_entities:
  entity_id:
    - media_player.living_room_speaker
    - media_player.bedroom_speaker
```

### 7️⃣ Mobile App notifikace

```yaml
enable_mobile: true
mobile_service: notify.mobile_app_phone
mobile_title: "🔥 POŽÁRNÍ POPLACH!"
mobile_message: "⚠️ Detekována vysoká teplota!"
mobile_channel: alarm  # Android notification channel
```

### 8️⃣ Manuální ovládání

```yaml
alarm_control: input_boolean.fire_alarm_enabled
```

Vytvořte `input_boolean`:
```yaml
input_boolean:
  fire_alarm_enabled:
    name: Požární alarm aktivní
    initial: on
    icon: mdi:fire-alert
```

## 💡 Příklady použití

### Příklad 1: Základní domácí ochrana

```yaml
alias: Fire Detection - Basic Home
description: Základní požární ochrana pro domácnost
use_blueprint:
  path: fire_detection/fire_detection_v6.yaml
  input:
    threshold: 60
    include:
      - sensor.kitchen_temperature
      - sensor.living_room_temperature
    minimum_duration: 30
    enable_telegram: true
    telegram_service: notify.telegram
    enable_light_alarm: true
    light_entities:
      entity_id:
        - light.living_room
        - light.bedroom
    light_mode: blink
```

### Příklad 2: Kompletní ochrana s vypínáním spotřebičů

```yaml
alias: Fire Detection - Full Protection
description: Plná ochrana s auto-vypínáním a kamerami
use_blueprint:
  path: fire_detection/fire_detection_v6.yaml
  input:
    threshold: 60
    include:
      - sensor.kitchen_temperature
      - sensor.living_room_temperature
      - sensor.garage_temperature
    minimum_duration: 20

    # Vypínání spotřebičů
    enable_appliance_shutdown: true
    appliance_entities:
      entity_id:
        - switch.electric_kettle
        - switch.space_heater
        - water_heater.boiler
        - climate.air_conditioner

    # Camera snapshots
    enable_camera_snapshot: true
    camera_entities:
      - camera.kitchen
      - camera.living_room
      - camera.garage
    camera_snapshot_delay: 1

    # Alarmy
    enable_sirens: true
    siren_entities:
      entity_id:
        - switch.alarm_siren

    enable_light_alarm: true
    light_entities:
      entity_id:
        - light.all_lights
    light_mode: blink

    # Notifikace
    enable_telegram: true
    telegram_service: notify.telegram

    enable_mobile: true
    mobile_service: notify.mobile_app_iphone
```

### Příklad 3: Noční režim (tichý)

```yaml
alias: Fire Detection - Night Mode
description: Noční režim bez sirény a TTS
use_blueprint:
  path: fire_detection/fire_detection_v6.yaml
  input:
    threshold: 55
    include:
      - sensor.bedroom_temperature
    minimum_duration: 15

    # Jen světla a notifikace
    enable_light_alarm: true
    light_mode: pulse
    light_brightness: 128  # Poloviční jas v noci

    enable_telegram: true
    enable_mobile: true

    # BEZ sirény a TTS
    enable_sirens: false
    enable_media_player: false
```

## 🔧 Troubleshooting

### Problém: Blueprint nefunguje, alarm se nespouští

**Řešení:**
1. Zkontrolujte že teplotní senzory jsou typu `device_class: temperature`
2. Ověřte že senzory vrací platné hodnoty (ne `unknown` nebo `unavailable`)
3. Zkontrolujte že `alarm_control` input_boolean je `on` (pokud je nastavený)
4. Sledujte logbook při spuštění alarmu

### Problém: Camera snapshots se neukládají

**Řešení:**
1. Ověřte že složka `/config/www/` existuje a má správná oprávnění
2. Zkontrolujte že kamery fungují a jsou dostupné
3. Otestujte ručně: `Developer Tools` → `Services` → `camera.snapshot`
4. Zkontrolujte logy Home Assistanta pro chyby

### Problém: Telegram fotky se neodesílají

**Řešení:**
1. Ověřte že Telegram integrace funguje (otestujte normální notifikaci)
2. Zkontrolujte že `enable_camera_snapshot: true` A `enable_telegram: true`
3. Ověřte že snapshoty se ukládají (viz předchozí problém)
4. Zkontrolujte že Telegram service má správný název (např. `notify.telegram`)

### Problém: Spotřebiče se nevypínají

**Řešení:**
1. Zkontrolujte že `enable_appliance_shutdown: true`
2. Ověřte že vybrané entity existují a jsou dostupné
3. Otestujte ručně vypnutí entity přes UI
4. Zkontrolujte logbook - měla by být zpráva o bezpečnostním vypnutí

### Problém: Falešné alarmy

**Řešení:**
1. Zvyšte `threshold` (práh teploty)
2. Prodlužte `minimum_duration` (minimální dobu trvání)
3. Odeberte problematické senzory (např. blízko sporáku)
4. Zkontrolujte kalibraci teplotních senzorů

### Problém: Notifikace přichází příliš často

**Řešení:**
1. Zvyšte `cooldown_minutes` (např. na 10-15 minut)
2. Zkontrolujte že teploty opravdu klesají pod práh mezi alarmymí

## ❓ FAQ

**Q: Jaký teplotní práh je vhodný?**
A: Závisí na místnosti:
- Obývací pokoj/ložnice: 50-60°C
- Kuchyň: 70-80°C (vyšší kvůli vaření)
- Sauna: 100-120°C
- Garáž/sklad: 45-55°C

**Q: Mohu použít více instancí blueprintu?**
A: Ano! Můžete vytvořit několik automatizací s různými nastaveními (např. jedna pro kuchyň s vyšším prahem, druhá pro ložnice s nižším).

**Q: Jak vypnout alarm manuálně?**
A: Nastavte `alarm_control` input_boolean a přepněte ho na `off`. Také můžete vypnout celou automatizaci v UI.

**Q: Funguje blueprint bez internetu?**
A: Ano, kromě Telegram notifikací vše funguje lokálně. Camera snapshots se ukládají lokálně a lze je odeslat později.

**Q: Kolik kamer mohu použít?**
A: Neomezený počet, ale doporučujeme max 5-10 pro rychlost odesílání. Každá fotka se posílá samostatně do Telegramu.

**Q: Jak dlouho trvá pořízení a odeslání fotek?**
A: Závisí na počtu kamer a `camera_snapshot_delay`. Pro 3 kamery s delay 1s = cca 3 sekundy + čas uploadu do Telegramu (cca 1-2s na fotku).

**Q: Můžu přidat vlastní akce?**
A: Ano! Použijte pole `actions` pro libovolné další akce (např. vypnutí elektřiny, zavolání na mobil, etc.).

**Q: Blueprint podporuje detektory kouře?**
A: Aktuálně ne, ale plánujeme přidat v budoucí verzi jako dodatečný trigger.

## 📝 Changelog

### v6 (2026-01-10)
- ✨ **NOVÉ**: Automatické vypínání spotřebičů při alarmu
- ✨ **NOVÉ**: Camera snapshots s odesláním do Telegramu
- 📝 Podpora pro switch, light, climate, water_heater, fan, humidifier
- 📸 Multi-camera podpora s konfigurovatelným delay
- 📊 Logování vypnutí spotřebičů do logbooku
- 🐛 Vylepšené šablony a normalizace entit

### v5 a starší
- Základní detekce teploty
- Světelný alarm (3 režimy)
- TTS oznámení
- Siréna podpora
- Multiple notification channels
- Cooldown ochrana

## 📜 Licence

MIT License - volně použitelné pro osobní i komerční účely.

## 🤝 Přispívání

Návrhy, bugreporty a pull requesty jsou vítány!

## 🔗 Odkazy

- [Home Assistant Community](https://community.home-assistant.io/)
- [Blueprint Documentation](https://www.home-assistant.io/docs/automation/using_blueprints/)
- [GitHub Issues](https://github.com/joshuaaaaa/HA-Hori/issues)

---

**⚠️ DŮLEŽITÉ UPOZORNĚNÍ**: Tento blueprint je určen jako **doplňková ochrana**. Nenahrazuje profesionální požární hlásič, detektory kouře a hasicí přístroje. Vždy dodržujte místní požární předpisy a stavební normy.

🔥 **Buďte v bezpečí!** 🔥
