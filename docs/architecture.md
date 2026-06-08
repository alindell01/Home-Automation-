# Architecture

## Context

One **local-first brain** — Home Assistant on a Raspberry Pi 5 — ties together devices spread
across several ecosystems while keeping day-to-day life easy for a family of four. Today
everything runs through Alexa with no unifying logic, no remote-access strategy, and no
basement leak protection. This design fixes that.

### Goals

- **Keep only Alexa's _voice_** (the Echos already in the house) — but move device *control* off
  the Alexa cloud into local HA, so Alexa is a convenience mic, not a dependency, and can be
  dropped later.
- **Unify** smart plugs/bulbs, a Schlage lock, Blink cameras, MyQ garage, and a Dreame vacuum.
- **Fold in** a thermostat/HVAC, door/window contact sensors, and smoke/CO monitoring.
- **Add** basement water-leak sensors (alerts only), a **driveway camera**, and a **UPS** for
  outage resilience.
- **Remote access from anywhere** for the whole family, ≤ $30/mo in subscriptions.
- A **super-simple iPad "Family Remote"** (lights, garage, scenes, media, and a tap-a-room
  **Dreame map**), plus widgets feeding a **40" wall-mounted family dashboard** (a *separate
  code project*).
- Use **Apple Home** (the family is mostly iPhone) as a secondary control surface.
- **Moderate maintenance** appetite; leave headroom for **local voice, room presence, and
  energy monitoring**.

### Key research findings (June 2026) that shaped this design

- **MyQ**: the official HA integration was removed (2023) and even cloud bridges were broken by
  the Dec-2025 Security+ 3.0 firmware. → Use a **local DIY board** (ratgdo/Konnected) wired to
  the opener.
- **Blink**: HA integration is **cloud-only** — snapshots + arm/disarm + motion events, **no
  live view**. Good for automation/alerts only.
- **Dreame**: excellent **local** control + maps via the `Tasshack/dreame-vacuum` HACS
  integration.
- **Alexa + remote access**: **Home Assistant Cloud (Nabu Casa) $6.50/mo** cleanly provides both
  (Alexa Smart Home skill, Google, remote access, TTS, cloud backup) — well under budget.
- **Coordinators (2026)**: Zigbee/Thread = **Connect ZBT-2 ($49)** (ZBT-1 discontinued); Z-Wave
  = **Connect ZWA-2 ($49)**. One USB stick is one radio — both are needed for "Z-Wave + Zigbee."
- **Apple Home**: HA's built-in **HomeKit Bridge** exposes HA entities to the Apple Home app.

## Architecture Overview

```
                        ┌─────────────────────────────────────────────┐
                        │     Raspberry Pi 5 (8GB) — Home Assistant OS  │
                        │                  (the "brain")                │
   Alexa Echos ◀──Nabu  │  • Automations engine + Lovelace dashboards   │
   (voice)      Casa──▶ │  • ZWA-2 (Z-Wave)  • ZBT-2 (Zigbee/Thread)    │
                        │  • HomeKit Bridge  • REST + WebSocket API     │
   Apple Home ◀─HomeKit │  • HACS custom integrations                   │
   (iPhones)    Bridge  └───┬───────────────┬───────────────┬───────────┘
                            │ local          │ local          │ cloud APIs
   Your 40" Dashboard ◀──REST/WS token──┐   │                │
   (separate project)                   │   │                │
        ┌───────────────────────────────┴───┴────────┐  ┌────┴───────────────┐
        │ LOCAL: Kasa plugs/bulbs, Sengled (Zigbee),  │  │ CLOUD: Blink cams,  │
        │ Schlage (if Z-Wave), water sensors, garage  │  │ Dreame (cloud opt), │
        │ board, Reolink driveway cam, ZWA-2/ZBT-2    │  │ Wyze/Govee via Alexa│
        └─────────────────────────────────────────────┘  └────────────────────┘
```

## Design Principles

1. **Local control wherever possible** — survives internet outages, faster, private. Cloud only
   where the vendor forces it (Blink, Wyze/Govee).
2. **Alexa is intentionally demoted to voice-only.** We actively pull device *control* off the
   Alexa cloud into local HA (Kasa local, Sengled onto Zigbee, etc.), so Alexa is a microphone,
   not the brain — and can be removed with no loss of function.
3. **Nabu Casa is the single paid "front door"** for voice + remote access. No port forwarding.
4. **Everything family-facing flows through HA surfaces**: a super-simple iPad "Family Remote,"
   the Apple Home app, the 40" dashboard, and Alexa voice for hands-free convenience.
5. **Boot from NVMe, not SD** — SD cards die. Nightly local + cloud backups.
6. **Least privilege** — separate HA users/tokens for the dashboard; secrets git-ignored.
7. **Plan for the future now** — entities named for voice, headroom for presence and energy
   monitoring, so growth is plug-in rather than re-architecture.

## Layers

| Layer | What lives here |
|-------|-----------------|
| **Hub** | Pi 5 + HA OS, on a UPS, booting from NVMe |
| **Radios** | ZWA-2 (Z-Wave) + ZBT-2 (Zigbee/Thread) on a USB-2 extension away from RF noise |
| **Local devices** | Kasa/Tapo, Sengled (Zigbee), Schlage (if Z-Wave), water/contact/smoke sensors, garage board, Reolink cam, Dreame |
| **Cloud devices** | Blink cameras, Wyze/Govee holdouts (phase out over time) |
| **Family surfaces** | Family Remote (iPad), Apple Home, 40" dashboard, Alexa voice |
| **Remote/voice** | Nabu Casa (Alexa skill + remote access + cloud backup) |

## Out of scope for v1 (by the owner's call)

- **No video doorbell.**
- **No kid-specific access controls/tiles.** The architecture can add either later — HA users
  and Alarmo already support per-person restriction if that changes.

See [integration-map.md](integration-map.md) for the per-device path, and
[implementation-roadmap.md](implementation-roadmap.md) for the build order.
