# wt32-eth01

WT32-ETH01 wired Ethernet ESP32 device. Used primarily as a Bluetooth proxy for Home Assistant over a reliable wired connection — no WiFi required.

## Hardware

| Component | Detail |
|---|---|
| Board | WT32-ETH01 (ESP32 + LAN8720) |
| Framework | Arduino |
| Connectivity | Wired Ethernet (LAN8720) |
| Status LED | Onboard LED → GPIO2 (inverted) |

## Wiring / Pinout

Ethernet is handled onboard by the LAN8720 PHY. No external wiring needed beyond power and an RJ45 cable.

| Signal | GPIO |
|---|---|
| MDC | GPIO23 |
| MDIO | GPIO18 |
| CLK (ext in) | GPIO0 |
| PHY power | GPIO16 |
| Status LED | GPIO2 |

## Entities

| Entity | Description |
|---|---|
| `sensor.wt32_eth01_uptime` | Device uptime, updated every 60s |
| `text_sensor.wt32_eth01_ip_address` | Current Ethernet IP address |
| `text_sensor.wt32_eth01_esphome_version` | ESPHome firmware version |
| `binary_sensor.wt32_eth01_status` | HA API connection status |

Also exposes a web UI on port 80 for diagnostics.

## ESPHome device stub

```yaml
packages:
  wt32_eth01: github://bgulla/esphome-configs/wt32-eth01/wt32-eth01.yaml@main

esphome:
  name: wt32-eth01
  friendly_name: WT32-ETH01
  platformio_options:
    board_build.flash_mode: dio

esp32:
  board: esp32dev
  framework:
    type: arduino

logger:

api:
  encryption:
    key: # ENTER KEY

ota:
  - platform: esphome
    password: # ENTER PASSWORD
```

No `wifi` or `secrets.yaml` entries needed — this device runs on Ethernet only.
