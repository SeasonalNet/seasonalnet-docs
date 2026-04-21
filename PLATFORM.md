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

SeasonalWeather is the weather, alerting, and radio-oriented subsystem.

SeasonalWeather is an important subsystem within SeasonalNet, but it does not define all of SeasonalNet.

### Voice and communications systems

These include PBX, CME, phone provisioning, and related communications services.

### Web and platform services

These include public front doors, status views, authentication-related services, Git services, and supporting utilities.

### Infrastructure and operations

These include VM hosts, network topology, observability checks, and supporting operational tooling.

## Grounding and truth rules

Documentation describes intended design, stable terminology, and platform reference facts.

Runtime tools provide live operational truth.

If runtime tool output and docs disagree, runtime tool output is authoritative for current state.
