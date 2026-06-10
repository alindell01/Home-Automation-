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

## 4. School-bus "approaching" alert (FirstView → Alexa)

Announce on the Echos when the kids' bus is near the house. The district uses **FirstView**,
which has **built-in email alerts** — so HA never has to touch the bus GPS or intercept a phone
notification (the latter is impossible on iPhone). We let FirstView do the proximity math and
catch its email on the way out.

- **In FirstView** (Settings → Notifications):
  - **Manage Distance Notifications** → set an alert for when the bus is a chosen distance / N
    minutes from the stop.
  - **Manage Recipients** → add a **dedicated throwaway email address** (e.g., a Gmail made just
    for this) as a recipient.
- **In HA**: the built-in **IMAP integration** watches that inbox. When the "bus approaching"
  email arrives, an automation fires an **Alexa announcement** (+ optional phone push):
  ```yaml
  # sketch — finalize sender/subject match against a real FirstView email
  - id: bus_approaching
    alias: "School bus approaching"
    trigger:
      - platform: event
        event_type: imap_content
        event_data:
          sender: "alerts@firstviewapp.com"   # confirm exact sender
    condition:
      - condition: template
        value_template: "{{ 'approaching' in trigger.event.data['subject'] | lower }}"
    action:
      - service: notify.alexa_media
        data:
          target: !secret alexa_announce_target
          message: "The school bus is almost here."
          data:
            type: announce
  ```

**Why this works (and its limits):**
- ✅ Cross-platform — works fine with an all-iPhone family; no spare Android device needed.
- ✅ Reliable — no reverse-engineered/private API to break.
- ⚠️ **No live bus position on the dashboard** — FirstView has no public API or HA integration
  for the moving GPS dot, so this is an *alert*, not a map tile.
- ⚠️ Confirm the real sender address/subject line from an actual FirstView email before
  finalizing the trigger.

## Sequencing

These are independent and can be added in any order as budget/interest allows. None require
re-architecting the hub — they attach to the same HA brain, radios, and Areas defined in v1.
