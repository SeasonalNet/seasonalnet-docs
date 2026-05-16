# SeasonalNet

## What SeasonalNet is

SeasonalNet is the overall solo-operated homelab and service platform run by Seasonal_Currency.

It includes infrastructure, communications systems, web front doors, operational tooling, development services, and weather-related systems.

SeasonalNet is the platform as a whole. It is not limited to weather operations.

## What SeasonalNet is not

SeasonalNet is not only SeasonalWeather.
SeasonalNet is not only the network topology.
SeasonalNet is not only one VM host, one service, or one runtime.
SeasonalNet is not fully represented by any single subsystem.

## Operator model

SeasonalNet is operated by one primary operator: Seasonal_Currency.

It is a personal homelab and service platform, not a multi-team enterprise environment.

## Major platform domains

### Seasonal Agent

Seasonal Agent is the local assistant runtime used for operator support, inspection, reasoning, and bounded read-only tool execution.

Seasonal Agent is a subsystem of SeasonalNet. It is not the whole platform.

### SeasonalWeather

SeasonalWeather is the weather, alerting, and radio-oriented subsystem. The current runtime VM is SeasonalWX at `192.168.1.10`; the older VM named SeasonalWeather has been retired and replaced.

SeasonalWeather is an important subsystem within SeasonalNet, but it does not define all of SeasonalNet.

### Voice and communications systems

These include PBX, CME, phone provisioning, and related communications services.

### Web and platform services

These include public front doors, status views, authentication-related services, Git services, and supporting utilities.

### Infrastructure and operations

These include VM hosts, network topology, observability checks, backup systems, and supporting operational tooling. SeasonalR230 and SeasonalR340 are the active Proxmox hosts; SeasonalR340 also hosts the SeasonalBack Proxmox Backup Server VM.

SeasonalMonitor is the current observability spine for infrastructure reachability, dashboards, and later SNMP/Redfish collection. Grafana is exposed publicly at `grafana.seasonalnet.org` through SeasonalWeb with Authentik OAuth/OIDC; Prometheus remains a private backend service.

## Grounding and truth rules

Documentation describes intended design, stable terminology, and platform reference facts.

Runtime tools provide live operational truth.

If runtime tool output and docs disagree, runtime tool output is authoritative for current state.
