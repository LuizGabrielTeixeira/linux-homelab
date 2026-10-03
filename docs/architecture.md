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
- **Layered access paths:** DNS, reverse proxying and VPN services serve different
  connectivity needs. Network membership alone does not establish public exposure.
- **Operational feedback:** metrics and availability checks complement scheduled
  security and backup notifications.

## Scope and Constraints

This is a personal, single-host environment; high availability is not claimed.
The documented snapshot has approximately 17 containers across 13 Compose
projects. Counts describe the current inventory, not a capacity target.

Backup copies are local, and verification checks archives and extraction rather
than restored application behavior. Monitoring named volumes sit outside the
main directory backup. Detailed boundaries are recorded in the relevant pages.

## Evidence Basis

These documents use the owner-provided verified environment snapshot. No new
server audit was needed to write them. Component roles and known configuration
are distinguished from runtime health or test results that were not supplied.

## Reading Paths

- Platform: [host](host-platform.md) and [containers](container-platform.md).
- Connectivity: [networking](networking.md) and [security](security.md).
- Operations: [observability](observability.md), [backup](backup-and-recovery.md)
  and [automation](operations.md).
