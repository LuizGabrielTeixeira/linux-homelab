# Container Platform

Docker Engine and Docker Compose organize approximately 17 containers across
13 projects. Multiple bridge networks and selected host-network containers
support different infrastructure roles.

## Service Inventory

| Role | Services |
| --- | --- |
| DNS and reverse proxy | AdGuard Home, Nginx Proxy Manager |
| VPN access | WireGuard / wg-easy, OpenVPN |
| Database and operations | PostgreSQL, n8n, Portainer |
| Monitoring | Prometheus, Grafana, cAdvisor, node-exporter, Netdata, Uptime Kuma |
| Security integration | CrowdSec |
| Supporting applications | Vaultwarden, Filebrowser, Obsidian / WebDAV |

The table groups roles; it is not a one-row-per-container count. Application
workloads provide practical context for persistence, routing and operations.

## Network Membership

| Network / mode | Members |
| --- | --- |
| `nginx-proxy` | Nginx Proxy Manager, Vaultwarden, Filebrowser, Uptime Kuma, CrowdSec, n8n |
| `database` | PostgreSQL, n8n |
| `monitoring` | Prometheus, Grafana, cAdvisor, node-exporter |
| Host networking | AdGuard Home, Netdata |
| Docker default bridge | OpenVPN |
| Dedicated project bridge | WireGuard / wg-easy |

Other projects use their own networks. n8n deliberately belongs to multiple
networks because it needs connectivity to multiple infrastructure components.
See the [network diagram](../diagrams/docker-networks.md).

## Persistence

Bind mounts are the primary persistence method. Important state lives below
`~/homelab/`, with the exact directory layout intentionally generalized.

| Bind-mounted state | Examples |
| --- | --- |
| Proxy and access | Nginx Proxy Manager data/certificates; WireGuard and OpenVPN configuration |
| Application and database | Vaultwarden, n8n, Obsidian / WebDAV, PostgreSQL, Filebrowser database |
| Monitoring and administration | Grafana, Uptime Kuma, Portainer |
| DNS and security | AdGuard Home configuration/data; CrowdSec configuration/data |

Prometheus time-series data and Netdata cache/state use Docker named volumes.
Those volumes are outside the filesystem backup of the main homelab directory.
Their historical data is therefore not covered by that backup process.

## Lifecycle and Health Claims

Compose provides project-level organization. Portainer is present for container
administration. Neither establishes that every workload is healthy.

Per-container restart policies, health-check coverage and current health results
were not included in the supplied evidence, so no uniform policy or health count
is asserted here. Published ports also need separate review from network membership.

See [networking](networking.md) and [backup and recovery](backup-and-recovery.md).
