# Flow Sensor Calibration

The YF-S201 has a nominal pulse factor of **450 pulses per liter**, but this
can vary ±10% between units and changes slightly with flow rate and water pressure.

## Default Factor

In `greenhouse-water.yaml` the multiply filter is set to:

```yaml
filters:
  - multiply: 0.002222   # = 1/450
```

This converts pulses/min → L/min.

---

## How to Calibrate

1. Place a measuring jug (at least 1 liter) under the outlet
2. Open HA Developer Tools → Template and watch:
   ```
   {{ states('sensor.greenhouse_water_monitor_water_flow_rate') }}
   ```
3. Let exactly **1 liter** flow through and note the pulse count from logs
4. Calculate your factor: `factor = 1 / actual_pulses_per_liter`

### Example

If your sensor gives 480 pulses per liter:
```yaml
filters:
  - multiply: 0.002083   # = 1/480
```

---

## Checking Raw Pulses

Enable DEBUG logging temporarily in `greenhouse-water.yaml`:

```yaml
logger:
  level: DEBUG
```

Watch ESPHome logs – you'll see raw pulse counts per interval, which lets
you calculate the exact pulses-per-liter for your unit.

---

## Typical Values for YF-S201 Variants

| Variant | Pulses/Liter | Multiply factor |
|---|---|---|
| Standard YF-S201 | 450 | 0.002222 |
| YF-S201B | 660 | 0.001515 |
| YF-S401 (smaller) | 5880 | 0.000170 |

Always measure your specific unit for best accuracy.
