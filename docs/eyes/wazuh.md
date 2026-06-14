# Eye: Wazuh

> **Status:** Design intent.

Polls the [Wazuh](https://wazuh.com/) REST API for security events and agent state.

- **Mode:** poll (periodic REST query)
- **Source data:** alerts, rule matches, agent status

## Configuration

```toml
[eyes.wazuh]
enabled = true
api_url = "https://127.0.0.1:55000"
user_env = "HALO_WAZUH_USER"
password_env = "HALO_WAZUH_PASSWORD"
verify_tls = true
poll_interval_secs = 30
```

| Field | Description |
|-------|-------------|
| `api_url` | Wazuh API base URL |
| `user_env` / `password_env` | Env vars holding API credentials |
| `verify_tls` | Verify the API server certificate |
| `poll_interval_secs` | Poll cadence |

## Normalized output

| Field | Value |
|-------|-------|
| `eye` | `wazuh` |
| `kind` | `alert` \| `agent-status` |
| `severity` | mapped from Wazuh rule level |
| `labels` | `rule_id`, `agent`, `groups` |

## Notes

The Heimdall stack already surfaces Wazuh through Grafana via an OpenSearch datasource.
This Eye complements that with event-level normalization inside Halo and is **read-only**.
