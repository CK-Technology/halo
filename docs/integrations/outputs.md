# Outputs

> **Status:** Design intent.

Outputs receive [normalized events](../architecture/core.md#normalized-event-proposed) from
Halo Core and ship them onward. Multiple Outputs can be enabled at once; each consumes the
stream independently.

## Loki

Push normalized events as log lines with labels.

```toml
[output.loki]
enabled = true
url = "http://127.0.0.1:3100"
# Event fields promoted to Loki labels
labels = ["eye", "severity", "host"]
```

## Graylog

Send events via GELF (the original "Graylog alternative" framing — Halo can also just feed
an existing Graylog).

```toml
[output.graylog]
enabled = false
host = "127.0.0.1"
port = 12201
protocol = "udp"   # udp | tcp
```

## Elasticsearch

Index events for search/dashboards.

```toml
[output.elasticsearch]
enabled = false
url = "http://127.0.0.1:9200"
index = "halo-events"
```

## Custom backend

A generic HTTP sink for anything not listed above:

```toml
[output.http]
enabled = false
url = "https://example.internal/ingest"
# Optional bearer token from the environment
token_env = "HALO_HTTP_OUTPUT_TOKEN"
```

## Delivery semantics

| Output | Delivery |
|--------|----------|
| Loki | best-effort batched push |
| Graylog (GELF) | UDP best-effort / TCP ordered |
| Elasticsearch | bulk index with retry |
| HTTP | per-request, configurable retry |

Exact batching, retry, and ordering guarantees are documented per Output as they are
implemented. In the Heimdall stack, Loki + Grafana remain the primary store and viewer;
Halo Outputs are a way to feed those (or experiment with alternatives) from one daemon.
