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

### FreePBX, CME, and phone services

Role: telephony, call control, phone provisioning, and related communications functions.

Boundary: voice systems are part of SeasonalNet's communications domain.

### Auth, Git, and platform support services

Role: identity, code hosting, and supporting service infrastructure.

Boundary: these services support operator workflows and platform administration.

### Web front doors and SPAs

Role: browser-facing interfaces for platform functions such as docs, radio, PBX, provisioning, agent access, or status views.

Boundary: a web front door may expose part of SeasonalNet, but not the full live platform state by itself.

## Observability boundaries

Seasonal Agent may inspect some parts of SeasonalNet through configured tools and caller context.

It should not assume that one instrumented runtime host exposes the entire platform.

Questions about current live state should use tools when available.
Questions about platform identity, subsystem boundaries, or intended design may be answered from docs.
