# esphome-configs

ESPHome device configurations managed in version control. Each device has its own folder at the repo root. Secrets are never committed — they're stored in ESPHome's own `secrets.yaml`.

## Repo structure

```
<device-name>/
  <device-name>.yaml    # ESPHome config
  README.md             # Hardware, sensors, secrets, HA card
  dashboard.png         # HA dashboard screenshot (if applicable)
```

### Devices

| Device | Board | Description |
|---|---|---|
| [keg-scale](keg-scale/) | NodeMCU v2 (ESP8266) | HX711 kegerator scale with pour tracking |
| [mmwave-study](mmwave-study/) | ESP32-C3 DevKitM-1 | HLK-LD2410 mmwave presence sensor + BT proxy, study room |
| [wt32-eth01](wt32-eth01/) | WT32-ETH01 (ESP32) | Wired Ethernet Bluetooth proxy |
| [button-box](button-box/) | ESP32-C3 DevKitM-1 | 4-button HA scene controller with OLED display |

## Usage

The full device logic lives in this repo. Your ESPHome instance only needs a minimal stub file that pulls the package from GitHub and supplies secrets.

1. Create a new device config in your ESPHome instance using the stub shown in each device's README.
2. Add the required secret keys (listed in each device's README) to your ESPHome `secrets.yaml`.
3. Flash via the ESPHome dashboard or CLI.

ESPHome will fetch the config from GitHub automatically at flash time.
