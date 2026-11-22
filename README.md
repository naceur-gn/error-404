# robot-controler-via-mqtt

A browser-based **DroneShield** controller that publishes **signed commands** to an MQTT broker.

## What it does

Every tap publishes a signed JSON command to

```
droneshield/<device_id>/commands
```

on the HiveMQ broker (browser uses WSS; firmware listens on TLS 8883), with an HMAC-SHA256 signature matching `drone_attack_sim/common.py` (`sign`, `make_payload`):

```json
{"device_id":"device_01","seq":1,"timestamp":1700000000,"command":"ARM","params":{},"signature":"<64 hex>"}
```

- **seq** — monotonically increasing `uint64`, persisted locally (anti-replay).
- **timestamp** — current Unix time (within 300 s).
- **command** — `STATUS_REQUEST`, `PING`, `ARM`, `DISARM`, `SIM_RESET`.
- **params** — object with sorted keys, compact (used in HMAC).
- **signature** — HMAC-SHA256 hex of `device_id|seq|timestamp|command|<canonical params>` using `DSHL_HMAC_KEY`.

## UI

- Light, clean theme (easy on the eyes)
- Status dot, link/RX/SEQ/topic chips
- Command buttons: ARM, DISARM, STATUS, PING, SIM RESET — each shows the target topic
- Terminal log (TX/RX)
- Settings: broker WSS URL, username, password (never stored), device ID, HMAC key (never stored)

## Run

Open `index.html` in a browser, or deploy the folder as a static site (Vercel config included).

Broker credentials and HMAC key must match the firmware (`drone_attack_sim/config.py`).
