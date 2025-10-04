<div align="center">
  <img src="assets/icons/halo.png" alt="Halo Logo" width="200"/>

  # 🌌 Halo

  **Unified Security & Telemetry Platform for the GhostStack**

  ![rust](https://img.shields.io/badge/Built%20with-Rust-CE422B?logo=rust&logoColor=white)
  ![siem](https://img.shields.io/badge/Type-SOC%20%26%20SIEM-blue?logo=security)
  ![crowdsec](https://img.shields.io/badge/Integration-CrowdSec-4B7BBE?logo=crowdsource)
  ![wazuh](https://img.shields.io/badge/Integration-Wazuh-005B94)
  ![graylog](https://img.shields.io/badge/Integration-Graylog-888888)
  ![fortinet](https://img.shields.io/badge/Integration-Fortinet-EE3124)
  ![proxmox](https://img.shields.io/badge/Integration-Proxmox%20VE-E57000?logo=proxmox)
  ![tailscale](https://img.shields.io/badge/Integration-Tailscale-3C3C3C?logo=tailscale)
  ![prometheus](https://img.shields.io/badge/Metrics-Prometheus-DA4E2B?logo=prometheus)
  ![nginx](https://img.shields.io/badge/Integration-NGINX-009639?logo=nginx)
  ![archlinux](https://img.shields.io/badge/Tested%20on-Arch%20Linux-1793D1?logo=archlinux)
  ![license](https://img.shields.io/badge/License-MIT-lightgrey)
</div>

---

## 🎯 Overview

**Halo** is a plug-and-drop distributed security observability platform built in Rust. Deploy it on your server and let it seamlessly integrate with your entire infrastructure — NGINX servers, Tailscale nodes, Fortinet firewalls, and more. Think of it as a next-generation Graylog alternative with native support for modern security tools and zero-trust networking.

**One console to monitor, visualize, and enforce zero-trust posture across your entire ecosystem.**

### Key Features

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
git clone https://github.com/yourusername/halo.git
cd halo

# Build with Cargo
cargo build --release

# Run Halo
./target/release/halo
```

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

---

## 📄 License

MIT License - see LICENSE file for details

