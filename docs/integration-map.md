# Device Integration Map

How each device connects, whether it's local or cloud, and the setup steps. The guiding rule:
**local wherever possible; cloud only where the vendor forces it.**

## Summary table

| Device | Path | Local/Cloud | Notes |
|--------|------|-------------|-------|
| TP-Link **Kasa/Tapo** plugs & bulbs | HA TP-Link integration | **Local** | Auto-discovered on LAN |
| **Sengled** bulbs | Join to **ZBT-2 (Zigbee)** | **Local** | Pull off Alexa onto HA's Zigbee radio |
| **Wyze / Govee** plugs/bulbs | Community HA integrations where available, else replace over time | Cloud | Last Alexa-dependent holdouts; plan to phase out |
| **Schlage** lock | ZWA-2 (if Z-Wave) *or* Alexa bridge (if Wi-Fi) | Local/Cloud | Model-dependent (see below) |
| **Blink** cameras | HA Blink integration | Cloud | Snapshots, arm/disarm, motion → automations/alerts only |
| **Reolink** driveway cam | HA Reolink + (optional) **Frigate** add-on | **Local** | RTSP live view; Frigate adds person/car detection |
| **MyQ** garage | **ratgdo/Konnected** board (ESPHome) | **Local** | Replaces dead MyQ cloud path |
| **Dreame** vacuum | `Tasshack/dreame-vacuum` (HACS) | **Local** (+map) | Room cleaning, away-triggers |
| **Water leak** sensors | ZWA-2 (Z-Wave) | **Local** | Instant alerts (see automations) |
| **Thermostat / HVAC** | Native integration (Ecobee/Honeywell/Nest) or Z-Wave if "dumb" | Local/Cloud | Schedules, away-setback, climate tiles |
| **Door/window** contacts | ZBT-2 (Zigbee) | **Local** | Doors-open alerts, security, lighting |
| **Smoke / CO** | Z-Wave (First Alert ZCOMBO) or monitor existing | **Local** | Critical push + Alexa announce |
| **Alexa Echos** | Nabu Casa Alexa Smart Home skill | Cloud relay | **Voice only** + announcements |

## Per-device setup

### TP-Link Kasa/Tapo (local)
1. Settings → Devices & Services → **Add Integration → TP-Link**.
2. Devices auto-discover on the LAN. If a Tapo device asks for cloud creds, supply them once;
   control then runs locally.
3. **Migration**: remove these from the Alexa app's device list so Alexa no longer controls
   them directly — Alexa reaches them through HA instead (see [voice-and-apple-home.md](voice-and-apple-home.md)).

### Sengled bulbs (local, Zigbee)
1. Factory-reset each bulb (power cycle per Sengled's pattern).
2. ZHA/Zigbee2MQTT (on **ZBT-2**) → **Add device** → join the bulb.
3. Rename for voice ("Living Room Lamp"), assign to an Area.
4. **Migration**: delete the Sengled skill/devices from Alexa once they're on Zigbee.

### Wyze / Govee (cloud holdouts)
- Use community HACS integrations if available and well-maintained; otherwise leave on Alexa
  for now and **replace with local-capable gear over time**. These are the last
  Alexa-dependent devices — track them so Alexa can eventually be dropped entirely.

### Schlage lock (decision branch)
- **Z-Wave Schlage Connect** → exclude from any old hub, then **Add device** on ZWA-2 with
  S2 secure pairing. Full local lock/unlock, codes, and status. **$0**.
- **Wi-Fi Schlage Encode** → limited/cloud HA support; keep on the Alexa bridge for now. A
  future **Encode Plus (Matter)** makes it fully local.
- **Confirm the exact model first** — it determines everything here.

### Blink cameras (cloud)
1. **Add Integration → Blink**, sign in, complete 2FA.
2. You get snapshots, motion events, and arm/disarm — **no live view** (vendor limitation).
3. Use motion events as **automation triggers** and alerts only.

### Reolink driveway cam (local)
1. Wire PoE (camera → injector → switch). Give it a static IP.
2. **Add Integration → Reolink**; live RTSP/streams render in HA.
3. *(Optional)* Install the **Frigate** add-on for local person/car detection on the driveway.

### MyQ garage → ratgdo/Konnected (local)
1. Flash the ratgdo/Konnected board with **ESPHome**.
2. Wire to the opener's terminals + door sensor per the board's guide.
3. **Add Integration → ESPHome**; you get a `cover` entity with real open/closed state.
4. This **replaces the dead MyQ cloud path** entirely.

### Dreame vacuum (local + map)
1. Install **HACS**, then add `Tasshack/dreame-vacuum`.
2. Authenticate (local where supported; cloud option for initial map pull).
3. Exposes room-level cleaning + the live map used by the Family Remote
   (see [family-remote.md](family-remote.md)).

### Water leak sensors (local, Z-Wave)
1. **Add device** on ZWA-2 (S2 secure) for each Zooz ZSE42.
2. Place at basement floor, laundry, and water-heater base (or sump probe variant).
3. Wired into the leak automation (see `config/automations/water_leak.yaml`).

### Thermostat / HVAC
- **Existing Ecobee/Honeywell/Nest** → native integration, **software-only**.
- **"Dumb" thermostat** → replace with a Z-Wave T6/Zooz and join ZWA-2.
- Drives away-setback and climate tiles.

### Door/window contacts (local, Zigbee)
- Join each to **ZBT-2**; name per door. Feed doors-open alerts, Alarmo, and lighting.

### Smoke / CO
- **First Alert ZCOMBO (Z-Wave)** joined to ZWA-2, or monitor an existing system if it exposes
  status. Drives the critical alert (see `config/automations/smoke_co.yaml`).

### Alexa Echos (voice only)
- Exposed through the **Nabu Casa Alexa Smart Home skill** with a **curated entity list** —
  see [voice-and-apple-home.md](voice-and-apple-home.md). Announcements via the **Alexa Media
  Player** HACS integration.
