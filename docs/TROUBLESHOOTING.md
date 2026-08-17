# Troubleshooting

## Green LED flashing, blank screen

The stick is in **USB download mode**.

- **Single-click** the side button to boot the app.
- Or long-press to stay in bootloader for flashing (`303a:1001` in `lsusb`).

## USB flash fails / device disappears

`dmesg` shows `usb disconnect` during upload.

1. Reseat USB cable (direct to host, not through a flaky hub).
2. Retry: `esphome run m5stick-s3.yaml --device /dev/ttyACM0`
3. If stuck in bootloader, flash again while `303a:1001` is visible.

## Never joins Wi-Fi

| Check | Fix |
| --- | --- |
| 5 GHz only network | Use 2.4 GHz SSID |
| SSID typo in `secrets.yaml` | Match router exactly (quotes, spaces) |
| Malformed YAML | No backslash-escaped quotes around SSID |
| Wrong password | Re-flash after fixing `secrets.yaml` |

Use `m5stick-s3-minimal.yaml` to isolate Wi-Fi from display code.

## Stick shows green SAFE at idle

Fixed in current firmware: live `ok` only triggers SAFE after a real `alert`/`pre_alert` → `ok` transition. Idle `ok` shows clock.

If you still see it after OTA, confirm HA template publishes `test_ok` for test safe (not bare `ok`) and firmware parses the `|` sync suffix.

## Test script does nothing

`input_select` may not change the template output when the base state repeats.

Test scripts bump `input_number.m5stick_pikud_sync` so `sensor.m5stick_pikud_display_state` always republishes (`<state>|<sync>`).

## HA pairing fails

| Check | Fix |
| --- | --- |
| Port 6053 closed | Stick not on Wi-Fi — fix Wi-Fi first |
| Wrong encryption key | Use exact `api_encryption_key` from `secrets.yaml` |
| mDNS blocked | Use IP address instead of `m5stick-s3.local` |

## Clock wrong or missing

Time comes from HA (`homeassistant` time platform). Pair ESPHome integration first; HA must be reachable at runtime.

## OTA fails but USB works

```bash
esphome run m5stick-s3.yaml --device <stick-ip>
```

Ensure `ota_password` in `secrets.yaml` matches the flashed firmware.

## Shelter countdown shows "?"

`sensor.oref_alert_time_to_shelter` unavailable or NaN. Normal outside active alerts; press B during a live `alert` state.
