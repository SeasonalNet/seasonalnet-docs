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
| SeasonalCME Voice | `10.10.0.0/24`, `fd97:418:22dd:10::/64` (VLAN 10) | `LANs/seasonalcme-network-map.md` |
| NET-MGMT | `10.40.0.0/24`, `fd97:418:22dd:40::/64` (VLAN 40) | `LANs/management-vlans-map.md` |
| BMC-MGMT | `10.41.0.0/24`, `fd97:418:22dd:41::/64` (VLAN 41) | `LANs/management-vlans-map.md` |
| CME-MGMT | `10.42.0.0/24`, `fd97:418:22dd:42::/64` (VLAN 42) | `LANs/management-vlans-map.md` |
| HOST-MGMT | `10.43.0.0/24`, `fd97:418:22dd:43::/64` (VLAN 43) | `LANs/management-vlans-map.md` |
| Phone-LAN | `10.30.0.0/24`, `fd97:418:22dd:30::/64` (VLAN 30) | `LANs/phonelan-map.md` |
| SeasonalVPN | `10.88.0.0/24`, `fd97:418:22dd:88::/64` | `LANs/seasonalvpn-network-map.md` |

All routed internal IPv6 ULAs are allocated from the SeasonalNet site prefix `fd97:418:22dd::/48`. The fourth hextet is a human-readable segment identifier matching the printed VLAN/segment number where practical. Each LAN receives a `/64`; ordinary LANs must not be assigned longer or shorter host prefixes without a documented exception.

SeasonalNet-DSW1 is the Layer-3 distribution core for VLANs 10, 30, 40, 41, 42, and 43. SeasonalNet-ASW1 is the primary PoE+ access/aggregation switch and reaches DSW1 over a two-member LACP `Po1`; SeasonalNet-SW3 retains its direct DSW1 fiber uplink plus a redundant trunk to ASW1. FW4C (`192.168.1.5`) is the production edge firewall/router reached over the `10.255.255.0/30` / `fd97:418:22dd:255::/64` transit. SeasonalVPN is hosted by the SeasonalVPN VM (`192.168.1.22`). SeasonalPBX remains a PBX service endpoint and does not route Phone-LAN.

---

## Key Infrastructure Addresses

| Address | Device | Role |
|---|---|---|
| `192.168.1.1` | Verizon WNC-CR200A | Internet gateway and Main-LAN router |
| `192.168.1.2` – `192.168.1.4` | Reserved | Former bridge/EDGE infrastructure assignments |
| `192.168.1.5` | FW4C | Production OpenWrt edge firewall/router |
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
| `192.168.1.32` | Free | Former SeasonalR230-iDRAC Main-LAN address; BMC moved to `10.41.0.32` |
| `192.168.1.33` | Free | Former SeasonalR340-iDRAC Main-LAN address; BMC moved to `10.41.0.33` |
| `192.168.1.40` | seasonal-victus | Ubuntu desktop/admin workstation |
| `192.168.1.41` | seasonal-teal-laptop | Ubuntu admin/build laptop |
| `192.168.1.52` | SeasonalBridge-ROOT | Legacy Aironet root bridge |
| `192.168.1.53` | SeasonalBridge-NONROOT | Legacy Aironet non-root bridge; quarantined as a production path |
| `192.168.1.54` | SeasonalNet-NP-R1 | Cisco 1921 non-production lab router |
| `10.10.0.1` / `10.30.0.1` / `10.40.0.1` / `10.41.0.1` / `10.42.0.1` / `10.43.0.1` | SeasonalNet-DSW1 | VLAN 10/30/40/41/42/43 IPv4 gateways and Layer-3 distribution core |
| `fd97:418:22dd:10::1` / `fd97:418:22dd:30::1` / `fd97:418:22dd:40::1` / `fd97:418:22dd:41::1` / `fd97:418:22dd:42::1` / `fd97:418:22dd:43::1` | SeasonalNet-DSW1 | Corresponding routed ULA gateways under `fd97:418:22dd::/48` |
| `10.255.255.1` / `fd97:418:22dd:255::1` | SeasonalNet-DSW1 | Routed transit endpoint toward FW4C |
| `10.255.255.2` / `fd97:418:22dd:255::2` | FW4C | Routed transit endpoint toward DSW1 |
| `10.10.0.10` / `fd97:418:22dd:10::10` | SeasonalCME-R1 | Cisco CME call-control node |
| `10.40.0.0/24` / `fd97:418:22dd:40::/64` | NET-MGMT | Active general network-device management network |
| `10.40.0.2` / `fd97:418:22dd:40::2` | SeasonalNet-ASW1 | Access-switch management on NET-MGMT |
| `10.40.0.3` / `fd97:418:22dd:40::3` | SeasonalNet-SW3 | Compact access-switch management on NET-MGMT |
| `10.40.0.4` / `fd97:418:22dd:40::4` | SeasonalAP-3602i | Autonomous AP management; service WLAN remains on VLAN 10 |
| `10.40.0.5` / `fd97:418:22dd:40::5` | SeasonalAP-2702E | Autonomous AP management; service WLAN remains on VLAN 30 |
| `10.41.0.32` | SeasonalR230-iDRAC | R230 out-of-band management on BMC-MGMT/VLAN 41 |
| `10.41.0.33` | SeasonalR340-iDRAC | R340 out-of-band management on BMC-MGMT/VLAN 41 |
| `10.42.0.2` / `fd97:418:22dd:42::2` | SeasonalCME-R1 | CME-specific management on `Gi0/0.42` |
| `10.43.0.0/24` / `fd97:418:22dd:43::/64` | HOST-MGMT | Routed/reserved host-management segment; no host cutover yet |

---

## Documentation Conventions

- Each LAN map includes address bands, infrastructure devices, routing/services boundaries, a topology summary, and known remaining items.
- Static addresses are configured on-device. DHCP pools are documented by range rather than as stable client identity.
- Main LAN uses native ISP-assigned IPv6 from the CR200A; do not assign the SeasonalNet ULA to Main LAN.
- The authoritative ULA root is `fd97:418:22dd::/48`. Assigned `/64`s currently use segment IDs `10`, `30`, `40`, `41`, `42`, `43`, `88`, and `255`.
- The old `fd42:*` prefixes are retired or transitional legacy space and must not be used for new assignments.
- OSPFv2 and OSPFv3 run between DSW1 and FW4C over the routed transit and between DSW1 and SeasonalCME-R1 over Voice VLAN 10. Process IDs are locally significant.
- CR200A primary static IPv4 routes use metric `10`; VLANs 10, 30, 40, 41, 42, and 43 return through FW4C at `192.168.1.5`, while SeasonalVPN returns through `192.168.1.22`. Future alternate or backup routes use metric `50` or higher.
- Phone-LAN no longer uses SeasonalPBX as its router. SeasonalNet-DSW1 owns `10.30.0.1` / `fd97:418:22dd:30::1` on `Vlan30`.

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
