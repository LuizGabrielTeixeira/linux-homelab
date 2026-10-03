# High-Level Architecture

```mermaid
flowchart TD
    Internet[Internet] --- Router[Upstream router / NAT]
    LAN[Private LAN] --- Router
    Router --- Host[home-server: Ubuntu Server 24.04 LTS]
    Host --> Native[Host services: SSH, UFW, systemd, cron]
    Host --> Docker[Docker Engine / Compose]
    Docker --> DNS[DNS: AdGuard Home]
    Docker --> VPN[VPN: WireGuard / wg-easy and OpenVPN]
    Docker --> Proxy[Reverse proxy: Nginx Proxy Manager]
    Docker --> Metrics[Observability stack]
    Docker --> Workloads[Persistent services and automation]
```

Connections represent architecture, not a map of externally reachable ports.
Actual Internet exposure depends on upstream router/NAT configuration and
host/container networking. DNS and VPN services also run as containers.

See [architecture](../docs/architecture.md) and [networking](../docs/networking.md).
