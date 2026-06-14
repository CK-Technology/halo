<div align="center">
  <img src="assets/icons/halo.png" alt="Halo Logo" width="200"/>

  # 🌌 Halo

  **Unified Security & Telemetry Platform for the GhostStack**

  ![rust](https://img.shields.io/badge/Built%20with-Rust-CE422B?style=for-the-badge&logo=rust&logoColor=white)
  ![siem](https://img.shields.io/badge/Type-SOC%20%26%20SIEM-blue?style=for-the-badge&logo=security&logoColor=white)
  ![crowdsec](https://img.shields.io/badge/Integration-CrowdSec-4B7BBE?style=for-the-badge&logo=crowdsource&logoColor=white)
  ![wazuh](https://img.shields.io/badge/Integration-Wazuh-005B94?style=for-the-badge)
  ![graylog](https://img.shields.io/badge/Integration-Graylog-888888?style=for-the-badge)
  ![fortinet](https://img.shields.io/badge/Integration-Fortinet-EE3124?style=for-the-badge)
  ![proxmox](https://img.shields.io/badge/Integration-Proxmox%20VE-E57000?style=for-the-badge&logo=proxmox&logoColor=white)
  ![tailscale](https://img.shields.io/badge/Integration-Tailscale-3C3C3C?style=for-the-badge&logo=tailscale&logoColor=white)
  ![prometheus](https://img.shields.io/badge/Metrics-Prometheus-DA4E2B?style=for-the-badge&logo=prometheus&logoColor=white)
  ![nginx](https://img.shields.io/badge/Integration-NGINX-009639?style=for-the-badge&logo=nginx&logoColor=white)
  ![archlinux](https://img.shields.io/badge/Tested%20on-Arch%20Linux-1793D1?style=for-the-badge&logo=archlinux&logoColor=white)
  ![license](https://img.shields.io/badge/License-MIT-lightgrey?style=for-the-badge)
</div>

---

> [!WARNING]
> **Experimental.** Halo is a personal lab project I tinker with *alongside*,
> not in place of, my production observability stack. Real monitoring/SIEM duties are
> handled by the **Heimdall stack** (syslog-ng → Loki, Prometheus, Alertmanager, Grafana,
> with read-only views into CrowdSec + Wazuh). Treat everything here as a moving target:
> APIs, architecture, and the "Eyes" modules described below are design intent, not a
> finished product. Do not deploy this anywhere you care about.

---

## 🎯 Overview

**Halo** aims to be a plug-and-drop distributed security observability platform built in Rust. The goal: drop it on a server and let it integrate with the surrounding infrastructure — NGINX servers, Tailscale nodes, Fortinet firewalls, and more — acting as a modern, Rust-native Graylog alternative with first-class support for security tooling and zero-trust networking.

**One console to monitor, visualize, and reason about posture across the ecosystem.**

> This is the *target* design. See the experimental warning above and the [roadmap](docs/development/roadmap.md) for what actually exists today.

### Design Goals

- 🔌 **Plug & Play Deployment** — Drop Halo on a server and watch it connect to your infrastructure
- 🛡️ **Multi-Source Security Aggregation** — CrowdSec, Wazuh, Fortinet firewalls, NGINX logs, and more
- 🌐 **Zero-Trust Network Integration** — Native Tailscale and Headscale support
- 📊 **Advanced Log Management** — Modern Graylog alternative with better performance
- 🦀 **Built with Rust** — High performance, memory-safe, and reliable
- 🔄 **Real-Time Telemetry** — Prometheus metrics and live event streaming

---

## 🧠 Architecture

### Halo Core (Rust)

Async daemon that collects telemetry via WebSocket, Kafka, or REST and exposes a unified `/metrics` and `/events` API.

### Modular "Eyes" System

- **halo-wazuh** — Polls Wazuh REST API for security events
- **halo-crowdsec** — Subscribes to CrowdSec LAPI stream for threat intelligence
- **halo-prom** — Prometheus metrics scraper
- **halo-nginx** — Tails NGINX access/error logs in real-time
- **halo-firewall** — Monitors Fortinet firewalls and nftables events
- **halo-tailscale** — Integrates with Tailscale via `tailscale status --json`
- **halo-proxmox** — Reads Proxmox VE cluster events

### Data Pipeline

Aggregates and forwards events to:
- Graylog
- Loki
- Elasticsearch
- Custom backends

### UI Layer *(Coming Soon)*

Modern dashboard built with Tauri + Leptos or React for real-time visualization and control.

---

## 🚀 Getting Started

```bash
# Clone the repository
git clone https://github.com/CK-Technology/halo.git
cd halo

# Build with Cargo
cargo build --release

# Run Halo
./target/release/halo
```

---

## 📚 Documentation

Full documentation lives in [`docs/`](docs/README.md).

- **[Documentation Index](docs/README.md)** — start here
- **[Getting Started](docs/getting-started/installation.md)** — build, run, configure
- **[Architecture](docs/architecture/overview.md)** — core daemon, Eyes, data pipeline
- **[Eyes (Collectors)](docs/eyes/overview.md)** — CrowdSec, Wazuh, Prometheus, NGINX, firewall, Tailscale, Proxmox
- **[Integrations](docs/integrations/outputs.md)** — Graylog, Loki, Elasticsearch outputs
- **[Roadmap](docs/development/roadmap.md)** — what exists vs. what's planned

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

---

## 📄 License

MIT License - see LICENSE file for details

