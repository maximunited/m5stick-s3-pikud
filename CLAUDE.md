# CLAUDE.md

M5Stack Stick S3 Pikud HaOref display — ESPHome firmware and Home Assistant integration snippets.

## Layout

- `esphome/m5stick-s3.yaml` — device firmware, LVGL UI, button handling
- `homeassistant/templates/m5stick_pikud.yaml` — display driver sensor
- `homeassistant/scripts.yaml` — test scripts (no Pikud scene side effects)
- `docs/ARCHITECTURE.md` — state machine and data flow

## Key entities

- `sensor.m5stick_pikud_display_state` — ESPHome subscribes to this (`<semantic>|<sync>`)
- `sensor.oref_alert` — real Oref state; lighting automations use this
- `input_select.m5stick_pikud_test_mode` — live vs test override

## Validate

```bash
cp esphome/secrets.yaml.sample esphome/secrets.yaml
cd esphome && esphome config m5stick-s3.yaml
```

## Flash / OTA

```bash
esphome run m5stick-s3.yaml --device /dev/ttyACM0      # USB
esphome run m5stick-s3.yaml --device m5stick-s3.local  # OTA
```
