# Future-Expansion Headroom

The Pi 5 (8GB) + NVMe has ample headroom for all three stated future goals. We wire the
**foundations now** so adding them later is plug-in, not re-architecture.

## 1. Drop Alexa entirely → local voice

- **Now**: keep **Assist** configured and **name every entity for voice** from day one
  (e.g., "Living Room Lamp," "Front Door"). Expose only a curated set to Alexa.
- **Later**: add **Home Assistant Voice PE ($70)** pucks per room. They use the "Okay Nabu"
  wake word and run the same Assist pipeline.
- **STT/TTS**: cloud STT/TTS via Nabu Casa works on the Pi 5 today; fully-local **Whisper +
  Piper** is also feasible on the Pi 5 for total cloud independence.
- **Result**: voice migrates off Alexa with no change to automations — the Echos can be unplugged.

## 2. Room-level presence

- Standardize on **ESPHome Bluetooth-Proxy** ESP32 nodes (~$8 each) + the **Bermuda** HACS
  integration.
- The ZBT-2 plus a couple of ESP32 proxies give whole-house **BLE presence** for "lights follow
  people" and per-person scenes — **no extra subscription**.
- Foundation now: keep Areas/rooms well-defined so presence maps cleanly onto them.

## 3. Energy monitoring

- Leave a **panel slot / plan** for an **Emporia Vue 2** (whole-panel) or **Shelly EM**.
- HA's built-in **Energy Dashboard** then visualizes whole-home + per-circuit usage.
- **Power-metering smart plugs** (some Kasa models) already cover individual high-draw loads in
  the meantime.

## Sequencing

These are independent and can be added in any order as budget/interest allows. None require
re-architecting the hub — they attach to the same HA brain, radios, and Areas defined in v1.
