# Halo Core

> **Status:** Design intent.

Halo Core is the async daemon at the center of the system. It owns the lifecycle of every
enabled Eye, normalizes their events, and serves/forwards the results.

## Responsibilities

1. **Supervision** — start, monitor, and restart Eyes based on config; surface their health.
2. **Normalization** — convert each Eye's source-specific payload into a common event.
3. **Enrichment** *(optional)* — annotate events (e.g., source host, severity mapping).
4. **Serving** — expose a streaming `/events` API and a Prometheus `/metrics` endpoint.
5. **Fan-out** — deliver normalized events to every configured Output.

## Normalized event (proposed)

A single event shape keeps Outputs and consumers simple:

```jsonc
{
  "ts":       "2026-06-14T12:00:00Z",  // RFC3339 timestamp
  "eye":      "crowdsec",              // which collector produced it
  "source":   "lapi",                  // sub-source within the Eye
  "host":     "edge-01",               // origin host, when known
  "severity": "warning",              // normalized: info|warning|critical
  "kind":     "decision",             // event category
  "message":  "ban 203.0.113.5 (http-probing)",
  "labels":   { "ip": "203.0.113.5", "scenario": "http-probing" },
  "raw":      { }                      // original payload, preserved
}
```

## APIs (proposed)

| Endpoint | Purpose |
|----------|---------|
| `GET /metrics` | Prometheus metrics (event counts, Eye health, lag) |
| `GET /events` | Server-sent / WebSocket stream of normalized events |
| `GET /healthz` | Liveness/readiness for orchestration |

## Internal model

- **Channels** connect Eyes → Core → Outputs (bounded, backpressure-aware).
- **Each Eye runs as its own task**; a crash is isolated and restarted, not fatal.
- **Outputs are independent consumers** of the normalized stream.

See [Data Pipeline](data-pipeline.md) for how a single event moves through these stages.
