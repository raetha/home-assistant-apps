# WS2M Hub (devel)

> ⚠️ **This is an unstable development build.** It tracks the `devel` branch
> of WS2M Hub and may contain incomplete features, breaking changes, or
> bugs at any time. **Do not use in production.**

For stable usage, install the **WS2M Hub** app instead.

For full documentation see the
[WS2M Hub documentation](https://github.com/raetha/wyzesense2mqtt#readme).

## Differences from the Stable App

- Pulls the `devel` image tag (built from the `devel` branch).
- `boot` is set to `manual` — will not auto-start on HAOS boot.
- `log_level` defaults to `DEBUG` for easier troubleshooting.
- `mqtt_client_id` defaults to `ws2m-devel` to avoid collisions if both
  stable and devel apps are installed simultaneously.

## Configuration

Configuration options are identical to the stable app. See the
[stable app documentation](../ws2m-hub/DOCS.md) for the full option reference.

## Reporting Issues

Please open issues against the source project:
https://github.com/raetha/wyzesense2mqtt/issues
