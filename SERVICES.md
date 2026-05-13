# SeasonalNet Services Overview

## Purpose

This document describes the major systems that make up SeasonalNet and the role each system serves.

This is platform reference material. It is not a live status page.

## Core systems

### Seasonal Agent

Role: local assistant runtime for operator-facing support, reasoning, and bounded read-only inspection.

Boundary: answers about live state should use tools when available.

### SeasonalWeather

Role: weather, alerting, broadcast, and related operational workflows.

Boundary: SeasonalWeather is one subsystem within SeasonalNet, not a synonym for the whole platform.

### SeasonalPBX, CME, and phone services

Role: telephony, call control, phone provisioning, and related communications functions. SeasonalPBX is the current FreePBX/Asterisk VM and Phone-LAN gateway at `192.168.1.12` / `192.168.56.1`; it replaced the retired legacy FreePBX17 VM.

Boundary: voice systems are part of SeasonalNet's communications domain.

### Auth, Git, DNS, and platform support services

Role: identity, code hosting, internal DNS, and supporting service infrastructure.

Boundary: these services support operator workflows and platform administration.

### SeasonalDNS

Role: internal Main-LAN DNS resolver and local DNS management service, expected to run Technitium DNS Server.

Boundary: SeasonalDNS is not public authoritative DNS for `seasonalnet.org`; Cloudflare remains authoritative for public SeasonalNet DNS.

### SeasonalMonitor

Role: first-pass observability spine for SeasonalNet infrastructure.

SeasonalMonitor runs on `192.168.1.15` and provides Prometheus, Grafana, blackbox_exporter, snmp_exporter, and host-native node_exporter. Its first targets are SeasonalMonitor itself, SeasonalDNS, SeasonalR230, R230 iDRAC, and SeasonalCME-SW2 / CME rack backhaul reachability.

Grafana is publicly reachable at `https://grafana.seasonalnet.org` through SeasonalWeb and uses Authentik OAuth/OIDC. A local Grafana admin account is retained as break-glass access. Prometheus remains private and should not be published directly.

Boundary: SeasonalMonitor is observability infrastructure. It does not replace SeasonalAgent, does not mutate infrastructure, and should not become a custom monitoring platform before the standard Prometheus/Grafana/SNMP/Redfish foundation is complete.

### Retired systems

SeasonalUtilities is retired. It previously occupied `192.168.1.11` and hosted utility functions such as Uptime Kuma / `status.seasonalnet.org`, but the VM has been archived and removed from the active VirtualBox inventory. Treat `192.168.1.11` as reserved until a replacement utility/status host is deliberately assigned.

### Web front doors and SPAs

Role: browser-facing interfaces for platform functions such as docs, radio, PBX, provisioning, agent access, or status views.

Boundary: a web front door may expose part of SeasonalNet, but not the full live platform state by itself.

## Observability boundaries

SeasonalMonitor is the central first-pass metrics and dashboarding system. Grafana is the human-facing dashboard front end; Prometheus is the private metrics backend; exporters collect host, reachability, SNMP, and later Redfish state.

Seasonal Agent may inspect some parts of SeasonalNet through configured tools and caller context.

It should not assume that one instrumented runtime host exposes the entire platform.

Questions about current live state should use tools when available.
Questions about platform identity, subsystem boundaries, or intended design may be answered from docs.
