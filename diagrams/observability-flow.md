# Observability Flow

```mermaid
flowchart LR
    Node[node-exporter: host metrics] --> Prom[Prometheus: collection and time-series storage]
    Cad[cAdvisor: container metrics] --> Prom
    Prom --> Graf[Grafana: visualization]
    Netdata[Netdata] --> Live[Real-time host/container telemetry]
    Kuma[Uptime Kuma] --> Availability[Service availability checks]
```

This shows the monitoring architecture and component roles. It does not certify
the health of every scrape target, dashboard, availability check or alert.

See [observability](../docs/observability.md).
