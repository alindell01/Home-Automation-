# 40" Wall Dashboard — Integration Contract

Your 40" wall-mounted family dashboard is a **separate code project you own**. Home Assistant
is its **data and command source**. This doc is the decoupled contract so your dashboard code
doesn't need to know HA internals — just entities and service calls.

## 1. Create a dedicated, limited user + token

1. **Settings → People → Add Person** → create a user like `dashboard` (not an admin).
2. Log in as that user once, then **Profile → Long-Lived Access Tokens → Create Token**.
3. Store the token in your dashboard project's secrets — **never commit it** (see
   [network-security.md](network-security.md)). Rotate if the display is lost/repurposed.

> Use a non-admin user so a compromised token can't change HA configuration.

## 2. Live state — WebSocket API

Subscribe to entity state for real-time widgets.

```
wss://<your-nabucasa-or-lan-host>/api/websocket
```

Handshake then subscribe:

```jsonc
// 1. server sends: {"type":"auth_required"}
{ "type": "auth", "access_token": "LONG_LIVED_TOKEN" }
// 2. server sends: {"type":"auth_ok"}
{ "id": 1, "type": "subscribe_entities" }   // pushes initial states + every change
```

You receive a full snapshot, then incremental diffs — ideal for an always-on display.

## 3. Commands — REST API

Call services to actuate devices.

```bash
# Toggle a light
curl -X POST https://<host>/api/services/light/toggle \
  -H "Authorization: Bearer $HA_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"entity_id": "light.kitchen"}'

# Read one entity's state
curl https://<host>/api/states/cover.garage_door \
  -H "Authorization: Bearer $HA_TOKEN"

# Activate a scene
curl -X POST https://<host>/api/services/scene/turn_on \
  -H "Authorization: Bearer $HA_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"entity_id": "scene.movie_night"}'
```

## 4. Quick-win alternative — embedded Lovelace

If you'd rather not build custom widgets first, embed HA **Lovelace cards via iframe** and use
the **`kiosk-mode`** HACS plugin to hide HA's header/sidebar so it looks like a clean panel.
Good for a fast v1; swap to the WebSocket/REST contract above when you want full custom UI.

## 5. Widget contract (decoupled)

The dashboard project should depend only on this list — names finalized in the build phase:

| Widget | Entities (read) | Action (service call) |
|--------|-----------------|-----------------------|
| Lights | `light.*` per room | `light.turn_on` / `turn_off` / `toggle` |
| Garage | `cover.garage_door` | `cover.open_cover` / `close_cover` |
| Lock | `lock.front_door` | `lock.lock` / `unlock` |
| Climate | `climate.house` | `climate.set_temperature` |
| Water | `binary_sensor.*_leak` | (read-only alert) |
| Scenes | `scene.*` | `scene.turn_on` |
| Camera | `camera.driveway` | (stream URL / snapshot) |

Keeping this contract stable means your dashboard code and the HA config can evolve
independently.
