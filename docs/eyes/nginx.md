# Eye: NGINX

> **Status:** Design intent.

Tails NGINX access and error logs in real time and emits structured request/error events.

- **Mode:** tail (follow log files)
- **Source data:** access log lines, error log lines

## Configuration

```toml
[eyes.nginx]
enabled = true
access_log = "/var/log/nginx/access.log"
error_log = "/var/log/nginx/error.log"
# Optional: named log format to parse access lines
log_format = "combined"
```

| Field | Description |
|-------|-------------|
| `access_log` | Path to the access log to tail |
| `error_log` | Path to the error log to tail |
| `log_format` | Access log format used for field extraction |

## Normalized output

| Field | Value |
|-------|-------|
| `eye` | `nginx` |
| `kind` | `access` \| `error` |
| `severity` | `info` for access, mapped from error level |
| `labels` | `method`, `path`, `status`, `client_ip`, `bytes` |

## Notes

Log rotation is handled by re-opening the file on truncation/rename. For multi-host
setups, run an Eye per host or ship logs centrally (the Heimdall stack already does this
via syslog-ng → Loki).
