# 💡 Blink/Flash Device Controller Blueprint

Univerzální blueprint pro automatické blikání/pulzování zařízení v Home Assistant s přesným časovým řízením.

## ✨ Hlavní funkce

### 🎯 Podporovaná zařízení
- **Světla** (light) - s podporou barvy a jasu
- **Vypínače** (switch)
- **Ventilátory** (fan)
- **Sirény** (siren)
- Jakékoli zařízení s on/off stavem

### ⏱️ Flexibilní časování
- **ON interval**: Nastavte jak dlouho je zařízení zapnuté (0.1 - 3600 sekund)
- **OFF interval**: Nastavte jak dlouho je zařízení vypnuté (0.1 - 3600 sekund)
- **Nezávislé nastavení**: ON a OFF intervaly můžou být různé

### 🔄 Tři režimy trvání

#### 1. Neomezené blikání
- Běží dokud trigger zůstává aktivní
- Automaticky se zastaví když trigger změní stav
- Ideální pro alarmy, varování

#### 2. Časově omezené
- Bliká po stanovenou dobu (v sekundách)
- Přesné řízení doby trvání
- Ideální pro notifikace, upozornění

#### 3. Počet opakování
- Přesný počet bliknutí (1-1000x)
- Ideální pro doorbell, zvonky, signály

### 🎨 Pokročilé funkce pro světla
- **Nastavitelná barva** (RGB)
- **Nastavitelný jas** (1-255)
- **Zachování původního stavu** - obnoví barvu a jas po skončení

### 🔚 Konečný stav
Po skončení blikání můžete zvolit:
- **Vypnout** - zařízení zůstane vypnuté
- **Zapnout** - zařízení zůstane zapnuté
- **Obnovit** - vrátí původní stav (barva, jas, on/off)

### ⚡ Trigger a podmínky
- **Volitelný trigger**: input_boolean, binary_sensor, alarm_control_panel, atd.
- **Volitelná podmínka**: Další entity která musí být v určitém stavu
- **Automatické zastavení**: Když trigger nebo podmínka přestane platit

## 📦 Instalace

### Import do Home Assistant

1. V Home Assistant přejděte do **Settings** → **Automations & Scenes** → **Blueprints**
2. Klikněte na **Import Blueprint**
3. Vložte URL:
   ```
   https://raw.githubusercontent.com/joshuaaaaa/HA-Hori/claude/fire-detection-blueprint-cx6Ql/blueprints/automation/blink_flash/blink_flash.yaml
   ```
4. Klikněte **Preview** a **Import**

## 🎓 Příklady použití

### Příklad 1: Základní blikání světla

```yaml
alias: Blink Light - Basic
use_blueprint:
  path: blink_flash/blink_flash.yaml
  input:
    target_entities:
      entity_id: light.living_room
    trigger_entity: input_boolean.blink_mode
    trigger_state: "on"
    on_interval: 0.5
    off_interval: 0.5
    duration_mode: unlimited
    end_state: "off"
    light_brightness: 255
    light_color: [255, 0, 0]
```

**Co to dělá:**
- Světlo bliká červeně když je `input_boolean.blink_mode` zapnutý
- 0.5s zapnuto, 0.5s vypnuto
- Běží dokud není input_boolean vypnut

### Příklad 2: Alarm - rychlé blikání

```yaml
alias: Alarm Flash
use_blueprint:
  path: blink_flash/blink_flash.yaml
  input:
    target_entities:
      entity_id:
        - light.living_room
        - light.bedroom
        - light.kitchen
    trigger_entity: alarm_control_panel.home_alarm
    trigger_state: "triggered"
    on_interval: 0.2
    off_interval: 0.2
    duration_mode: limited
    duration_time: 120  # 2 minuty
    end_state: restore
    light_brightness: 255
    light_color: [255, 0, 0]
```

**Co to dělá:**
- Všechna světla rychle blikají červeně při alarmu
- Velmi rychlé blikání (0.2s / 0.2s)
- Trvá 2 minuty
- Obnoví původní stav světel po skončení

### Příklad 3: Notifikace - pomalé pulzování

```yaml
alias: Notification Pulse
use_blueprint:
  path: blink_flash/blink_flash.yaml
  input:
    target_entities:
      entity_id: light.desk_lamp
    trigger_entity: sensor.unread_notifications
    trigger_state: "1"
    on_interval: 2.0
    off_interval: 2.0
    duration_mode: limited
    duration_time: 300  # 5 minut
    end_state: "off"
    light_brightness: 128
    light_color: [0, 100, 255]
```

**Co to dělá:**
- Lampička pomalu pulzuje modře když máte neprečtenou notifikaci
- Pomalé pulzování (2s / 2s)
- Trvá 5 minut
- Vypne se po skončení

### Příklad 4: Zvonek - 3x bliknutí

```yaml
alias: Doorbell Blink
use_blueprint:
  path: blink_flash/blink_flash.yaml
  input:
    target_entities:
      entity_id:
        - light.entrance
        - light.living_room
    trigger_entity: binary_sensor.doorbell
    trigger_state: "on"
    on_interval: 1.0
    off_interval: 0.5
    duration_mode: count
    repeat_count: 3
    end_state: restore
    light_brightness: 255
    light_color: [255, 200, 0]
```

**Co to dělá:**
- Světla bliknou 3x žlutě když někdo zvoní
- 1s zapnuto, 0.5s vypnuto
- Přesně 3 opakování
- Obnoví původní stav

## ⚙️ Konfigurace

### Základní nastavení

| Parametr | Popis | Výchozí | Rozsah |
|----------|-------|---------|--------|
| **target_entities** | Zařízení k blikání | - | light, switch, fan, siren |
| **on_interval** | Doba zapnutí | 0.5s | 0.1 - 3600s |
| **off_interval** | Doba vypnutí | 0.5s | 0.1 - 3600s |

### Trigger

| Parametr | Popis | Výchozí |
|----------|-------|---------|
| **trigger_entity** | Entity která spouští blikání | (volitelné) |
| **trigger_state** | Stav který spouští | "on" |

### Režim trvání

| Režim | Popis | Parametr |
|-------|-------|----------|
| **unlimited** | Neomezené (dokud trigger) | - |
| **limited** | Časově omezené | duration_time (sekundy) |
| **count** | Počet opakování | repeat_count (1-1000) |

### Světla (volitelné)

| Parametr | Popis | Výchozí |
|----------|-------|---------|
| **light_brightness** | Jas (1-255, 0=nechat) | 0 |
| **light_color** | RGB barva | [255, 255, 255] |

### Konečný stav

| Hodnota | Popis |
|---------|-------|
| **off** | Vypnout po skončení |
| **on** | Zapnout po skončení |
| **restore** | Obnovit původní stav |

### Podmínky (volitelné)

| Parametr | Popis |
|----------|-------|
| **condition_entity** | Entity pro podmínku |
| **condition_state** | Požadovaný stav |

## 💡 Tipy a triky

### 1. Rychlé SOS blikání
```yaml
on_interval: 0.2
off_interval: 0.2
duration_mode: count
repeat_count: 9
light_color: [255, 0, 0]
```

### 2. Jemné pulzování pro ambient
```yaml
on_interval: 3.0
off_interval: 3.0
light_brightness: 50
light_color: [255, 100, 150]
```

### 3. Varování - 2x rychle, pauza
Vytvořte 2 automatizace:
- První: 2x rychlé bliknutí
- Druhá: Spuštění první automatizace každých 10s

### 4. Blikání jen v noci
Přidejte condition:
```yaml
condition_entity: sun.sun
condition_state: "below_horizon"
```

## 🔧 Troubleshooting

### Problém: Blikání se nespustí

**Řešení:**
1. Zkontrolujte že trigger_entity existuje a má správný stav
2. Ověřte že target_entities jsou dostupné
3. Pokud máte condition_entity, zkontrolujte jeho stav
4. Podívejte se do Logbooku na chybové hlášky

### Problém: Blikání se nezastaví

**Řešení:**
1. V režimu "unlimited" zkontrolujte že trigger_entity změní stav
2. Zkontrolujte že automatizace je v mode "restart" (výchozí)
3. Můžete manuálně zastavit automatizaci v UI

### Problém: Barva nebo jas světla nefunguje

**Řešení:**
1. Zkontrolujte že light_brightness > 0 (jinak se nepoužije)
2. Ověřte že vaše světla podporují RGB barvy
3. Některá světla mají omezený rozsah barev

### Problém: Obnovení stavu nefunguje

**Řešení:**
1. Ujistěte se že end_state je nastaveno na "restore"
2. Scene se vytváří na začátku - pokud změníte stav během blikání, původní stav se nepřepíše
3. Zkontrolujte že máte volnou scene slot

## 📋 Use Cases (Případy použití)

### 🚨 Bezpečnost
- Alarm: Rychlé červené blikání při vniknutí
- Kouř/požár: Pulzující oranžová při detekci kouře
- Varování: Blikání při otevření dveří v noci

### 🔔 Notifikace
- Doorbell: 3x žluté bliknutí při zvonění
- Telefon: Pulzující modrá při zmeškaném hovoru
- Zpráva: Krátké bliknutí při nové zprávě

### 🎉 Zábava
- Party mode: Barevné blikání světel
- Film: Pulzující červená během filmové scény
- Gaming: Synchronizace s herními událostmi

### 🏠 Upozornění
- Pračka hotová: 5x zelené bliknutí
- Timer vypršel: Pulzování dokud není vypnut
- Baterie vybitá: Červené pulzování

## 🔗 Související blueprinty

- [Fire Detection Blueprint](../fire_detection/) - Požární detekce s blikajícími světly

## 📜 Licence

MIT License - volně použitelné

---

**Vytvořte si vlastní blikací vzory a automatizace! 💡✨**
