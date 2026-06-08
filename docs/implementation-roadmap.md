# Phased Implementation Roadmap

The build order, designed so each phase is independently verifiable before moving on. Nothing
here is purchased or wired until the plan is approved.

## 1. Hub bring-up
- Assemble Pi 5 + NVMe case on the **UPS**.
- Flash **HA OS**, boot **from NVMe**, set a **static IP**.
- Take the **first backup**.
- ✅ `ha core info` healthy; reboot survives and boots from NVMe.

## 2. Core + remote
- Onboard, install **HACS**.
- Start **Nabu Casa** trial; enable **remote access** + **cloud backup**.
- ✅ HA reachable remotely; cloud backup runs.

## 3. Local Wi-Fi devices
- Add **Kasa/Tapo** plugs/bulbs, **Dreame** vacuum (HACS + map), **Blink**, **Reolink** camera.
- ✅ Reolink live RTSP renders; Dreame map loads.

## 4. Radios & sensors
- Plug in **ZWA-2** + **ZBT-2** (on the USB extension).
- Join **Sengled bulbs** + **door/window contacts** (Zigbee).
- Join **water** + **smoke/CO** sensors (Z-Wave).
- Join/confirm the **Schlage lock** (model-dependent).
- ✅ Sensors report state; lock locks/unlocks from HA.

## 5. Climate & garage
- Integrate the **thermostat/HVAC**.
- Flash & wire the **ratgdo/Konnected** board; verify locally.
- ✅ Garage open/close from HA matches reality; climate tile works.

## 6. Automations & alerts
- **Water-leak** alert (notify + Alexa announce).
- **Smoke/CO** critical alert.
- **Away-mode** (vacuum + lights + HVAC setback).
- **Night routine** (lock + garage check).
- **Camera motion** alerts.
- Enable **Adaptive Lighting**; stand up **Alarmo** using contact/water/camera sensors.
- ✅ Leak test fires push + announce within seconds.

## 6b. Scenes, outdoor & media
- Build **Good Morning / Movie Night / Vacation / Good Night** scenes (one tap, also
  voice-triggerable).
- **Outdoor lighting** on sun-based schedules (porch/landscape on at sunset, off at
  sunrise/bedtime; holiday-light helper).
- Surface **TVs + speakers as media controls** in HA and on the Family Remote.
- **Vacation mode** randomizes lights for an occupied look.
- ✅ Each scene activates from a tile and by voice.

## 7. Family surfaces
- Build the **Family Remote** (Mushroom tiles + **Dreame tap-a-room map**); pin on iPads via
  Companion app + **Guided Access**.
- Configure the **HomeKit Bridge** for Apple Home.
- Set the curated **40" wall dashboard**.
- Expose only a **minimal entity set to Alexa** for voice.
- ✅ Non-technical user taps Garage / All Off / cleans two rooms; Apple Home toggles a light.

## 8. Dashboard API
- Create the **dashboard user + token**.
- Document the **widget contract** for the 40" TV project (see
  [dashboard-integration.md](dashboard-integration.md)).
- ✅ WebSocket `subscribe_entities` shows live updates; REST service call actuates a device.

## 9. Harden & hand off
- **UPS shutdown** integration; **watchdog** alerts; backups verified; **restore tested**.
- Review **docs + Family Remote walkthrough** with the family.
- ✅ Pull WAN → local control still works; pull hub power → UPS holds + on-battery logged.
