# WS2M Hub

Local Wyze Sense hub that publishes sensor events to MQTT with full
Home Assistant MQTT discovery support. No Wyze account or cloud access required.

For full documentation — usage, pairing, compatible hardware, remote dongles,
and CLI tools — see the
[WS2M Hub documentation](https://github.com/raetha/wyzesense2mqtt#readme).

## Prerequisites

- A **Wyze Sense Bridge** (WHSB1) plugged into a USB port on the HA host.
- Either the **Mosquitto broker app** (recommended — MQTT config is automatic),
  or an external MQTT broker with connection details on hand.

## Configuration

### MQTT

If you use the **Mosquitto broker app**, leave all MQTT fields blank. The app
discovers the broker automatically via the Supervisor API — no host, port, or
credentials needed.

To use an **external broker**, set `mqtt_host` plus any required credentials.

| Option | Default | Description |
|---|---|---|
| `mqtt_host` | `""` | Broker hostname or IP. Blank = auto-discover Mosquitto. |
| `mqtt_port` | `1883` | Broker port. |
| `mqtt_username` | `""` | Username (blank if not required). |
| `mqtt_password` | `""` | Password (blank if not required). |
| `mqtt_client_id` | `ws2m` | MQTT client identifier. |
| `mqtt_clean_session` | `false` | Start with a clean session on each connect. |
| `mqtt_keepalive` | `60` | Keepalive interval in seconds. |

### Topics

| Option | Default | Description |
|---|---|---|
| `self_topic_root` | `ws2m` | Root prefix for hub and sensor messages. |
| `hass_topic_root` | `homeassistant` | Root prefix for HA discovery messages. |
| `hass_discovery` | `true` | Publish HA MQTT discovery payloads. |

### Hardware

| Option | Default | Description |
|---|---|---|
| `usb_dongle` | `auto` | HID device path, or `auto` to detect all connected dongles. |

`auto` (the default) scans for all connected WyzeSense bridge dongles at
startup. Set an explicit path (e.g. `/dev/hidraw0`) only if you need to
restrict which dongle is used.

### Other

| Option | Default | Description |
|---|---|---|
| `log_level` | `INFO` | Verbosity: `DEBUG`, `INFO`, `WARNING`, or `ERROR`. |

## Persistent Data

Data is stored at `/config/ws2m-hub` and is included in HA backups.
