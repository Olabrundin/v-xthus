# 🌱 Greenhouse Water Monitor

ESP32-C6 based water flow monitoring for greenhouse using ESPHome, Thread, and Home Assistant.

## Hardware

| Component | Model | Purpose |
|---|---|---|
| Microcontroller | ESP32-C6 Mini | Thread + ESPHome node |
| Flow sensor | YF-S201 | Pulse-based water flow measurement |
| Level converter | TXS0108E (8ch) | 5V → 3.3V signal conversion |

## Architecture

```
YF-S201 (5V) → TXS0108E → ESP32-C6 Mini → Thread → HA Border Router → Home Assistant
```

## Wiring

```
YF-S201          TXS0108E              ESP32-C6 Mini
─────────        ────────────          ─────────────
VCC  (red)  ───► VB (5V side)
GND  (black)───► GND          ◄──────── GND
Signal(yel) ───► B1                     
                 A1 (3.3V) ───────────► GPIO4

                 VA  ◄──────────────── 3.3V
                 VB  ◄──────────────── 5V (external)
                 OE  ◄──────────────── 3.3V (enable HIGH)
```

### Pin Reference – TXS0108E

| Pin | Connect to |
|---|---|
| VA | 3.3V (ESP32 side) |
| VB | 5V (sensor side) |
| OE | 3.3V (must be HIGH to enable) |
| B1 | YF-S201 signal wire (yellow) |
| A1 | GPIO4 on ESP32-C6 |
| GND | Common ground |

## Project Structure

```
greenhouse-water-monitor/
├── esphome/
│   ├── greenhouse-water.yaml      # Main ESPHome config
│   └── secrets.yaml.template      # Secrets template
├── homeassistant/
│   ├── dashboards/
│   │   └── water-dashboard.yaml   # Lovelace dashboard
│   └── automations/
│       └── water-alerts.yaml      # Alert automations
├── docs/
│   ├── setup.md                   # Step-by-step setup guide
│   └── calibration.md             # Flow sensor calibration
├── diagrams/
│   └── wiring.md                  # Detailed wiring diagram (ASCII)
└── README.md
```

## Quick Start

1. Copy `esphome/secrets.yaml.template` → `esphome/secrets.yaml` and fill in your values
2. Flash ESP32-C6 via USB: `esphome run esphome/greenhouse-water.yaml`
3. Verify Thread connection in HA: **Settings → System → Thread**
4. Import dashboard from `homeassistant/dashboards/water-dashboard.yaml`
5. Import automations from `homeassistant/automations/water-alerts.yaml`

## Requirements

- Home Assistant with Thread Border Router (HA Yellow, SkyConnect, or Apple/Google device)
- ESPHome 2024.6.0 or later (esp-idf framework required for Thread)
- ESP32-C6 with antenna

## License

MIT
