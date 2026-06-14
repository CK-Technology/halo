# Roadmap

Halo is **experimental**. This page is the honest accounting of what exists versus what is
still design intent, so the docs above can be read with the right expectations.

## Today

- [x] Repository scaffold and Cargo project (Rust, edition 2024)
- [x] Documentation structure and target architecture
- [ ] Minimal binary only — no collectors, APIs, or outputs yet

## Near term

- [ ] Halo Core skeleton: async runtime, config loading, Eye supervisor
- [ ] Normalized event type and internal event channel
- [ ] First Eye end-to-end (likely **CrowdSec** or **NGINX**, both already running locally)
- [ ] First Output end-to-end (likely **Loki**, to match the Heimdall stack)
- [ ] `/healthz` and `/metrics` endpoints

## Later

- [ ] Remaining Eyes: Wazuh, Prometheus, firewall, Tailscale, Proxmox
- [ ] Remaining Outputs: Graylog (GELF), Elasticsearch, generic HTTP
- [ ] `/events` streaming API
- [ ] Container image + `docker/` setup (host networking)
- [ ] systemd unit and packaging

## Explicitly out of scope (for now)

- Replacing the production **Heimdall stack** (syslog-ng, Loki, Prometheus, Alertmanager,
  Grafana, CrowdSec, Wazuh). Halo observes the same sources; it does not take over.
- Writing back to or mutating any monitored source. Every Eye is read-only by design.
- A UI/dashboard layer — deferred until Core and a couple of Eyes are real.

## Contributing

See [contributing.md](contributing.md).
