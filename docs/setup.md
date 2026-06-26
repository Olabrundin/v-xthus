# Setup Guide

## Prerequisites

- Home Assistant OS or Supervised (2024.1+)
- ESPHome add-on installed in HA
- Thread Border Router active (HA Yellow, SkyConnect USB dongle, or Apple/Google device)
- Python 3.8+ (optional, for CLI flashing)

---

## Step 1 – Prepare Secrets

```bash
cp esphome/secrets.yaml.template esphome/secrets.yaml
```

Generate an API key:
```bash
python3 -c "import secrets, base64; print(base64.b64encode(secrets.token_bytes(32)).decode())"
```

Edit `secrets.yaml` and paste the key + a strong OTA password.

---

## Step 2 – Wire the Hardware

See `diagrams/wiring.md` for full wiring diagram.

Quick summary:
1. Connect YF-S201 VCC (red) → 5V supply
2. Connect YF-S201 GND (black) → common GND
3. Connect YF-S201 signal (yellow) → TXS0108E **B1**
4. Connect TXS0108E **A1** → ESP32-C6 GPIO4
5. Connect TXS0108E **VA** → 3.3V, **VB** → 5V, **OE** → 3.3V, **GND** → GND

---

## Step 3 – Flash ESP32-C6

### Via ESPHome Dashboard (recommended)
1. Open ESPHome in HA
2. Click **+ New Device** → **Skip** (manual config)
3. Paste content of `esphome/greenhouse-water.yaml`
4. Click **Install** → **Plug into this computer**
5. Select your serial port and flash

### Via CLI
```bash
pip install esphome
esphome run esphome/greenhouse-water.yaml
```

---

## Step 4 – Verify Thread Connection

1. In HA go to **Settings → System → Thread**
2. Your ESP32-C6 should appear as a Thread end device after ~60 seconds
3. In ESPHome dashboard the device shows **Online**

---

## Step 5 – Import Dashboard

1. In HA go to **Settings → Dashboards → Add Dashboard**
2. Click the three-dot menu → **Edit Dashboard** → **Raw configuration editor**
3. Paste content of `homeassistant/dashboards/water-dashboard.yaml`
4. Save

---

## Step 6 – Import Automations

1. In HA go to **Settings → Automations → + Add Automation**
2. Click the three-dot menu → **Edit in YAML**
3. Paste each automation block from `homeassistant/automations/water-alerts.yaml`
4. Save and enable

---

## Troubleshooting

| Issue | Solution |
|---|---|
| No pulses detected | Check OE pin on TXS0108E is connected to 3.3V |
| Flow reads 0 always | Verify GPIO4 is correct; check wiring continuity |
| Thread not connecting | Ensure Border Router is active in HA Thread settings |
| OTA fails | Check OTA password matches secrets.yaml |
| Wrong flow values | See `docs/calibration.md` to adjust pulse factor |
