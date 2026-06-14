# Security Policy

> **Note:** Halo is **experimental** and is not intended for production use. It runs as a
> personal lab project alongside — not in place of — a production observability stack. Do
> not rely on it to secure anything you care about.

## Supported versions

Halo has not reached a stable release. No versions carry security guarantees yet.

## Reporting a vulnerability

If you find a security issue, please report it privately rather than opening a public issue:

- Open a [GitHub security advisory](https://github.com/CK-Technology/halo/security/advisories/new) on the repository, or
- Contact the maintainers through the [CK-Technology/halo](https://github.com/CK-Technology/halo) repository.

Please include reproduction steps and the affected commit. As an experimental project,
response times are best-effort.

## Scope notes

By design, Halo's collectors ("Eyes") are **read-only** — they observe sources such as
CrowdSec, Wazuh, NGINX, firewalls, Tailscale, and Proxmox, and do not modify them. Treat
any deviation from that read-only posture as a security-relevant bug.
