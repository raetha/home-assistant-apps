# Raetha Home Assistant Apps

Home Assistant app repository containing apps maintained by Raetha.

[![Add to Home Assistant](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Fraetha%2Fhome-assistant-apps)

Or add manually in **Settings → Apps → App Store → ⋮ → Repositories**:
```
https://github.com/raetha/home-assistant-apps
```

## Apps

### [WS2M Hub](./ws2m-hub)

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]

_Local Wyze Sense hub with full Home Assistant MQTT discovery support. No Wyze account or cloud access required._

Auto-discovers the Mosquitto broker app — no MQTT configuration needed when running alongside Mosquitto.

Source project: [raetha/wyzesense2mqtt](https://github.com/raetha/wyzesense2mqtt)

---

### [WS2M Hub (devel)](./ws2m-hub-devel)

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]

> ⚠️ **Unstable — for development and testing only.** Installs the test
> builds from the `dev` branch of the source project. May contain incomplete features, breaking
> changes, or bugs. Do not use in production.

---

[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
