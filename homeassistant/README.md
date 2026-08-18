# Home Assistant merge

Copy or merge these files into your Home Assistant config directory.

| This repo | Your HA config |
| --- | --- |
| `templates/m5stick_pikud.yaml` | `config/templates/m5stick_pikud.yaml` |
| `input_select.yaml` | merge into `config/input_select.yaml` |
| `input_number.yaml` | merge into `config/input_number.yaml` |
| `scripts.yaml` | merge into `config/scripts.yaml` |

Ensure `configuration.yaml` includes:

```yaml
template: !include_dir_merge_list templates/
input_select: !include input_select.yaml
input_number: !include input_number.yaml
script: !include scripts.yaml
```

See `configuration.snippet.yaml` for reference.

## Prerequisites

- [Oref Alert](https://github.com/amitfin/oref_alert) integration installed — provides `sensor.oref_alert` and `sensor.oref_alert_time_to_shelter`.
- ESPHome integration used to pair the stick after first flash.
- Optional: [Battery Notes](https://github.com/andrew-codechimp/Battery-Notes) — auto-discovers `sensor.m5stick_s3_battery_level` from the ESPHome device.

## After merge

1. Check configuration: **Developer tools → YAML → Check configuration**
2. Restart Home Assistant
3. Confirm entities exist:
   - `sensor.m5stick_pikud_display_state`
   - `input_select.m5stick_pikud_test_mode`
   - `input_select.m5stick_pikud_alert_language`
   - `input_number.m5stick_pikud_sync`
   - `input_number.m5stick_pikud_safe_minutes`
   - `script.m5stick_pikud_test_*`
   - `sensor.m5stick_s3_battery_level` (on the ESPHome device after OTA)
   - `binary_sensor.m5stick_s3_charging`

Pikud **lighting/scene automations** still listen to `sensor.oref_alert` only. Test scripts drive the stick via `sensor.m5stick_pikud_display_state` without touching scenes.

## New helpers

| Entity | Purpose |
| --- | --- |
| `input_number.m5stick_pikud_safe_minutes` | How long the green SAFE screen shows after alert clears (minutes) |
| `input_select.m5stick_pikud_alert_language` | `english` or `hebrew` for on-screen alert text |
