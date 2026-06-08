# Maintenance Plan

Tuned to a **moderate** maintenance appetite — keep it stable, keep it boring, find problems
before the family does.

## Stability-first HACS policy

- Install **only well-maintained, popular** integrations/cards:
  Mushroom, lovelace-xiaomi-vacuum-map-card, Adaptive Lighting, Alarmo, Frigate, Bermuda,
  Alexa Media Player, dreame-vacuum.
- **Before adding anything**: check recent GitHub activity (commits, open-issue health, stars).
- **Pin versions** in HACS; don't auto-update custom components.

## Update cadence (~monthly)

1. **Back up first** (HA takes an automatic pre-update backup — confirm it exists).
2. **Read breaking-change notes** for HA core **and each HACS item** before clicking update.
3. **Skip x.0 releases for ~a week** — let early bugs surface and get patched.
4. Update core, then add-ons, then HACS items — one layer at a time so a failure is easy to
   trace.

## Backups

- **Nightly local** + **Nabu Casa cloud** backup.
- Periodically **copy one backup off-box** (NAS/laptop).
- **Test a restore once** so you know recovery actually works — an untested backup is a guess.

## Watchdog ("system health")

`config/automations/system_health.yaml` alerts you when:

- The hub, a coordinator (ZWA-2/ZBT-2), or a key sensor goes **unavailable**.
- A battery-powered sensor drops **low battery**.
- The **UPS goes on-battery** (power outage).

The point: **you** find out before your family does.

## Documentation discipline

- Every device and automation is captured in `docs/` so future-you isn't reverse-engineering a
  black box.
- When you add/replace a device, update [integration-map.md](integration-map.md) and the
  relevant automation file in the same change.

## Quick monthly checklist

- [ ] Pre-update backup exists
- [ ] Read core + HACS breaking-change notes
- [ ] Update core → add-ons → HACS (one layer at a time)
- [ ] Confirm all coordinators + key sensors online after reboot
- [ ] Glance at battery levels; replace anything low
- [ ] Once a quarter: copy a backup off-box and test a restore
