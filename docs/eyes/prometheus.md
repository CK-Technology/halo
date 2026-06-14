# Eye: Prometheus

> **Status:** Design intent.

Scrapes one or more Prometheus-format `/metrics` endpoints and emits selected series as
normalized events (and/or re-exposes them on Halo's own `/metrics`).

- **Mode:** scrape (periodic HTTP GET of metrics endpoints)
- **Source data:** exporter metrics (node_exporter, cAdvisor, app metrics)

## Configuration

```toml
[eyes.prometheus]
enabled = true
scrape = [
  "http://127.0.0.1:9100/metrics",   # node_exporter
  "http://127.0.0.1:8085/metrics",   # cAdvisor
]
interval_secs = 15
```

| Field | Description |
|-------|-------------|
| `scrape` | List of metrics endpoints to poll |
| `interval_secs` | Scrape interval |

## Normalized output

| Field | Value |
|-------|-------|
| `eye` | `prometheus` |
| `kind` | `metric` |
| `labels` | metric name + its Prometheus labels |

## Notes

Halo does not aim to replace Prometheus. In the Heimdall stack, Prometheus remains the
metrics system of record; this Eye is for pulling a focused subset of series into Halo's
event view during experiments.
