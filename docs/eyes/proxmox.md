# Eye: Proxmox

> **Status:** Design intent.

Reads Proxmox VE cluster events and node/guest state via the PVE API.

- **Mode:** poll (periodic REST query)
- **Source data:** task log, node status, VM/CT state changes

## Configuration

```toml
[eyes.proxmox]
enabled = true
api_url = "https://127.0.0.1:8006/api2/json"
token_id_env = "HALO_PVE_TOKEN_ID"
token_secret_env = "HALO_PVE_TOKEN_SECRET"
verify_tls = true
poll_interval_secs = 30
```

| Field | Description |
|-------|-------------|
| `api_url` | PVE API base URL |
| `token_id_env` / `token_secret_env` | Env vars for an API token (preferred over passwords) |
| `verify_tls` | Verify the API server certificate |
| `poll_interval_secs` | Poll cadence |

## Normalized output

| Field | Value |
|-------|-------|
| `eye` | `proxmox` |
| `kind` | `task` \| `node-status` \| `guest-status` |
| `labels` | `node`, `vmid`, `type`, `status` |

## Notes

Use a scoped, read-only API token. Across the cluster, run one Eye pointed at the cluster
API rather than one per node. This Eye is **read-only** and does not start/stop guests.
