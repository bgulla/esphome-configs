# mmwave-study

ESP32-C3 with an HLK-LD2410 mmwave presence sensor, deployed in a study/office. Detects presence, movement, and still targets with per-gate energy tuning. Also acts as a Bluetooth proxy for Home Assistant.

## Hardware

| Component | Detail |
|---|---|
| Board | ESP32-C3 DevKitM-1 |
| Framework | Arduino |
| Radar | HLK-LD2410 mmwave presence sensor |
| Status LED | Blue LED → GPIO2 |

## Wiring

| Signal | GPIO |
|---|---|
| UART TX (to LD2410 RX) | GPIO21 |
| UART RX (from LD2410 TX) | GPIO20 |
| Status LED | GPIO2 |

## Sensors & Entities

### Presence
| Entity | Description |
|---|---|
| `binary_sensor.mmwave_study_presence` | Any target detected |
| `binary_sensor.mmwave_study_moving_target` | Moving target detected |
| `binary_sensor.mmwave_study_still_target` | Still target detected |
| `sensor.mmwave_study_moving_distance_cm` | Distance to moving target |
| `sensor.mmwave_study_still_distance_cm` | Distance to still target |
| `sensor.mmwave_study_distance_detection_cm` | Overall detection distance |
| `sensor.mmwave_study_move_energy` | Moving target signal energy % |
| `sensor.mmwave_study_still_energy` | Still target signal energy % |

### Per-gate energy (g0–g8)
Each detection gate (~0.75m) exposes `move_energy` and `still_energy` sensors for tuning sensitivity.

### Diagnostics
| Entity | Description |
|---|---|
| `sensor.mmwave_study_internal_temperature` | ESP32-C3 die temperature |
| `text_sensor.mmwave_study_presence_sensor_version` | LD2410 firmware version |
| `text_sensor.mmwave_study_presence_sensor_mac_address` | LD2410 BT MAC |

### Controls
| Entity | Description |
|---|---|
| `switch.mmwave_study_engineering_mode` | Enable per-gate energy reporting |
| `switch.mmwave_study_control_bluetooth` | Toggle LD2410 onboard BT |
| `number.mmwave_study_timeout` | Presence hold-off time |
| `number.mmwave_study_max_move_distance_gate` | Max detection gate for motion |
| `number.mmwave_study_max_still_distance_gate` | Max detection gate for still |
| `button.mmwave_study_factory_reset` | Factory reset LD2410 |
| `button.mmwave_study_restart` | Restart LD2410 |

## ESPHome device stub

```yaml
packages:
  mmwave_study: github://bgulla/esphome-configs/mmwave-study/mmwave-study.yaml@main

esphome:
  name: mmwave-study
  friendly_name: mmwave-study

esp32:
  variant: ESP32C3
  board: esp32-c3-devkitm-1
  framework:
    type: arduino

logger:

api:
  encryption:
    key: # ENTER KEY

ota:
  - platform: esphome
    password: # ENTER PASSWORD

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
  output_power: 8.5dB
  power_save_mode: none
  ap:
    ssid: "Mmwave-Study Fallback Hotspot"
    password: # ENTER PASSWORD

captive_portal:
```

Add to your ESPHome `secrets.yaml`:

```yaml
wifi_ssid: "..."
wifi_password: "..."
```
