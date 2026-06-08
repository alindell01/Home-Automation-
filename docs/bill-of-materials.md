# Bill of Materials

All prices approximate (USD, June 2026). Links are vendor/category starting points — confirm
current pricing and availability at purchase time.

## Core build (~$535)

| # | Item | Purpose | ~USD |
|---|------|---------|------|
| 1 | [Raspberry Pi 5, 8GB](https://www.raspberrypi.com/products/raspberry-pi-5/) | The hub | 80 |
| 2 | [Argon NEO 5 M.2 NVMe case](https://argon40.com/) (case + cooling + NVMe slot) | Reliable, fanless-ish, SSD boot | 45 |
| 3 | 256GB NVMe SSD | Fast, durable storage (SD cards die) | 30 |
| 4 | [Official 27W USB-C PSU](https://www.raspberrypi.com/products/27w-power-supply/) | Stable power (Pi 5 is picky) | 14 |
| 5 | [Connect ZWA-2](https://www.home-assistant.io/connectzwa-2/) (Z-Wave 800) | Lock + water + secure sensors | 49 |
| 6 | [Connect ZBT-2](https://www.home-assistant.io/connectzbt-2/) (Zigbee/Thread) | Cheap sensors, Sengled bulbs, Matter future | 49 |
| 7 | USB 2.0 extension cable | Move sticks away from Pi/USB3 RF noise | 8 |
| 8 | 3× [Zooz ZSE42](https://www.getzooz.com/) 800LR water leak sensor (Z-Wave) | Basement / laundry / water-heater | 75 |
| 9 | [Reolink RLC-810A](https://reolink.com/) PoE camera + PoE injector | Driveway, local RTSP → HA/Frigate | 72 |
| 10 | [ratgdo](https://paulwieland.github.io/ratgdo/) / Konnected GDO blaQ board | **Local** garage control | 45 |
| 11 | UPS ~600VA (APC/CyberPower) for Pi + modem + router | Leak alerts/lock survive power blips | 68 |
| | **Core subtotal** | | **~$535** |

This is ~$35 over the original $500 target because the **driveway camera** and **UPS** were
added. Levers to land at ≤ $500 if a hard cap is wanted:

- Drop the USB extension (−$8)
- Use 2 water sensors instead of 3 (−$25)
- Use a Pi 5 fan case + separate NVMe HAT instead of the Argon (−~$15)

## Phase 2 add-ons (fold in as budget allows — not required for v1)

| Item | Purpose | ~USD |
|------|---------|------|
| 3–5× Zigbee door/window contact sensors (Aqara/Sonoff) | Doors-open, security, automations | ~$12 ea |
| Z-Wave smoke/CO ([First Alert ZCOMBO](https://www.firstalert.com/)) ×1–2 | HA-monitored smoke/CO alerts | ~$40 ea |
| Local thermostat: integrate existing, **or** Z-Wave T6/Zooz if "dumb" | HVAC schedules/automation | $0–80 |
| ESP32 Bluetooth-proxy nodes ×2–4 | Room-level **presence** (Bermuda) later | ~$8 ea |
| [Home Assistant Voice PE](https://www.home-assistant.io/voice-pe/) ×1–2 | Local voice to **replace Alexa** | $70 ea |
| Energy: Emporia Vue 2 (whole-panel) or Shelly EM | Per-circuit **energy monitoring** | $50–120 |

## Subscription

| Service | Cost | Notes |
|---------|------|-------|
| Home Assistant Cloud (Nabu Casa) | **$6.50/mo** (~$78/yr) | Alexa skill + remote access + cloud TTS + cloud backup |

That leaves **~$23/mo of headroom** under the $30/mo budget — no other subscriptions required.

## Notes / contingencies

- **Sump pit**: optionally swap one ZSE42 for an **Aeotec Water Sensor 7 Pro w/ probe (~$45)** to
  sense the pit floor without the whole unit getting wet.
- **Schlage lock — decision branch**:
  - **Z-Wave Schlage Connect** → joins ZWA-2 for full local control at **$0**.
  - **Wi-Fi Schlage Encode** → HA support is limited/cloud; keep it on the Alexa bridge for now.
    A future **Schlage Encode Plus (Matter, ~$300)** would make it fully local.
  - *Confirm the exact model in the build phase.*
- **Thermostat**: an existing Ecobee/Honeywell/Nest integrates **software-only ($0)**; only a
  "dumb" thermostat needs new hardware. *Confirm brand in build phase.*
- **PoE**: if no Ethernet reaches the driveway, substitute a **Reolink Wi-Fi** model (same HA
  integration).

## Budget summary

- **Core hardware**: ~$535 (or ≤ $500 with the trims above)
- **Subscription**: $6.50/mo ≤ $30/mo ✅
