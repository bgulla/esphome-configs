# mm-wave

ESP32-C3 based mmwave presence sensor platform. Currently configured with an SH1106 OLED display, 4 GPIO buttons, and WiFi diagnostics. mmwave radar integration (e.g. LD2410) to be wired in.

## Hardware

| Component | Detail |
|---|---|
| Board | ESP32-C3 DevKitM-1 |
| Framework | esp-idf |
| Display | SH1106 128x64 OLED (I2C 0x3C) |
| I2C | SDA → GPIO5, SCL → GPIO6 |

## Pinout

```
              USB
┌───────────────────┐
│ GPIO5 ──SDA    5V │
│ GPIO6 ──SCL   GND ── Brown  (COM)
│ GPIO7         3.3 │
│ GPIO8        GPIO4 │
│ GPIO9        GPIO3 ── White  (Button 4)
│ GPIO10─Black(Btn3) GPIO2 ── Gray   (Button 1)
│ GPIO20       GPIO1 ── Purple (Button 2)
│ GPIO21       GPIO0 │
└───────────────────┘
```

## Sensors & Components

| Entity | Type | Description |
|---|---|---|
| `sensor.mm_wave_uptime` | sensor | Device uptime in seconds |
| `sensor.mm_wave_wifi_signal` | sensor | WiFi RSSI, updated every 60s |
| `sensor.mm_wave_ip_address` | text sensor | Current IP address |
| `binary_sensor.mm_wave_button_1` | binary sensor | GPIO4, active low |
| `binary_sensor.mm_wave_button_2` | binary sensor | GPIO1, active low |
| `binary_sensor.mm_wave_button_3` | binary sensor | GPIO10, active low |
| `binary_sensor.mm_wave_button_4` | binary sensor | GPIO3, active low |

Display shows IP address and uptime on the OLED at 1s refresh.

## ESPHome device stub

```yaml
packages:
  mm_wave: github://bgulla/esphome-configs/mm-wave/mm-wave.yaml@main

esphome:
  name: mm-wave
  friendly_name: mm-wave

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
