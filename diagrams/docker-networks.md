# Docker Networks

```mermaid
flowchart LR
    subgraph ProxyNet[nginx-proxy network]
        NPM[Nginx Proxy Manager]
        Vault[Vaultwarden]
        Files[Filebrowser]
        Kuma[Uptime Kuma]
        Crowd[CrowdSec]
        Automation[n8n]
    end
    subgraph DatabaseNet[database network]
        DB[PostgreSQL]
        AutomationDB[n8n: same container]
    end
    subgraph MonitoringNet[monitoring network]
        Prom[Prometheus]
        Graf[Grafana]
        Cad[cAdvisor]
        Node[node-exporter]
    end
    subgraph HostNet[Host networking]
        DNS[AdGuard Home]
        Netdata[Netdata]
    end
    subgraph WireGuardNet[WireGuard project bridge]
        WG[WireGuard / wg-easy]
    end
    subgraph DefaultNet[Docker default bridge]
        OVPN[OpenVPN]
    end
    Automation -. same container .- AutomationDB
```

Boxes show network membership.
n8n deliberately joins multiple networks to reach infrastructure components;
its two labels represent one container. Other projects have their own networks.

See [container platform](../docs/container-platform.md) and
[networking](../docs/networking.md).
