# WyzeSense2MQTT (devel)

> ⚠️ **This is an unstable development build.** It tracks the `devel` branch
> of WyzeSense2MQTT and may contain incomplete features, breaking changes, or
> bugs at any time. **Do not use in production.**

For stable usage, install the **WyzeSense2MQTT** app instead.

## Differences from the Stable App

- Pulls the `devel` image tag (built from the `devel` branch).
- `boot` is set to `manual` — will not auto-start on HAOS boot.
- `log_level` defaults to `DEBUG` for easier troubleshooting.
- `mqtt_client_id` defaults to `ws2m-devel` to avoid collisions if both
  stable and devel apps are installed simultaneously.

## Configuration

Configuration options are identical to the stable app. See the
[stable app documentation](../wyzesense2mqtt/DOCS.md) for full details.

## Reporting Issues

Please report issues against the `devel` branch of the source project:
https://github.com/raetha/wyzesense2mqtt/issues
