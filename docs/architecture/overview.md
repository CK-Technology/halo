# Architecture Overview

> **Status:** Design intent. The shapes below describe the target architecture; the current
> codebase is a minimal binary.

Halo has three layers: **Eyes** (collectors), **Halo Core** (the async daemon), and
**Outputs** (forwarders). Events flow left to right.

```
┌────────────┐     ┌──────────────────────────┐     ┌────────────┐
│   Eyes     │     │        Halo Core         │     │  Outputs   │
│ (collect)  │     │  (normalize + serve)     │     │ (forward)  │
├────────────┤     ├──────────────────────────┤     ├────────────┤
│ crowdsec   │──┐  │  ingest → normalize →    │  ┌─►│ loki       │
│ wazuh      │──┤  │  enrich → fan-out        │  │  │ graylog    │
│ prometheus │──┼─►│                          │──┼─►│ elastic    │
│ nginx      │──┤  │  /events  (stream API)   │  │  │ custom     │
│ firewall   │──┤  │  /metrics (Prometheus)   │  └─►│            │
│ tailscale  │──┤  │                          │     │            │
│ proxmox    │──┘  └──────────────────────────┘     └────────────┘
└────────────┘
```

## Layers

### Eyes (collectors)

Each Eye watches exactly one source (CrowdSec LAPI, a Wazuh API, NGINX logs, …), converts
its native data into a common normalized event, and hands it to Core. Eyes share a common
lifecycle and interface so new sources are cheap to add. See [Eyes overview](../eyes/overview.md).

### Halo Core (daemon)

An async Rust daemon that:

- supervises enabled Eyes,
- normalizes and (optionally) enriches their events,
- exposes a unified `/events` stream and a Prometheus `/metrics` endpoint,
- fans events out to configured Outputs.

See [Halo Core](core.md).

### Outputs (forwarders)

Sinks that receive normalized events: Loki, Graylog, Elasticsearch, or a custom backend.
See [Outputs](../integrations/outputs.md).

## Design principles

- **Rust-native, async-first** — one supervised process, no plugin runtime.
- **Normalize early** — every Eye emits the same event shape so Outputs stay simple.
- **Read-only by default** — Halo observes; it does not mutate the sources it watches.
- **Composable** — enable only the Eyes and Outputs you need via config.
