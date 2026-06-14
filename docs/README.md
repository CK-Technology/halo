# Halo Documentation

Halo is an **experimental** security & telemetry aggregation platform built in Rust.
It is a personal lab project — separate from, and not a replacement for, the production
[Heimdall observability stack](#relationship-to-heimdall).

> **Status:** Experimental. The documents below describe the intended design and the
> current state of each piece. Anything not marked **Implemented** is design intent.

## Quick Start

| Document | Description |
|----------|-------------|
| [Installation](getting-started/installation.md) | Build from source and run the daemon |
| [Configuration](getting-started/configuration.md) | Config file layout and options |
| [Deployment](getting-started/deployment.md) | Running Halo on a server / in a container |

## Architecture

| Document | Description |
|----------|-------------|
| [Overview](architecture/overview.md) | How Halo Core, Eyes, and the pipeline fit together |
| [Halo Core](architecture/core.md) | The async daemon, APIs, and event model |
| [Data Pipeline](architecture/data-pipeline.md) | Ingest → normalize → forward |

## Eyes (Collectors)

The modular "Eyes" each watch one source and feed normalized events into Halo Core.

| Document | Source |
|----------|--------|
| [Overview](eyes/overview.md) | Eye design, lifecycle, and the common interface |
| [CrowdSec](eyes/crowdsec.md) | LAPI decision/alert stream |
| [Wazuh](eyes/wazuh.md) | Wazuh REST API security events |
| [Prometheus](eyes/prometheus.md) | Metrics scraping |
| [NGINX](eyes/nginx.md) | Access/error log tailing |
| [Firewall](eyes/firewall.md) | Fortinet + nftables events |
| [Tailscale](eyes/tailscale.md) | Tailnet status and node state |
| [Proxmox](eyes/proxmox.md) | Proxmox VE cluster events |

## Integrations

| Document | Description |
|----------|-------------|
| [Outputs](integrations/outputs.md) | Forwarding to Graylog, Loki, and Elasticsearch |

## Development

| Document | Description |
|----------|-------------|
| [Roadmap](development/roadmap.md) | What exists today vs. what's planned |
| [Contributing](development/contributing.md) | How to build, test, and contribute |

---

## Relationship to Heimdall

Halo does **not** run production monitoring. That job belongs to the **Heimdall stack**:

```
syslog-ng (native) ──► Loki ──┐
node_exporter / cAdvisor ─► Prometheus ─► Alertmanager
                               ├──► Grafana (dashboards)
CrowdSec LAPI ─(scrape)────────┘         (read-only views)
Wazuh indexer ─(OpenSearch datasource)─► Grafana
```

Halo is where I experiment with a single Rust daemon that could one day own pieces of
that pipeline. Until then, Heimdall is the source of truth and Halo is a sandbox.

## Resources

- [Project README](../README.md)
- [GitHub — CK-Technology/halo](https://github.com/CK-Technology/halo)
- [License](../LICENSE)
