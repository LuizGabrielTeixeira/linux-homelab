# Observability Flow

```mermaid
flowchart LR
    Node[node-exporter: host metrics] --> Prom[Prometheus: collection and time-series storage]
    Cad[cAdvisor: container metrics] --> Prom
    Prom --> Graf[Grafana: visualization]
    Netdata[Netdata] --> Live[Real-time host/container telemetry]
    Kuma[Uptime Kuma] --> Availability[Service availability checks]
```

The node-exporter → Prometheus → Grafana path is validated with host metrics.
Uptime Kuma is actively used for availability checks.

See [observability](../docs/observability.md) for the dashboards and storage boundary.
