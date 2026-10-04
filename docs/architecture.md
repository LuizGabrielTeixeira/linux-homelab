# Architecture

This personal homelab is a single Ubuntu Server host running Docker workloads
alongside host-level administration and security services. Its engineering focus
is Linux operations, networking, visibility and recoverability.

See the [high-level diagram](../diagrams/architecture.md).

## Infrastructure Layers

| Layer | Components | Role |
| --- | --- | --- |
| Host | Ubuntu Server 24.04 LTS, systemd, SSH | Operating system and service administration |
| Container platform | Docker Engine, Docker Compose | Service lifecycle and persistence |
| Networking | Docker bridges, host networking, upstream router | Service connectivity and access boundaries |
| Infrastructure services | AdGuard Home, Nginx Proxy Manager, WireGuard / wg-easy, OpenVPN | DNS, reverse proxying and VPN access |
| Observability | Prometheus, Grafana, exporters, Netdata, Uptime Kuma | Metrics, visualization and availability checks |
| Operations | cron, n8n, Telegram integration | Scheduled work and status notifications |
| Backup | Daily archives, local and USB copies, verification | Filesystem recovery material and integrity checks |

## Design Choices

- **Service-oriented network separation:** reverse proxy, database and monitoring
  services use distinct Docker networks. n8n joins multiple networks intentionally.
- **Visible persistent state:** most important service state is bind-mounted
  below `~/homelab/`, making the filesystem backup boundary explicit.
- **Layered access paths:** DNS resolves names, the reverse proxy routes service
  requests and VPNs provide remote access.
- **Operational feedback:** metrics and availability checks complement scheduled
  security and backup notifications.

## Scope and Operational Boundaries

The environment runs approximately 17 containers across 13 Compose projects
on one host. Host maintenance affects the services it runs.

Backup copies are local. Verification combines integrity checks and an
extraction-based restore simulation; application-level recovery remains a
separate task. Some named-volume state is outside the main directory archive.
See [backup and recovery](backup-and-recovery.md) for the exact boundary.

## Reading Paths

- Platform: [host](host-platform.md) and [containers](container-platform.md).
- Connectivity: [networking](networking.md) and [security](security.md).
- Operations: [observability](observability.md), [backup](backup-and-recovery.md)
  and [automation](operations.md).
