# Configuration

> **Status:** Planned. No configuration is read yet. This document captures the intended
> configuration model so the surrounding code can be built against it.

## Goals

- A single, declarative config file describes which **Eyes** are enabled and where Halo
  forwards normalized events.
- Secrets (API tokens, LAPI keys) are referenced from the environment, never committed.
- Sensible defaults so a minimal config "just works" for local experiments.

## Proposed layout

A TOML file (default: `/etc/halo/halo.toml`, override with `--config`):

```toml
[core]
# Address the daemon binds for its API and /metrics endpoint
listen = "127.0.0.1:8787"
log_level = "info"

# --- Eyes: each block enables one collector ---

[eyes.crowdsec]
enabled = true
lapi_url = "http://127.0.0.1:8080"
lapi_key_env = "HALO_CROWDSEC_LAPI_KEY"   # read from environment

[eyes.wazuh]
enabled = false
api_url = "https://127.0.0.1:55000"

[eyes.prometheus]
enabled = true
scrape = ["http://127.0.0.1:9100/metrics"]
interval_secs = 15

[eyes.nginx]
enabled = true
access_log = "/var/log/nginx/access.log"
error_log = "/var/log/nginx/error.log"

# --- Outputs: where normalized events go ---

[output.loki]
enabled = true
url = "http://127.0.0.1:3100"
```

## Environment variables

| Variable | Purpose |
|----------|---------|
| `HALO_CONFIG` | Path to the config file (alternative to `--config`) |
| `HALO_CROWDSEC_LAPI_KEY` | CrowdSec LAPI key referenced by `lapi_key_env` |

See [Eyes overview](../eyes/overview.md) and [Outputs](../integrations/outputs.md) for the
per-source and per-sink options as they are designed.
