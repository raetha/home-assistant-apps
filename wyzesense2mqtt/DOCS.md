# WyzeSense2MQTT

Local Wyze Sense Bridge gateway that publishes sensor events to MQTT with full
Home Assistant MQTT discovery support. No Wyze account or cloud access required.

## Prerequisites

- A **Wyze Sense Bridge** (WHSB1) plugged into a USB port on the HA host.
- Either the **Mosquitto broker app** (recommended — MQTT config is automatic),
  or an external MQTT broker with connection details on hand.

## Configuration

### MQTT

If you use the **Mosquitto broker app**, leave all MQTT fields blank. The app
discovers the broker automatically — no host, port, or credentials needed.

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
| `mqtt_qos` | `0` | QoS level (0, 1, or 2). |
| `mqtt_retain` | `true` | Publish states with the MQTT retain flag. |

### Topics

| Option | Default | Description |
|---|---|---|
| `self_topic_root` | `ws2m` | Root prefix for bridge and sensor messages. |
| `hass_topic_root` | `homeassistant` | Root prefix for HA discovery messages. |
| `hass_discovery` | `true` | Publish HA MQTT discovery payloads. |
| `publish_sensor_name` | `true` | Include sensor names in payloads. |

### Hardware

| Option | Default | Description |
|---|---|---|
| `usb_dongle` | `/dev/hidraw0` | HID device path for the Wyze Sense Bridge. |

To find the correct path, go to **Settings → System → Hardware** and look for
a HID device with vendor `1a86` / product `e024`.

### Other

| Option | Default | Description |
|---|---|---|
| `log_level` | `INFO` | Verbosity: `DEBUG`, `INFO`, `WARNING`, or `ERROR`. |

## Persistent Data

Data is stored at `/config/wyzesense2mqtt` and is included in HA backups:

- `sensors.yaml` — per-sensor settings (name, class, timeout). Safe to edit.
- `state.yaml` — internal runtime state. Do not edit.
- `migrations.yaml` — schema migration tracking. Do not edit.

## Pairing a Sensor

Use the **Scan** button on the **WyzeSense2MQTT Bridge** device page in Home
Assistant, then press the reset switch on the sensor with the pin tool until
the LED blinks. The sensor appears in HA automatically via MQTT discovery.

## Removing a Sensor

Use the **Remove** button on the sensor's device page in Home Assistant.

## Reloading Sensor Config

After editing `sensors.yaml`, use the **Reload** button on the bridge device
page to apply changes without restarting the app.

## Compatible Hardware

**Wyze:** Sense Bridge (WHSB1), Contact Sensor v1, Motion Sensor v1,
Entry Sensor v2 (WSES2), Motion Sensor v2 (WSMS2), Climate Sensor (WSCS1),
Leak Sensor (WSLS1).

**Neos Smart:** Bridge (N-LSP-US1) and sensors (not tested, theoretically
compatible based on shared hardware).
