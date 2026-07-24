# 🏠 Home Assistant Blueprints Collection

[🇬🇧 English](./README.md) | 🇨🇿 Čeština

Sbírka užitečných a spolehlivých blueprintů pro Home Assistant. Každý blueprint je pečlivě navržen, otestován a dokumentován.

## 📦 Dostupné Blueprinty

### 🔥 [Fire Detection Blueprint](./automation/fire_detection/)

Komplexní systém pro detekci požáru pomocí teplotních senzorů s automatickým vypínáním spotřebičů a pořizováním fotografií z kamer.

**Hlavní funkce:**
- 🌡️ Multi-sensor teplotní monitoring s konfigurovatelným prahem
- 🔌 Automatické vypínání nebezpečných spotřebičů při alarmu
- 📸 Camera snapshots s automatickým odesláním do Telegramu
- 📁 Separátní složka pro fotky (`/config/www/fire_snapshots/`)
- ⚡ Okamžité Telegram notifikace (text posílán hned, fotky následují)
- 📲 Konfigurovatelný způsob odesílání Telegramu — volba mezi starším servisem `notify.*` a novějším `telegram_bot.send_message` / `send_photo`, s volitelnou notify entitou a message thread ID
- 🚨 Vizuální alarmy (blikající světla, 3 režimy)
- 📱 Multiple notification channels (Telegram, mobile, HA)
- 📢 TTS hlášení přes reproduktory
- 🔔 Sirény a zvukové alarmy
- ⏱️ Konfigurovatelný delay mezi snapshoty (0-60 minut)
- 🔄 Restart mode pro okamžitou reakci na nové triggery
- 📝 Pre-computed zprávy pro lepší výkon

**Instalace:**
```
https://raw.githubusercontent.com/joshuaaaaa/HA--Fire-Detection/main/automation/fire_detection/fire_detection_v7.yaml
```

**⚠️ Důležité:** Před použitím vytvořte složku `/config/www/fire_snapshots/` pro ukládání fotografií z kamer.

[💡 Příklady](./examples/)

---

### 🌡️ [Temperature Threshold Alert Blueprint](./automation/temperature_alert/)

Univerzální teplotní monitoring s alarmy pro VYSOKÉ (požár, přehřátí) i NÍZKÉ (mrznutí, podchlazení) teploty.

**Hlavní funkce:**
- 🔄 Bidirekcční monitoring: Above (nad prahem) i Below (pod prahem)
- ❄️ Ochrana před mrznutím: Automatické zapnutí topení, cirkulace
- 🔥 Ochrana před přehřátím: Automatické zapnutí ventilace, chlazení
- ⚙️ Automatické akce: Zapnout/vypnout zařízení při alarmu
- 📱 Flexibilní notifikace: Telegram, mobile, HA, TTS (různé zprávy pro above/below)
- 🚰 Prevence zamrznutí potrubí: Speciální režim s kritickým prahem
- 🖥️ Ochrana elektroniky: Server rooms, network cabinets
- ✅ Double-check logika: CHECK → DELAY → RECHECK (robustnější detekce)
- 🔄 Restart mode pro okamžitou reakci
- 📝 Pre-computed zprávy pro lepší výkon
- 📊 Logbook entries pro audit trail

**Typické použití:**
- ❄️ **Mrznutí** (práh 2-5°C): Sklepy, garáže, vodovodní potrubí
- 🔥 **Přehřátí** (práh 30-40°C): Server rooms, síťové rozvaděče, podkroví

**Instalace:**
```
https://raw.githubusercontent.com/joshuaaaaa/HA--Fire-Detection/main/automation/temperature_alert/temperature_alert.yaml
```

[💡 Příklady](./examples/temperature_alert/)

---

### 💡 [Blink/Flash Device Controller](./automation/blink_flash/)

Univerzální blueprint pro automatické blikání/pulzování zařízení s přesným časovým řízením.

**Hlavní funkce:**
- 💡 Podpora světel, vypínačů, ventilátorů, sirén
- ⏱️ Nezávislé ON/OFF intervaly (0.1 - 3600 sekund)
- 🔄 3 režimy: Neomezené, Časově omezené, Počet opakování
- 🎨 Nastavitelná barva a jas pro světla
- 🔚 Konečný stav: Vypnout, Zapnout, nebo Obnovit původní
- 🎯 Volitelný trigger a podmínky
- 🚀 Restart mode pro okamžitou reakci

**Použití:**
- 🚨 Alarmy a varování (rychlé červené blikání)
- 🔔 Notifikace (pulzující světla)
- 🎉 Efekty a zábava (barevné vzory)
- 🏠 Upozornění (doorbell, timer)

**Instalace:**
```
https://raw.githubusercontent.com/joshuaaaaa/HA--Fire-Detection/main/automation/blink_flash/blink_flash.yaml
```

[💡 Příklady](./examples/blink_flash/)

---

## 🚀 Rychlý Start

### 1. Příprava (pouze pro Fire Detection s kamerami)

Pokud plánujete používat camera snapshots, vytvořte složku pro fotky:

**Přes File Editor:**
1. Otevřete **File Editor** v Home Assistant
2. Přejděte do složky `/config/www/`
3. Vytvořte novou složku s názvem `fire_snapshots`

**Nebo přes SSH/Terminal:**
```bash
mkdir /config/www/fire_snapshots
```

### 2. Import Blueprintu

1. Otevřete Home Assistant
2. Přejděte do **Settings** → **Automations & Scenes** → **Blueprints**
3. Klikněte na **Import Blueprint** (pravý dolní roh)
4. Vložte URL blueprintu (viz výše)
5. Klikněte **Preview** a **Import**

### 3. Vytvoření Automatizace

1. Přejděte do **Automations & Scenes** → **Automations**
2. Klikněte **Create Automation** → **Create from Blueprint**
3. Vyberte importovaný blueprint
4. Nakonfigurujte podle svých potřeb
5. Uložte a otestujte

### 4. Použijte Příklady

Všechny blueprinty mají příklady v složce [`examples/`](./examples/). Zkopírujte, upravte a použijte!

## 📚 Dokumentace

Každý blueprint obsahuje:

- **Komentáře přímo v kódu** - Detailní popis každé sekce
- **Příklady použití** - Ready-to-use konfigurace v `examples/`
- **FAQ v tomto README** - Odpovědi na časté otázky

## 💡 Příklady Použití

### Fire Detection Blueprint

```yaml
# Základní požární ochrana s kamerami
alias: Fire Detection - Basic Home
use_blueprint:
  path: joshuaaaaa/HA--Fire-Detection/automation/fire_detection/fire_detection_v7.yaml
  input:
    threshold: 60
    include:
      entity_id:
        - sensor.kitchen_temperature
        - sensor.living_room_temperature
    minimum_duration: 30
    enable_telegram: true
    telegram_service: notify.telegram
    enable_camera_snapshot: true
    camera_entities:
      entity_id: camera.kitchen
    camera_snapshot_delay: 10
    enable_light_alarm: true
    light_entities:
      entity_id: light.hallway
    light_mode: blink
```

### Temperature Alert Blueprint

```yaml
# Automatická prevence zamrznutí
alias: Freeze Prevention
use_blueprint:
  path: joshuaaaaa/HA--Fire-Detection/automation/temperature_alert/temperature_alert.yaml
  input:
    threshold_mode: below
    threshold: 3
    include:
      - sensor.basement_temperature
      - sensor.garage_temperature
    minimum_duration: 120
    cooldown_minutes: 30
    enable_auto_actions: true
    entities_to_turn_on:
      entity_id:
        - switch.basement_heater
        - switch.pipe_heating_cable
    enable_telegram: true
```

### Blink/Flash Blueprint

```yaml
# Blikající světlo při alarmu
alias: Alarm Flash
use_blueprint:
  path: joshuaaaaa/HA--Fire-Detection/automation/blink_flash/blink_flash.yaml
  input:
    target_entities:
      entity_id: light.living_room
    trigger_entity: alarm_control_panel.home
    trigger_state: "triggered"
    on_interval: 0.2
    off_interval: 0.2
    duration_mode: unlimited
    light_brightness: 255
    light_color: [255, 0, 0]
```

## 🔧 Struktura Repozitáře

```
HA--Fire-Detection/
├── README.md                                    # English version
├── README.cs.md                                 # Tento soubor (česká verze)
├── LICENSE                                      # MIT licence
├── automation/                                  # Home Assistant Blueprinty
│   ├── fire_detection/
│   │   └── fire_detection_v7.yaml              # Fire Detection Blueprint
│   ├── temperature_alert/
│   │   └── temperature_alert.yaml              # Temperature Alert Blueprint
│   └── blink_flash/
│       └── blink_flash.yaml                    # Blink/Flash Blueprint
└── examples/                                    # Příklady konfigurace
    ├── basic_home_protection.yaml              # Fire: Základní ochrana
    ├── full_protection_with_cameras.yaml       # Fire: Plná ochrana
    ├── night_mode_quiet.yaml                   # Fire: Noční režim
    ├── garage_workshop.yaml                    # Fire: Garáž/dílna
    ├── temperature_alert/
    │   ├── freeze_alert_basic.yaml             # Temp: Mrznutí základní
    │   ├── freeze_prevention_auto.yaml         # Temp: Auto topení
    │   ├── overheat_alert.yaml                 # Temp: Přehřátí
    │   └── pipe_freeze_prevention.yaml         # Temp: Potrubí prevence
    └── blink_flash/
        ├── basic_blink.yaml                    # Blink: Základní
        ├── alarm_flash.yaml                    # Blink: Alarm
        ├── slow_pulse.yaml                     # Blink: Pomalé pulzování
        └── doorbell_count.yaml                 # Blink: Zvonek
```

## 🤝 Přispívání

Návrhy, bugreporty a pull requesty jsou vítány!

### Jak přispět:

1. Forkněte repozitář
2. Vytvořte feature branch (`git checkout -b feature/AmazingFeature`)
3. Commitněte změny (`git commit -m 'Add some AmazingFeature'`)
4. Pushněte do branche (`git push origin feature/AmazingFeature`)
5. Otevřete Pull Request

## 🐛 Bug Reports

Pokud najdete problém:

1. Zkontrolujte [Issues](https://github.com/joshuaaaaa/HA--Fire-Detection/issues) jestli už není nahlášen
2. Vytvořte nový Issue s detaily:
   - Blueprint který používáte
   - Verze Home Assistant
   - Popis problému
   - Kroky k reprodukci
   - Logy (pokud jsou relevantní)

## ❓ FAQ

### Q: Fungují blueprinty bez internetu?
**A:** Ano, většina funkcí funguje lokálně. Výjimka: Telegram notifikace vyžadují internet.

### Q: Mohu použít více blueprintů současně?
**A:** Ano! Blueprinty jsou nezávislé a můžete jich mít libovolný počet.

### Q: Jak aktualizuji blueprint?
**A:** Prostě znovu importujte URL. HA automaticky aktualizuje existující automatizace.

### Q: Jsou blueprinty kompatibilní se všemi verzemi HA?
**A:** Testováno na HA 2024.1+. Starší verze mohou mít problémy s některými funkcemi.

### Q: Mohu modifikovat blueprinty?
**A:** Ano! MIT licence umožňuje libovolné úpravy. Můžete si je přizpůsobit podle potřeby.

### Q: Proč nepřichází fotky z kamer přes Telegram?
**A:** Zkontrolujte:
1. Je vytvořená složka `/config/www/fire_snapshots/`
2. Je zapnutý toggle "Enable Camera Snapshots"
3. Je zapnutý toggle "Enable Telegram Notifications"
4. Je vybraná alespoň jedna kamera
5. Telegram service je správně nakonfigurovaný (např. `notify.telegram`)

### Q: Jaké jsou nejnovější vylepšení blueprintů?
**A:** Všechny blueprinty byly nedávno refaktorovány:

**Fire Detection v7:**
- Telegram textová zpráva se posílá OKAMŽITĚ (nemusíte čekat na fotky)
- Fotky se ukládají do separátní složky `/config/www/fire_snapshots/`
- Přidán 2s delay mezi snapshot a odesláním (spolehlivější)
- Pre-computed zprávy pro lepší výkon
- Restart mode pro okamžitou reakci

**Temperature Alert (nový):**
- Double-check logika: CHECK → DELAY → RECHECK (robustnější)
- Pre-computed zprávy (telegram, HA, TTS)
- Restart mode místo single pro lepší responsivitu
- Cooldown check v variables pro lepší výkon
- Logbook entries pro audit trail
- Žádná top-level condition - vše řešeno v action (flexibilnější)

## 📊 Statistiky

- **Počet blueprintů**: 3
- **Celkem příkladů**: 12
- **Podporované domény**: 15+ (light, switch, camera, sensor, climate, water_heater, fan, atd.)
- **Jazyk**: Čeština + Angličtina

## 🔗 Užitečné odkazy

- [Home Assistant](https://www.home-assistant.io/)
- [Home Assistant Community](https://community.home-assistant.io/)
- [Blueprint Documentation](https://www.home-assistant.io/docs/automation/using_blueprints/)
- [YAML Guide](https://www.home-assistant.io/docs/configuration/yaml/)

## 📜 Licence

MIT License - volně použitelné pro osobní i komerční účely.

Viz [LICENSE](./LICENSE) pro detaily.

## ⚠️ Důležité upozornění

**Fire Detection Blueprint** je určen jako doplňková ochrana. Nenahrazuje profesionální požární hlásič, detektory kouře a hasicí přístroje. Vždy dodržujte místní požární předpisy a stavební normy.

---

**Vytvořeno s ❤️ pro Home Assistant komunitu**

🔥 Buďte v bezpečí! 💡 Buďte kreativní!
