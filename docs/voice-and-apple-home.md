# Voice (Alexa) & Apple Home

Two secondary control surfaces, both fed by Home Assistant. **Alexa is intentionally
minimized to voice-only.** Apple Home gives the iPhone-heavy family a native option.

## Alexa = voice only (deliberately minimized)

The Echos stay as **microphones and speakers**, not the brain. We expose a *small, curated*
set of HA entities to Alexa so spoken commands and announcements still work — but no control
logic runs through the Alexa cloud, and devices migrate onto HA's local radios so Alexa can be
dropped later with **no loss of function**.

### Setup
1. Start the **Nabu Casa** subscription (also powers remote access + cloud backup).
2. In HA: **Settings → Home Assistant Cloud → Alexa**.
3. Turn **off** "Expose all entities." Manually expose only a **curated list**, e.g.:
   - Front Door Lock, Garage Door
   - A few key lights / "All Off" helper
   - Scene activators (Good Night, Movie Night)
4. In the Alexa app: **"Alexa, discover devices."** Only the curated set should appear.
5. **Rename entities for natural speech** ("Front Door," not "lock.schlage_be469").

### Announcements (e.g., "Water detected in the basement")
- Install the **Alexa Media Player** HACS integration to push **TTS announcements** to the
  Echos (used by the leak and smoke/CO automations).
- Example service call used in automations:
  ```yaml
  - service: notify.alexa_media
    data:
      target: media_player.kitchen_echo
      message: "Water detected in the basement."
      data:
        type: announce
  ```

### Migration checklist (pull control off Alexa)
- [ ] Kasa/Tapo controlled via HA (removed from Alexa device list)
- [ ] Sengled bulbs moved to Zigbee (ZBT-2), removed from Alexa
- [ ] Only the curated entity list is exposed to Alexa
- [ ] Everyday control verified working **with Alexa powered off**
- [ ] Wyze/Govee holdouts tracked for eventual replacement

> Endgame: when local voice (Voice PE) is added later, Alexa can be unplugged entirely. See
> [future-expansion.md](future-expansion.md).

## Apple Home (HomeKit Bridge)

HA's built-in **HomeKit Bridge** publishes a curated set of entities to the **Apple Home app**,
so the iPhone users control lights/locks/garage natively and via **Siri**, with per-person
access through Apple's Home sharing.

### Setup
1. **Settings → Devices & Services → Add Integration → HomeKit Bridge**.
2. Choose **which domains/entities** to include (curated — lights, locks, garage, climate,
   scenes). Avoid exposing everything to keep the Home app tidy.
3. Scan the generated **HomeKit pairing QR code** with an iPhone (Home app → Add Accessory).
4. Assign entities to **Apple Home rooms**; invite family members via Apple Home sharing for
   per-person access.

### Notes
- The bridge is **local** — Apple Home control works on the LAN without Nabu Casa.
- A **Home Hub** (HomePod/Apple TV) enables remote Apple Home access + automations on Apple's
  side, independent of HA.
- Base config for the bridge lives in `config/configuration.yaml`.
