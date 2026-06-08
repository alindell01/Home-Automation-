# Home Automation — Whole-Home Architecture

A **local-first** smart-home built on **Home Assistant** (Raspberry Pi 5), unifying devices
spread across Alexa, TP-Link/Kasa, Sengled, Schlage, Blink, MyQ, Dreame and more into one
brain — while keeping day-to-day control dead simple for a family of four.

**Design goals:** local control that survives internet outages, Alexa demoted to voice-only,
remote access for the whole family for ≤ $30/mo, basement leak protection, a super-simple iPad
"Family Remote," and clean integration points for a separate 40" wall-dashboard project.

## Start here

- [`docs/architecture.md`](docs/architecture.md) — the full design, diagram, and principles
- [`docs/implementation-roadmap.md`](docs/implementation-roadmap.md) — the phased build checklist
- [`docs/bill-of-materials.md`](docs/bill-of-materials.md) — what to buy, with budget tally

## Documentation

| Doc | What it covers |
|-----|----------------|
| [architecture.md](docs/architecture.md) | System design, data flow, principles |
| [bill-of-materials.md](docs/bill-of-materials.md) | Hardware list, links, budget |
| [integration-map.md](docs/integration-map.md) | Per-device setup (local vs cloud) |
| [network-security.md](docs/network-security.md) | Remote access, backups, users, VLAN |
| [voice-and-apple-home.md](docs/voice-and-apple-home.md) | Alexa (Nabu Casa) + HomeKit Bridge |
| [dashboard-integration.md](docs/dashboard-integration.md) | Token + WebSocket/REST contract for the 40" dashboard |
| [family-remote.md](docs/family-remote.md) | The simple iPad remote + Dreame map + kiosk lockdown |
| [future-expansion.md](docs/future-expansion.md) | Local voice, room presence, energy monitoring |
| [maintenance.md](docs/maintenance.md) | Update cadence, backups, HACS policy, watchdog |

## Config

The [`config/`](config) directory holds Home Assistant YAML — base config, automations,
scenes, scripts, and the Family Remote dashboard. Copy `config/secrets.yaml.example` to
`config/secrets.yaml` and fill in real values; the real `secrets.yaml` is git-ignored.

> Status: **architecture + config templates.** No hardware is purchased and nothing is wired
> until the plan is approved and the build phase begins.
