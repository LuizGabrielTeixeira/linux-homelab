# Observability

The monitoring stack combines Prometheus, node-exporter and cAdvisor for
infrastructure metrics, with Grafana for visualization. Netdata and Uptime
Kuma provide complementary real-time telemetry and availability checks.

## Component Roles

| Component | Role |
| --- | --- |
| node-exporter | Host metrics |
| cAdvisor | Container metrics |
| Prometheus | Metric collection and time-series storage |
| Grafana | Metric visualization |
| Netdata | Real-time host/container telemetry |
| Uptime Kuma | Service availability checks |

See the [observability flow](../diagrams/observability-flow.md).

## Connectivity and Storage

Prometheus, Grafana, cAdvisor and node-exporter belong to the `monitoring`
network. Netdata uses host networking; Uptime Kuma belongs to `nginx-proxy`.
These are network memberships, not proof of unrestricted cross-network access.

Grafana and Uptime Kuma state is bind-mounted below the main homelab directory.
Prometheus time-series data and Netdata cache/state use named volumes outside
that directory's filesystem backup. Historical monitoring data is not claimed
as recoverable from the daily homelab archive.

## Verification Boundary

The supplied snapshot establishes the deployed components and their roles.
It does not provide current scrape results, dashboard contents, configured
check coverage or alert-delivery tests.

Before claiming a particular host or service is actively monitored, confirm its
target/check status and recent data. A running container alone does not verify
collection, visualization or notification delivery.

## Operational Use

Host metrics, container metrics and service checks offer different views of a
failure. Together with host and container logs, they support distinguishing
resource pressure, container faults and connectivity problems.

Security and backup notifications are described in [operations](operations.md);
they are not evidence of a verified Prometheus alerting configuration.
