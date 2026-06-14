# Eyes Overview

> **Status:** Design intent. No Eye is implemented yet; this defines the shared model so
> each can be built consistently.

An **Eye** is a self-contained collector that watches one source and emits
[normalized events](../architecture/core.md#normalized-event-proposed) into Halo Core.

## Common interface

Every Eye implements the same contract so Core can supervise them uniformly:

| Capability | Description |
|------------|-------------|
| `name()` | Stable identifier used in events and config (`crowdsec`, `nginx`, …) |
| `start()` | Begin collecting; runs as its own async task |
| `events()` | Async stream of normalized events |
| `health()` | Current status (ok / degraded / failed) and last-seen timestamp |
| `shutdown()` | Cleanly stop and release resources |

## Collection modes

Eyes fall into a few patterns:

| Mode | Eyes | Notes |
|------|------|-------|
| **Stream** | CrowdSec | Subscribe to a long-lived push stream |
| **Poll** | Wazuh, Proxmox, Tailscale | Periodic REST/CLI poll on an interval |
| **Scrape** | Prometheus | Periodic metrics scrape |
| **Tail** | NGINX, firewall (nftables) | Follow log files / event sources |

## Lifecycle

1. Core reads config and instantiates each **enabled** Eye.
2. Each Eye `start()`s as an isolated task.
3. Normalized events flow into Core; failures restart only that Eye.
4. On shutdown, Core calls `shutdown()` on each Eye.

## Available Eyes

| Eye | Source | Doc |
|-----|--------|-----|
| CrowdSec | LAPI decision/alert stream | [crowdsec.md](crowdsec.md) |
| Wazuh | Wazuh REST API | [wazuh.md](wazuh.md) |
| Prometheus | Metrics endpoints | [prometheus.md](prometheus.md) |
| NGINX | Access/error logs | [nginx.md](nginx.md) |
| Firewall | Fortinet + nftables | [firewall.md](firewall.md) |
| Tailscale | Tailnet status | [tailscale.md](tailscale.md) |
| Proxmox | PVE cluster events | [proxmox.md](proxmox.md) |

## Adding a new Eye

1. Implement the common interface for the source.
2. Map its payload to the normalized event shape.
3. Add a `[eyes.<name>]` config block (see [Configuration](../getting-started/configuration.md)).
4. Document it as `docs/eyes/<name>.md` and link it here and in the index.
