# button-box

ESP32-C3 physical scene controller with 4 buttons and an SH1106 OLED display. Each button triggers a Home Assistant scene and briefly shows a confirmation message on the display before returning to the scene menu.

## Hardware

| Component | Detail |
|---|---|
| Board | ESP32-C3 DevKitM-1 |
| Framework | esp-idf |
| Display | SH1106 128x64 OLED (I2C 0x3C, rotated 180°) |
| I2C | SDA → GPIO5, SCL → GPIO6 |

## Button Wiring

| Button | Wire Color | GPIO | HA Scene |
|---|---|---|---|
| Button 1 | Gray | GPIO4 | `scene.scene_1` — Office |
| Button 2 | Purple | GPIO2 | `scene.scene_2` — Garage |
| Button 3 | Black | GPIO10 | `scene.scene_3` — Party Mode |
| Button 4 | White | GPIO3 | `scene.scene_4` — BOOYA |

All buttons are active-low with internal pull-up.

## Behavior

- OLED shows a scene menu at idle (page_main)
- On button press: triggers the corresponding HA scene, shows a confirmation page for 5 seconds, then returns to the menu

## HA Scenes Required

| Scene entity | Label |
|---|---|
| `scene.scene_1` | Office |
| `scene.scene_2` | Garage |
| `scene.scene_3` | Party Mode |
| `scene.scene_4` | BOOYA |

## ESPHome device stub

```yaml
packages:
  button_box: github://bgulla/esphome-configs/button-box/button-box.yaml@main

esphome:
  name: button-box
  friendly_name: button-box

esp32:
  variant: ESP32C3
  board: esp32-c3-devkitm-1
  framework:
    type: esp-idf

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
```

Add to your ESPHome `secrets.yaml`:

```yaml
wifi_ssid: "..."
wifi_password: "..."
```
