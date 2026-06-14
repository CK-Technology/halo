# Eye: Tailscale

> **Status:** Design intent.

Tracks tailnet state via `tailscale status --json` (or the local API) and emits node
online/offline and connectivity events.

- **Mode:** poll (periodic status read)
- **Source data:** peers, online state, exit nodes, last-seen times

## Configuration

```toml
[eyes.tailscale]
enabled = true
# How Halo reads tailnet state
source = "cli"            # "cli" (tailscale status --json) or "localapi"
poll_interval_secs = 30
```

| Field | Description |
|-------|-------------|
| `source` | `cli` shells out to `tailscale status --json`; `localapi` uses the local socket |
| `poll_interval_secs` | Poll cadence |

## Normalized output

| Field | Value |
|-------|-------|
| `eye` | `tailscale` |
| `kind` | `node-online` \| `node-offline` \| `node-change` |
| `labels` | `node`, `tailnet_ip`, `os`, `exit_node` |

## Notes

Halo only **observes** the tailnet — it does not manage ACLs or routes. Headscale support
follows the same model where it exposes equivalent status data.
