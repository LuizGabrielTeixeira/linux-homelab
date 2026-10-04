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

Application workloads provide practical context for persistence, routing and
operations. The table groups services by role rather than counting containers.

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

Bind mounts are the primary persistence method. The state below lives under
`~/homelab/` and is covered by the main filesystem archive.

| Bind-mounted state | Examples |
| --- | --- |
| Proxy and access | Nginx Proxy Manager data/certificates; WireGuard and OpenVPN configuration |
| Application and database | Vaultwarden, n8n, Obsidian / WebDAV, PostgreSQL data directory, Filebrowser main database |
| Monitoring and administration | Grafana, Uptime Kuma, Portainer |
| DNS and security | AdGuard Home configuration/data; CrowdSec configuration/data |

These Docker named volumes sit outside the main directory archive:

| Service | State outside the backup |
| --- | --- |
| Prometheus | Historical time-series data |
| Netdata | Cache/state |
| Filebrowser | `/config` state, containing a small `settings.json` |

Filebrowser's main database is bind-mounted and included; its `/config` volume
is separate. The live PostgreSQL files have a
[database-consistency limitation](backup-and-recovery.md#postgresql-limitation).

## Container Operations

Compose organizes service lifecycle by project, with Portainer available for
container administration. Troubleshooting combines container state and logs
with metrics and availability checks; network membership and published ports
are reviewed separately.

See [networking](networking.md) and [backup and recovery](backup-and-recovery.md).
