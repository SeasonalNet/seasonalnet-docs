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
| `NETWORK.md` | Top-level network segment index | Yes |
| `LANs/` | Per-segment network maps | Yes |
| `VMs/` | Per-VM host config and credentials | **NO — never commit, never publish** |

---

## Network Segments (Quick Reference)

| Segment | Subnet(s) | Map file |
|---|---|---|
| Main LAN | `192.168.1.0/24` | `LANs/seasonalnet-lan-map.md` |
| SeasonalCME Voice | `10.42.0.0/24` (VLAN 10) | `LANs/seasonalcme-network-map.md` |
| SeasonalCME Management | `10.42.2.0/24` (VLAN 42) | `LANs/seasonalcme-network-map.md` |
| Phone-LAN | `192.168.56.0/24` | `LANs/phonelan-map.md` |

All segments are reachable from `192.168.1.0/24`. SeasonalCME segments are behind `SeasonalEDGE-R1` (`192.168.1.4`). Phone-LAN is routed by `SeasonalPBX` (`192.168.1.12`, Phone-LAN gateway `192.168.56.1`).

---

## Key Infrastructure Addresses

| Address | Device | Role |
|---|---|---|
| `192.168.1.1` | Verizon WNC-CR200A | Internet gateway |
| `192.168.1.4` | SeasonalEDGE-R1 | SeasonalCME lab edge router |
| `192.168.1.8` | SeasonalWeb | NGINX reverse proxy / Next.js SPAs |
| `192.168.1.12` | SeasonalPBX | SIP PBX; Phone-LAN gateway (`192.168.56.1`) |
| `192.168.1.20` | seasonal-victus | VM hypervisor (VirtualBox); this machine |
| `192.168.1.21` | SeasonalR230 | Dell PowerEdge R230 / Proxmox host on Main LAN via CME rack VLAN 99 transit path |
| `192.168.1.22` | SeasonalR340 | Dell PowerEdge R340 / onsite backup storage host on Main LAN via CME rack VLAN 99 transit path |
| `192.168.1.23` | SeasonalR340-iDRAC | Dell PowerEdge R340 out-of-band management on Main LAN via CME rack VLAN 99 transit path |
| `10.42.0.10` | SeasonalCME-R1 | Cisco CME call control node |

---

## Documentation Conventions

- Each LAN map includes: address bands, infrastructure device table, routing/services boundary, topology ASCII summary, and a "Known Remaining Items" section for deferred work.
- Static addresses are configured on-device (VMs: `/etc/network/interfaces`; Cisco gear: in running-config). DHCP pools are documented by range only.
- IPv6 ULA is deployed on Phone-LAN (`FD42:56::/64`) and SeasonalCME (`FD42:42::/64`, `FD42:42:2::/64`). Main LAN IPv6 is native ISP-assigned.
- Phone-LAN now uses the conventional `.1` gateway on SeasonalPBX; older FreePBX17-era references to `.10` are retired/stale.

---

## Editing Rules

- Update the `Last updated` date in any LAN map you modify.
- Do not modify `VMs/` content through any automated process.
- If adding a new segment, create a new map file in `LANs/` following the existing structure, and add an entry to `NETWORK.md`.
- Do not commit secrets, credentials, or per-VM sensitive configuration under any path.

---

## Future: docs.seasonalnet.org

Long-term goal is to publish this documentation at `docs.seasonalnet.org` via [Fumadocs](https://fumadocs.vercel.app/) integrated into the SeasonalWeb Next.js deployment. Design requirements:

- Shared header and footer consistent with all other SeasonalNet SPAs
- Black and white theming (no color accent palette outside of monochrome)
- Source content from this documentation set (file-system routing via Fumadocs MDX)
- `VMs/` must be excluded from any published or version-controlled source
