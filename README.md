# robot-controler-via-mqtt

A browser-based **mission controller** that sends waypoint (x, y) missions over **MQTT**.

## What it does

Each "Send Mission" publishes a simple JSON message to a configurable topic (default `drone/mission`):

```json
{ "waypoints": [ { "x": 1.5, "y": 2.0 }, { "x": 3.0, "y": 0.5 } ] }
```

- No signing — plain JSON over MQTT.
- Broker credentials are never stored (password not persisted).
- Broker URL, username and publish topic are remembered on this device.

## UI

- Light, clean theme
- Status dot, link/RX/topic chips
- Waypoint inputs (x, y) + Add, editable list, Send Mission / Clear
- Terminal log (TX/RX)
- Settings: broker WSS URL, username, password, publish topic

## Run

Open `index.html` in a browser, or deploy the folder as a static site (Vercel config included).
