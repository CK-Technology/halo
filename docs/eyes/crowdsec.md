# Eye: CrowdSec

> **Status:** Design intent.

Watches a [CrowdSec](https://www.crowdsec.net/) LAPI for decisions and alerts and emits
them as normalized events.

- **Mode:** stream (subscribe to the LAPI decision stream)
- **Source data:** bans/decisions, alerts, scenarios, origin IPs

## Configuration

```toml
[eyes.crowdsec]
enabled = true
lapi_url = "http://127.0.0.1:8080"
lapi_key_env = "HALO_CROWDSEC_LAPI_KEY"   # bouncer API key from the environment
poll_interval_secs = 10                    # stream pull cadence
```

| Field | Description |
|-------|-------------|
| `lapi_url` | Base URL of the CrowdSec Local API |
| `lapi_key_env` | Env var holding the bouncer/LAPI key |
| `poll_interval_secs` | How often to pull new decisions |

## Normalized output

| Field | Value |
|-------|-------|
| `eye` | `crowdsec` |
| `kind` | `decision` \| `alert` |
| `severity` | mapped from scenario / decision type |
| `labels` | `ip`, `scenario`, `duration`, `origin` |

## Notes

In the home estate, CrowdSec already feeds a self-hosted threat-feed pipeline and is
scraped by Prometheus for the Heimdall stack. This Eye is **read-only** and does not alter
CrowdSec decisions or the bouncer flow.
