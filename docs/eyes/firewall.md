# Eye: Firewall

> **Status:** Design intent.

Monitors firewall activity from **Fortinet** devices and local **nftables**, emitting
normalized allow/deny and policy events.

- **Mode:** tail / poll
  - Fortinet: syslog ingest or REST poll
  - nftables: kernel log / event tail
- **Source data:** policy hits, blocked connections, IPS/UTM events

## Configuration

```toml
[eyes.firewall]
enabled = true

[eyes.firewall.fortinet]
enabled = true
# Receive FortiGate syslog, or poll the REST API
syslog_listen = "0.0.0.0:5514"

[eyes.firewall.nftables]
enabled = true
# Source of nftables log events (journald/kmsg)
source = "journald"
```

| Field | Description |
|-------|-------------|
| `fortinet.syslog_listen` | Address/port to receive FortiGate syslog |
| `nftables.source` | Where nftables log events are read from |

## Normalized output

| Field | Value |
|-------|-------|
| `eye` | `firewall` |
| `source` | `fortinet` \| `nftables` |
| `kind` | `policy` \| `block` \| `ips` |
| `labels` | `src`, `dst`, `proto`, `action`, `policy_id` |

## Notes

The production edge (FortiGate 90G) already enforces geo-allowlists, threat feeds, and IPS,
and ships syslog to Heimdall. This Eye is for pulling those same events into Halo for
experimentation — it does **not** push rules back to the firewall.
