# Network, Security & Resilience

## Network foundation (confirmed)

- The hub runs on **wired Ethernet** and the existing **mesh Wi-Fi covers the basement +
  driveway** — **no network upgrade is needed for v1.**
- Still place the ZWA-2/ZBT-2 sticks on the **USB-2 extension cable**, away from the Pi and any
  USB-3 ports, to avoid RF interference. Add Z-Wave/Zigbee repeaters only if range testing later
  shows a weak spot (most mains-powered Z-Wave/Zigbee devices act as repeaters automatically).

## Remote access

- **Nabu Casa only** — no port forwarding, no exposed ports, TLS handled for you. This is the
  single paid front door and also powers the Alexa skill and cloud backup.
- **Documented fallbacks** (not used by default): **Tailscale** (private mesh VPN) or
  **Cloudflare Tunnel** (free, no open ports). Nabu Casa is chosen because it also gives the
  family Alexa + the easiest remote experience.
- **Never** expose HA directly to the internet via router port-forwarding.

## Backups

- **Boot from NVMe**, not SD — SD cards wear out and fail.
- **Nightly local backup** + **Nabu Casa cloud backup**. Periodically copy one backup off-box
  (e.g., to a NAS or laptop). **Test a restore at least once** so you know it works.
- HA also takes an **automatic pre-update backup** before core updates.

## Users & least privilege

- **Separate HA users** per real person + dedicated **limited users** for machines:
  - A **dashboard user** with a **Long-Lived Access Token** scoped for the 40" display only
    (see [dashboard-integration.md](dashboard-integration.md)).
- **Secrets** live in `config/secrets.yaml` (**git-ignored**); commit `secrets.yaml.example`
  with placeholders instead. Tokens, Wi-Fi/RTSP passwords, and API keys never enter git.
- Rotate the dashboard token if a device is lost or repurposed.

## Optional hardening (documented, not required for v1)

- **IoT VLAN/SSID**: put cloud cameras and chatty IoT gear on a separate VLAN/SSID with limited
  routing to the main LAN. The HA hub bridges between segments. This contains a compromised
  cloud gadget. Skip for v1; revisit if the device count grows.

## Resilience

- **UPS** keeps the Pi + modem + router alive through power blips, so **leak alerts, the lock,
  and notifications keep working** during an outage. The UPS connects to HA (USB/NUT) to log
  on-battery events and trigger a **clean shutdown** on low battery.
- **Local-first by design**: lock, lights, garage, and water alerts all keep working **even if
  the internet drops** — only Alexa voice and remote access need the cloud.

## Out of scope for v1 (by the owner's call)

- **No video doorbell** and **no kid-specific access controls/tiles.** Both can be added later —
  HA users + Alarmo already support per-person restriction if that changes.
