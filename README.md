# 🏠 Home Assistant Blueprints Collection

🇬🇧 English | [🇨🇿 Čeština](./README.cs.md)

A collection of reliable and well-documented blueprints for Home Assistant. Each blueprint is carefully designed, tested, and documented for production use.

## 📦 Available Blueprints

### 🔥 [Fire Detection Blueprint](./automation/fire_detection/)

Comprehensive fire detection system using temperature sensors with automatic appliance shutdown and camera snapshot capabilities.

**Key Features:**
- 🌡️ Multi-sensor temperature monitoring with configurable threshold
- 🔌 Automatic dangerous appliance shutdown on alarm
- 📸 Camera snapshots with automatic Telegram delivery
- 📁 Dedicated folder for photos (`/config/www/fire_snapshots/`)
- ⚡ Instant Telegram notifications (text sent immediately, photos follow)
- 🚨 Visual alarms (flashing lights, 3 modes)
- 📱 Multiple notification channels (Telegram, mobile, HA)
- 📢 TTS announcements via speakers
- 🔔 Sirens and audio alarms
- ⏱️ Configurable delay between snapshots (0-60 minutes)
- 🔄 Restart mode for instant reaction to new triggers
- 📝 Pre-computed messages for better performance

**Installation:**
```
https://raw.githubusercontent.com/joshuaaaaa/HA--Fire-Detection/main/automation/fire_detection/fire_detection_v7.yaml
```

**⚠️ Important:** Before use, create folder `/config/www/fire_snapshots/` for camera snapshot storage.

[💡 Examples](./examples/)

---

### 🌡️ [Temperature Threshold Alert Blueprint](./automation/temperature_alert/)

Universal temperature monitoring with alerts for both HIGH (fire, overheating) and LOW (freezing, undercooling) temperatures.

**Key Features:**
- 🔄 Bidirectional monitoring: Above (over threshold) and Below (under threshold)
- ❄️ Freeze protection: Automatic heating, circulation activation
- 🔥 Overheat protection: Automatic ventilation, cooling activation
- ⚙️ Automatic actions: Turn on/off devices on alarm
- 📱 Flexible notifications: Telegram, mobile, HA, TTS (different messages for above/below)
- 🚰 Pipe freeze prevention: Special mode with critical threshold
- 🖥️ Electronics protection: Server rooms, network cabinets
- ✅ Double-check logic: CHECK → DELAY → RECHECK (more robust detection)
- 🔄 Restart mode for instant reaction
- 📝 Pre-computed messages for better performance
- 📊 Logbook entries for audit trail

**Typical Use Cases:**
- ❄️ **Freezing** (threshold 2-5°C): Basements, garages, water pipes
- 🔥 **Overheating** (threshold 30-40°C): Server rooms, network cabinets, attics

**Installation:**
```
https://raw.githubusercontent.com/joshuaaaaa/HA--Fire-Detection/main/automation/temperature_alert/temperature_alert.yaml
```

[💡 Examples](./examples/temperature_alert/)

---

### 💡 [Blink/Flash Device Controller](./automation/blink_flash/)

Universal blueprint for automatic device blinking/pulsing with precise timing control.

**Key Features:**
- 💡 Support for lights, switches, fans, sirens
- ⏱️ Independent ON/OFF intervals (0.1 - 3600 seconds)
- 🔄 3 modes: Unlimited, Time-limited, Repeat count
- 🎨 Configurable color and brightness for lights
- 🔚 Final state: Turn off, Turn on, or Restore original
- 🎯 Optional trigger and conditions
- 🚀 Restart mode for instant reaction

**Use Cases:**
- 🚨 Alarms and warnings (fast red blinking)
- 🔔 Notifications (pulsing lights)
- 🎉 Effects and entertainment (color patterns)
- 🏠 Alerts (doorbell, timer)

**Installation:**
```
https://raw.githubusercontent.com/joshuaaaaa/HA--Fire-Detection/main/automation/blink_flash/blink_flash.yaml
```

[💡 Examples](./examples/blink_flash/)

---

## 🚀 Quick Start

### 1. Preparation (Fire Detection with cameras only)

If you plan to use camera snapshots, create a folder for photos:

**Via File Editor:**
1. Open **File Editor** in Home Assistant
2. Navigate to `/config/www/` folder
3. Create a new folder named `fire_snapshots`

**Or via SSH/Terminal:**
```bash
mkdir /config/www/fire_snapshots
```

### 2. Import Blueprint

1. Open Home Assistant
2. Go to **Settings** → **Automations & Scenes** → **Blueprints**
3. Click **Import Blueprint** (bottom right)
4. Paste the blueprint URL (see above)
5. Click **Preview** and **Import**

### 3. Create Automation

1. Go to **Automations & Scenes** → **Automations**
2. Click **Create Automation** → **Create from Blueprint**
3. Select the imported blueprint
4. Configure according to your needs
5. Save and test

### 4. Use Examples

All blueprints have examples in the [`examples/`](./examples/) folder. Copy, modify, and use!

## 📚 Documentation

Each blueprint includes:

- **Inline code comments** - Detailed description of each section
- **Usage examples** - Ready-to-use configurations in `examples/`
- **FAQ in this README** - Answers to common questions

## 💡 Usage Examples

### Fire Detection Blueprint

```yaml
# Basic home fire protection with cameras
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
# Automatic freeze prevention
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
# Flashing light on alarm
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

## 🔧 Repository Structure

```
HA--Fire-Detection/
├── README.md                                    # This file (English)
├── README.cs.md                                 # Czech version
├── LICENSE                                      # MIT License
├── automation/                                  # Home Assistant Blueprints
│   ├── fire_detection/
│   │   └── fire_detection_v7.yaml              # Fire Detection Blueprint
│   ├── temperature_alert/
│   │   └── temperature_alert.yaml              # Temperature Alert Blueprint
│   └── blink_flash/
│       └── blink_flash.yaml                    # Blink/Flash Blueprint
└── examples/                                    # Configuration examples
    ├── basic_home_protection.yaml              # Fire: Basic protection
    ├── full_protection_with_cameras.yaml       # Fire: Full protection
    ├── night_mode_quiet.yaml                   # Fire: Night mode
    ├── garage_workshop.yaml                    # Fire: Garage/workshop
    ├── temperature_alert/
    │   ├── freeze_alert_basic.yaml             # Temp: Basic freeze alert
    │   ├── freeze_prevention_auto.yaml         # Temp: Auto heating
    │   ├── overheat_alert.yaml                 # Temp: Overheat alert
    │   └── pipe_freeze_prevention.yaml         # Temp: Pipe prevention
    └── blink_flash/
        ├── basic_blink.yaml                    # Blink: Basic
        ├── alarm_flash.yaml                    # Blink: Alarm
        ├── slow_pulse.yaml                     # Blink: Slow pulse
        └── doorbell_count.yaml                 # Blink: Doorbell
```

## 🤝 Contributing

Suggestions, bug reports, and pull requests are welcome!

### How to contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 🐛 Bug Reports

If you find an issue:

1. Check [Issues](https://github.com/joshuaaaaa/HA--Fire-Detection/issues) to see if it's already reported
2. Create a new Issue with details:
   - Blueprint you're using
   - Home Assistant version
   - Problem description
   - Steps to reproduce
   - Logs (if relevant)

## ❓ FAQ

### Q: Do blueprints work without internet?
**A:** Yes, most features work locally. Exception: Telegram notifications require internet.

### Q: Can I use multiple blueprints simultaneously?
**A:** Yes! Blueprints are independent and you can have any number of them.

### Q: How do I update a blueprint?
**A:** Simply re-import the URL. HA automatically updates existing automations.

### Q: Are blueprints compatible with all HA versions?
**A:** Tested on HA 2024.1+. Older versions may have issues with some features.

### Q: Can I modify blueprints?
**A:** Yes! MIT license allows any modifications. You can customize them as needed.

### Q: Why aren't camera photos coming through Telegram?
**A:** Check:
1. Is the folder `/config/www/fire_snapshots/` created
2. Is the "Enable Camera Snapshots" toggle on
3. Is the "Enable Telegram Notifications" toggle on
4. Is at least one camera selected
5. Is the Telegram service correctly configured (e.g., `notify.telegram`)

### Q: What are the latest blueprint improvements?
**A:** All blueprints were recently refactored:

**Fire Detection v7:**
- Telegram text message sent IMMEDIATELY (no waiting for photos)
- Photos saved to separate folder `/config/www/fire_snapshots/`
- Added 2s delay between snapshot and sending (more reliable)
- Pre-computed messages for better performance
- Restart mode for instant reaction

**Temperature Alert (new):**
- Double-check logic: CHECK → DELAY → RECHECK (more robust)
- Pre-computed messages (telegram, HA, TTS)
- Restart mode instead of single for better responsiveness
- Cooldown check in variables for better performance
- Logbook entries for audit trail
- No top-level condition - everything handled in action (more flexible)

## 📊 Statistics

- **Number of blueprints**: 3
- **Total examples**: 12
- **Supported domains**: 15+ (light, switch, camera, sensor, climate, water_heater, fan, etc.)
- **Languages**: English + Czech

## 🔗 Useful Links

- [Home Assistant](https://www.home-assistant.io/)
- [Home Assistant Community](https://community.home-assistant.io/)
- [Blueprint Documentation](https://www.home-assistant.io/docs/automation/using_blueprints/)
- [YAML Guide](https://www.home-assistant.io/docs/configuration/yaml/)

## 📜 License

MIT License - freely usable for personal and commercial purposes.

See [LICENSE](./LICENSE) for details.

## ⚠️ Important Notice

The **Fire Detection Blueprint** is intended as supplementary protection. It does not replace professional fire alarms, smoke detectors, and fire extinguishers. Always comply with local fire regulations and building codes.

---

**Created with ❤️ for the Home Assistant community**

🔥 Stay Safe! 💡 Be Creative!
