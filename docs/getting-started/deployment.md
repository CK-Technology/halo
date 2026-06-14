# Deployment

> **Status:** Planned. There is no release artifact or container image yet. This describes
> the intended deployment shapes.

## Local / lab

For experimentation, run the binary directly on the host:

```bash
cargo build --release
./target/release/halo --config ./halo.toml
```

## systemd (planned)

A unit will run Halo as a long-lived service on a dedicated host:

```ini
# /etc/systemd/system/halo.service
[Unit]
Description=Halo security & telemetry aggregator
After=network-online.target
Wants=network-online.target

[Service]
ExecStart=/usr/local/bin/halo --config /etc/halo/halo.toml
Restart=on-failure
DynamicUser=yes

[Install]
WantedBy=multi-user.target
```

## Container (planned)

Following the workstation conventions, the container setup will live under `docker/` with
host networking for local testing:

```
docker/
├── Dockerfile
└── scripts/
```

```yaml
# docker-compose.yml (planned)
services:
  halo:
    build:
      context: .
      dockerfile: docker/Dockerfile
      network: host
    network_mode: host
    volumes:
      - ./halo.toml:/etc/halo/halo.toml:ro
```

Host networking avoids the DNS/connectivity issues hit with bridged Docker networking on
the workstation, and lets Halo reach local sources (CrowdSec LAPI, NGINX logs, exporters)
without port juggling.

## Placement relative to Heimdall

Halo is intended to run as a **separate, optional** process — it observes the same sources
the Heimdall stack does, but does not replace syslog-ng, Loki, Prometheus, or Grafana.
See the [documentation index](../README.md#relationship-to-heimdall).
