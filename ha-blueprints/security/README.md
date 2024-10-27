# 🔐 Paradox Zone Parser Blueprint

A comprehensive Home Assistant blueprint that automatically creates entities from Paradox alarm zone MQTT messages, specifically designed for MG5050, EVO192, and other Paradox alarm systems.

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://github.com/jlwainwright/ha-blueprints/blob/main/security/paradox_zone_parser/paradox_zone_parser.yaml)
[![Buy Me A Coffee](https://img.shields.io/badge/buy%20me%20a%20coffee-donate-yellow.svg)](https://www.buymeacoffee.com/jlwainwright)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![HA Community](https://img.shields.io/badge/Home%20Assistant-Community-41BDF5.svg)](https://community.home-assistant.io/t/paradox-zone-parser-blueprint/12345)

## 🌟 Features

- 📱 Creates individual entities for all zone states (motion, tamper, fire, etc.)
- 🔋 Monitors signal strength with proper device classes
- 🎯 Device triggers for automations (motion and tamper events)
- 📊 Groups all entities under a device with proper manufacturer info
- 🏠 Auto-suggests areas based on zone names
- ⚙️ Configurable MQTT topic prefix
- 🔍 Proper error handling with default values

## 📋 Requirements

- Home Assistant 2024.1.0 or newer
- MQTT integration
- Paradox alarm system (tested with MG5050, EVO192)
- Zone states published to MQTT topics following pattern: `paradox/states/zones/ZONE_NAME`

## 🔧 Installation

### Method 1: Using My Home Assistant
1. Click the "Add Blueprint" badge above
2. Follow the import steps in Home Assistant
3. Create your automations using the blueprint

### Method 2: Manual Installation
1. Copy the [paradox_zone_parser.yaml](./paradox_zone_parser.yaml) content
2. In Home Assistant, go to `Configuration -> Blueprints`
3. Click `Import Blueprint`
4. Paste the YAML content and import

## ⚙️ Configuration

### Blueprint Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| zone_name | Name of the zone (e.g., Lapa_PIR) | Yes | None |
| device_model | Model of your Paradox system | No | MG5050 |
| mqtt_prefix | MQTT topic prefix | No | paradox |
| create_triggers | Create device triggers | No | true |

### Example Configuration

```yaml
mqtt: !include_dir_merge_list mqtt/

# Create directory mqtt/ and add file paradox_zones.yaml:
- !blueprint 
  path: paradox_zone_parser.yaml
  input:
    zone_name: Lapa_PIR
    device_model: MG5050
    mqtt_prefix: paradox

- !blueprint
  path: paradox_zone_parser.yaml
  input:
    zone_name: Lounge_PIR
    device_model: MG5050
    mqtt_prefix: paradox
```

## 📤 Generated Entities

For each zone, the blueprint creates:

### Binary Sensors
| Sensor | Device Class | Category | Description |
|--------|--------------|-----------|-------------|
| Open | motion | - | Zone motion status |
| Tamper | tamper | diagnostic | Tamper status |
| Fire | smoke | - | Fire detection |
| RF Supervision | problem | diagnostic | RF supervision status |
| Battery | battery | diagnostic | Battery status |
| Alarm States | problem | - | Various alarm states |

### Sensors
| Sensor | Device Class | Category | Description |
|--------|--------------|-----------|-------------|
| Signal Strength | signal_strength | diagnostic | RF signal strength in dBm |

### Device Triggers
- Motion detection
- Tamper detection

## 📝 Example MQTT Payload

```json
{
  "signal_strength": 5,
  "open": false,
  "tamper": false, 
  "fire": false,
  "rf_supervision_trouble": false,
  "rf_low_battery_trouble": false,
  "was_in_alarm": false,
  "alarm": false,
  "fire_delay": false,
  "entry_delay": false,
  "intellizone_delay": false,
  "no_delay": true,
  "bypassed": false,
  "shutdown": false,
  "in_tx_delay": false,
  "was_bypassed": false,
  "exit_delay": false
}
```

## ❗ Troubleshooting

### No Entities Appearing
- Verify MQTT integration is configured
- Check zone_name matches MQTT topic exactly
- Confirm MQTT messages are being published

### Missing States
- Check MQTT payload format
- Verify all states are being published
- Check Home Assistant MQTT logs

### Device Grouping Issues
- Ensure consistent zone_name usage
- Verify device_model is correct
- Check manufacturer info consistency

## 🔄 Version History

### 1.1.0
- Added configurable MQTT prefix
- Added device model selection
- Improved error handling
- Added unique_ids to all entities

### 1.0.0
- Initial release
- Full MG5050 zone state support
- Automatic entity organization
- Signal strength monitoring

## 👤 Author & Support

**Created and maintained by [Jacques Wainwright]**
- 💻 [GitHub](https://github.com/jlwainwright)

### Support My Work
If you find this blueprint helpful:
- ⭐ Star the repository
- ☕ [Buy me a coffee](https://buymeacoffee.com/jlwainwright)
- 🐛 Report issues
- 🛠️ Submit improvements

## 💝 Special Thanks

Thanks to all contributors and supporters who've helped improve this blueprint!

## 📜 License

This blueprint is licensed under the MIT License - see the [LICENSE](../LICENSE) file for details.

---
