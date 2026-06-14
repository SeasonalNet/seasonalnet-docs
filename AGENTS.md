# AGENTS.md — SeasonalNet Documentation

Machine/agent reference for working with this documentation set.

---

## What This Is

Static markdown documentation for SeasonalNet, a self-managed homelab operating as a structured private infrastructure. This is not a software project. Files are reference documentation — markdown only, no code, no build system (currently).

---

## Directory Layout

| Path | Contents | Commit-safe |
|---|---|---|
| `README.md` | Human-facing overview | Yes |
| `AGENTS.md` | This file | Yes |
| `PLATFORM.md` | Public-safe platform overview and boundaries | Yes |
| `SERVICES.md` | Public-safe service catalog and subsystem reference | Yes |
| `NETWORK.md` | Top-level topology and network segment index | Yes |
| `PROJECTS.md` | Projects/guides section root for future public FOSS documentation | Yes |
| `LANs/` | Per-segment network maps | Yes |
| `VMs/` | Per-VM host config and credentials | **NO — never commit, never publish** |

---

## Network Segments (Quick Reference)

| Segment | Subnet(s) | Map file |
|---|---|---|
| Main LAN | `192.168.1.0/24` | `LANs/seasonalnet-lan-map.md` |
| SeasonalCME Voice | `10.42.0.0/24` (VLAN 10) | `LANs/seasonalcme-network-map.md` |
| SeasonalCME Management | `10.42.2.0/24` (VLAN 42) | `LANs/seasonalcme-network-map.md` |
| Phone-LAN | `10.30.0.0/24`, `fd42:30::/64` | `LANs/phonelan-map.md` |

All segments are reachable from `192.168.1.0/24` when routing policy permits. SeasonalCME segments are behind `SeasonalEDGE-R1` (`192.168.1.4`). Phone-LAN has moved to `10.30.0.0/24` / `fd42:30::/64`; SeasonalPBX (`192.168.1.12`) is now a PBX service VM only and must not be documented as the Phone-LAN router, NAT boundary, DHCP server, or RA source.

---

## Key Infrastructure Addresses

| Address | Device | Role |
|---|---|---|
| `192.168.1.1` | Verizon WNC-CR200A | Internet gateway |
| `192.168.1.4` | SeasonalEDGE-R1 | SeasonalCME lab edge router |
| `192.168.1.6` | SeasonalAuth | Authentik / identity service |
| `192.168.1.7` | SeasonalForge | Forgejo / source control |
| `192.168.1.8` | SeasonalWeb / SeasonalNet | NGINX reverse proxy / Next.js SPAs / docs site |
| `192.168.1.9` | SeasonalRain | Discord bot/service VM |
| `192.168.1.10` | SeasonalWX | Current SeasonalWeather VM / weather radio pipeline and API |
| `192.168.1.12` | SeasonalPBX | SIP PBX / Asterisk service VM; not Phone-LAN router |
| `192.168.1.13` | SeasonalMail | mailcow mail server (`mail.seasonalnet.org`) |
| `192.168.1.14` | SeasonalDNS | Internal Technitium DNS resolver / local DNS management |
| `192.168.1.15` | SeasonalMonitor | Prometheus/Grafana observability VM |
| `192.168.1.16` | SeasonalBack | Proxmox Backup Server VM hosted on SeasonalR340 |
| `192.168.1.17` | SeasonalNAS | NAS / shared storage VM hosted on SeasonalR340 |
| `192.168.1.18` | SeasonalSand | Sandbox execution service VM hosted on SeasonalR340 |
| `192.168.1.19` | SeasonalDB | PostgreSQL database host for control-plane state |
| `192.168.1.20` | SeasonalDock | Docker/control-plane application host; currently hosts `pbx-controld` |
| `192.168.1.21` | SeasonalCA | LAN private CA / Smallstep step-ca host |
| `192.168.1.30` | SeasonalR230 | Dell PowerEdge R230 / primary Proxmox VM host on Main LAN via CME rack VLAN 99 transit path |
| `192.168.1.31` | SeasonalR340 | Dell PowerEdge R340 / Proxmox host on Main LAN via CME rack VLAN 99 transit path |
| `192.168.1.32` | SeasonalR230-iDRAC | Dell PowerEdge R230 out-of-band management |
| `192.168.1.33` | SeasonalR340-iDRAC | Dell PowerEdge R340 out-of-band management |
| `192.168.1.40` | seasonal-victus | Ubuntu desktop/admin workstation |
| `192.168.1.41` | seasonal-teal-laptop | Ubuntu admin/build laptop |
| `10.42.0.10` | SeasonalCME-R1 | Cisco CME call control node |

## Documentation Conventions

- Each LAN map includes: address bands, infrastructure device table, routing/services boundary, topology ASCII summary, and a "Known Remaining Items" section for deferred work.
- Static addresses are configured on-device (VMs: `/etc/network/interfaces`; Cisco gear: in running-config). DHCP pools are documented by range only.
- IPv6 ULA is deployed on Phone-LAN (`fd42:30::/64`) and SeasonalCME (`FD42:42::/64`, `FD42:42:2::/64`). Main LAN IPv6 is native ISP-assigned.
- Phone-LAN no longer uses SeasonalPBX as its router. The `10.30.0.1` / `fd42:30::1` gateway reservation belongs to the Phone-LAN routing boundary and its owner is pending managed-switch/trunk documentation.

---

## Editing Rules

- Update the `Last updated` date in any LAN map you modify.
- Do not modify `VMs/` content through any automated process.
- If adding a new segment, create a new map file in `LANs/` following the existing structure, and add an entry to `NETWORK.md`.
- Do not commit secrets, credentials, or per-VM sensitive configuration under any path.

---

## Published Docs Site

This documentation is published at `docs.seasonalnet.org` through the SeasonalWeb / SeasonalNet Next.js docs deployment. Design and publication requirements:

- Shared header and footer consistent with all other SeasonalNet SPAs
- Black and white theming (no color accent palette outside of monochrome)
- Source content from this documentation set
- The rendered docs site should expose broad sections such as About, Topology, and Projects rather than treating topology as the whole site
- `VMs/` must be excluded from any published or version-controlled source
