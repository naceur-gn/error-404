# robot-controler-via-mqtt

A browser-based **DroneShield flight controller** that drives the drone over **MQTT**, secured with HMAC.

## What it does

Movement buttons (up/down/yaw/pitch/roll) and arm/disarm send **signed commands** to

```
droneshield/<device_id>/commands
```

on the HiveMQ broker (browser uses WSS; firmware listens on TLS 8883). Every message is signed like `drone_attack_sim/common.py` (`sign`, `make_payload`):

```json
{"device_id":"device_01","seq":1,"timestamp":1700000000,"command":"UP","params":{"state":1},"signature":"<64 hex>"}
```

- **seq** — monotonically increasing `uint64`, persisted locally (anti-replay).
- **timestamp** — current Unix time (within 300 s).
- **command** — `UP`, `DOWN`, `YAW_LEFT`, `YAW_RIGHT`, `FORWARD`, `BACK`, `LEFT`, `RIGHT` (1 on press, 0 on release), plus `ARM`, `DISARM`, `STATUS_REQUEST`, `PING`, `SIM_RESET`.
- **params** — `{state: 1|0}` for movement holds, `{}` otherwise.
- **signature** — HMAC-SHA256 hex of `device_id|seq|timestamp|command|<canonical params>` using `DSHL_HMAC_KEY`.

## UI

- Light, clean theme; landscape game-console layout
- Left pad — climb, descend, yaw · Right pad — pitch, roll
- Centre — ARM, DISARM, STATUS, PING, RESET; status dot; link/RX/SEQ chips
- Terminal log (TX/RX); keyboard (WASD, arrows, IJKL, Space, Shift+D)
- Settings: broker WSS URL, username, password (never stored), device ID, HMAC key (never stored)

## Run

Open `index.html` in a browser, or deploy the folder as a static site (Vercel config included).

Broker credentials and HMAC key must match the firmware (`drone_attack_sim/config.py`).
