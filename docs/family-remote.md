# The "Family Remote" (super-simple iPad app)

A purpose-built, **zero-learning-curve** control surface for the whole family — especially a
non-technical spouse. It runs on the repurposed iPads, behaves like a single-purpose appliance
(not "Home Assistant"), and inherits Apple Home + Siri for free because it's the same backend.

**$0 hardware** — it reuses iPads you already have.

## What's on it

Big, rounded, idiot-proof tiles built with **Mushroom + Bubble Card + card-mod**:

- **Lights** — per-room toggles + a prominent **"All Off."**
- **Garage** — one button with bold open/closed color state.
- **Scenes** — one-tap **Movie Night** / **Good Night** buttons (also voice-triggerable).
- **Media** — a simple play-pause + volume tile for the main TV/speakers.
- **Dreame map mini-app** — see below.

The full Lovelace definition is in
[`config/dashboards/family_remote.yaml`](../config/dashboards/family_remote.yaml).

## Dreame tap-a-room map mini-app

The star feature for hands-off vacuuming:

- Uses [`lovelace-xiaomi-vacuum-map-card`](https://github.com/PiotrMachowski/lovelace-xiaomi-vacuum-map-card),
  which renders the vacuum's **real house map**.
- Your spouse just **taps the rooms** to clean, toggles a **Clean ↔ Mop** switch, and presses
  one **Start** button — plus **Dock** and **Pause**.
- **No room IDs, no zones, no setup** surfaced to her. It looks like a tiny remote, not a config
  screen.
- *(Alternative: the newer React-based "Dreame Vacuum Map Card" if we want an even slicker look.)*

## Kiosk lockdown (so it can't be navigated away from)

1. Install the **Home Assistant Companion app** on the iPad and log in as a normal family user.
2. Open the **Family Remote** dashboard.
3. Enable **Guided Access / Single App Mode** (iOS: Settings → Accessibility → Guided Access)
   so the iPad is pinned to this one screen — no way out without the passcode.
4. *(Optional)* Use the **`kiosk-mode`** HACS plugin to hide HA's header/sidebar for an even
   cleaner, appliance-like look.

## Hardware placement

- One iPad **wall-mounted**, one **handheld**.
- A **charging dock + keep-awake** setting keeps the wall unit always live.

## Why this design

- **Same backend** as everything else → it inherits Apple Home + Siri, and we can add tiles
  later **without new code**.
- **No new hardware** and **no app development** — it's all HA dashboards + iOS Guided Access.
- Family-proof: the only things visible are the things they need, sized for a glance and a tap.

## Verification

On an iPad in Guided Access, a non-technical user should be able to:

- [ ] Tap **"Garage"** and **"All Off"** and see them work.
- [ ] **Tap two rooms on the Dreame map, flip Mop, press Start** → the vacuum cleans exactly
      those rooms.
- [ ] Tap a **scene** (Movie Night) and see lights/media respond.
- [ ] **Not** be able to navigate out of the simple dashboard.
