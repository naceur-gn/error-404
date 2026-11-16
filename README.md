# robot-controler-via-mqtt

A browser-based robot / quad controller that drives any device over **MQTT** (WebSocket).

## Features

- **Light, clean interface** — soft white theme, high contrast, easy on the eyes
- **Landscape game-console layout** — dual control pads (throttle & yaw · pitch & roll) with hold feedback and haptics
- **Topic on every button** — each control prints the exact MQTT topic it publishes (updates live with the base topic)
- **Arm / Mode safety switches** — armed (green) · disarmed (red) · stabilized / acro
- **Live link telemetry** — connection state, RX counter and last received message in the console
- **Settings sheet** — broker WebSocket URL, username, password (never stored) and base topic, remembered locally
- **Keyboard support** — `WASD` / arrows · `IJKL` · `Space` arm · `M` mode · `Esc` close
- **Safety release** — releasing a button, switching tab or hiding the page publishes `0` for every held command
- **Portrait guard** — asks you to rotate to landscape, like a real handheld controller

## MQTT topics (base topic default `quad`)

| Topic | Payload | Meaning |
|---|---|---|
| `<base>/arm` | `1` / `0` | arm / disarm |
| `<base>/mode` | `1` / `0` | acro / stabilized |
| `<base>/up` `<base>/down` | `1` while held | climb / descend |
| `<base>/yaw_left` `<base>/yaw_right` | `1` while held | yaw |
| `<base>/forward` `<base>/back` | `1` while held | pitch |
| `<base>/left` `<base>/right` | `1` while held | roll |

Telemetry is read from `<base>/+` (e.g. `<base>/battery`, `<base>/rssi`) and shown in the console.

## Run

Open `index.html` in a browser, or deploy the folder as a static site (Vercel config included).

```json
{ "framework": null, "buildCommand": null, "installCommand": null, "outputDirectory": "." }
```
