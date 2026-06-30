# AGENTS.md — SeasonalNet Documentation

Machine/agent reference for working with this documentation set.

---

## What This Is

Static markdown documentation for SeasonalNet, a self-managed homelab operating as a structured private infrastructure. This is not a software project. Files are reference documentation — markdown only, with no build system in this repository.

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
| `POLICIES.md` | Platform policies section root | Yes |
| `Policies/` | Privacy Notice, Terms of Service, and Acceptable Use Policy | Yes |
| `LANs/` | Per-segment network maps | Yes |
| `VMs/` | Per-VM host config and credentials | **NO — never commit, never publish** |

---

## Network Segments (Quick Reference)

| Segment | Subnet(s) | Map file |
|---|---|---|
| Main LAN | `192.168.1.0/24`; native ISP IPv6 | `LANs/seasonalnet-lan-map.md` |
| SeasonalCME Voice | `10.42.0.0/24`, `fd97:418:22dd:10::/64` (VLAN 10) | `LANs/seasonalcme-network-map.md` |
| SeasonalCME Management | `10.42.2.0/24`, `fd97:418:22dd:42::/64` (VLAN 42) | `LANs/seasonalcme-network-map.md` |
| Phone-LAN | `10.30.0.0/24`, `fd97:418:22dd:30::/64` (planned VLAN/segment 30) | `LANs/phonelan-map.md` |
| SeasonalVPN | `10.88.0.0/24`, `fd97:418:22dd:88::/64` | `LANs/seasonalvpn-network-map.md` |

All routed internal IPv6 ULAs are allocated from the SeasonalNet site prefix `fd97:418:22dd::/48`. The fourth hextet is a human-readable segment identifier matching the printed VLAN/segment number where practical. Each LAN receives a `/64`; ordinary LANs must not be assigned longer or shorter host prefixes without a documented exception.

SeasonalCME segments are behind SeasonalEDGE-R1 (`192.168.1.4`). SeasonalVPN is hosted by the SeasonalVPN VM (`192.168.1.22`). Phone-LAN no longer uses SeasonalPBX as its router, NAT boundary, DHCP server, or RA source.

---

## Key Infrastructure Addresses

| Address | Device | Role |
|---|---|---|
| `192.168.1.1` | Verizon WNC-CR200A | Internet gateway and Main-LAN router |
| `192.168.1.4` | SeasonalEDGE-R1 | SeasonalCME lab edge router |
| `192.168.1.6` | SeasonalAuth | Authentik / identity service |
| `192.168.1.7` | SeasonalForge | Forgejo / source control |
| `192.168.1.8` | SeasonalWeb / SeasonalNet | NGINX reverse proxy / Next.js SPAs / docs site |
| `192.168.1.9` | SeasonalRain | Discord bot/service VM |
| `192.168.1.10` | SeasonalWX | SeasonalWeather VM / weather radio pipeline and API |
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
| `192.168.1.22` | SeasonalVPN | Defguard/WireGuard VPN and routed VPN-LAN gateway |
| `192.168.1.30` | SeasonalR230 | Dell PowerEdge R230 / primary Proxmox VM host |
| `192.168.1.31` | SeasonalR340 | Dell PowerEdge R340 / secondary Proxmox VM host |
| `192.168.1.32` | SeasonalR230-iDRAC | Dell PowerEdge R230 out-of-band management |
| `192.168.1.33` | SeasonalR340-iDRAC | Dell PowerEdge R340 out-of-band management |
| `192.168.1.40` | seasonal-victus | Ubuntu desktop/admin workstation |
| `192.168.1.41` | seasonal-teal-laptop | Ubuntu admin/build laptop |
| `10.42.0.10` / `fd97:418:22dd:10::10` | SeasonalCME-R1 | Cisco CME call-control node |

---

## Documentation Conventions

- Each LAN map includes address bands, infrastructure devices, routing/services boundaries, a topology summary, and known remaining items.
- Static addresses are configured on-device. DHCP pools are documented by range rather than as stable client identity.
- Main LAN uses native ISP-assigned IPv6 from the CR200A; do not assign the SeasonalNet ULA to Main LAN.
- The authoritative ULA root is `fd97:418:22dd::/48`. Assigned `/64`s currently use segment IDs `10`, `30`, `42`, and `88`.
- The old `fd42:*` prefixes are retired or transitional legacy space and must not be used for new assignments.
- SeasonalCME uses OSPFv2 process 42 for IPv4 and OSPFv3 process 42 for IPv6 between SeasonalEDGE-R1 and SeasonalCME-R1.
- CR200A primary static routes use metric `10`; future alternate or backup routes use metric `50` or higher.
- Phone-LAN no longer uses SeasonalPBX as its router. The `10.30.0.1` / `fd97:418:22dd:30::1` reservation belongs to the future Phone-LAN routing boundary.

---

## Editing Rules

- Update the `Last updated` date in any LAN map you modify.
- Do not modify `VMs/` content through any automated process.
- If adding a new segment, create a new map file in `LANs/` following the existing structure and add an entry to `NETWORK.md` and `README.md`.
- Keep platform policies under `Policies/`, with `POLICIES.md` as the public section index. Community or communication-platform rules do not belong in these platform policy documents.
- Policy changes must update the effective or last-updated date as appropriate and remain consistent across the Privacy Notice, Terms of Service, and Acceptable Use Policy.
- Do not commit secrets, credentials, private keys, tokens, or per-VM sensitive configuration under any path.
- Do not include information about client device MACs or IPs, this is sensitive information. Vice versa on server/switch/router, etc. MACs.
- Do not include clauses such as "The LAN survived the renumbering reshuffle." that may be epheremal.

---

## Published Docs Site

This documentation is published at `docs.seasonalnet.org` through the SeasonalWeb / SeasonalNet Next.js docs deployment. Design and publication requirements:

- Shared header and footer consistent with all other SeasonalNet SPAs
- Black-and-white theming
- Source content from this documentation set
- Broad sections such as About, Topology, Projects, and Policies rather than treating topology as the whole site
- `VMs/` excluded from all published and version-controlled source
