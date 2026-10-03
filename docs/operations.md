# Operations and Automation

Operations combine native scheduling, container administration, monitoring and
n8n workflows. n8n is an operational integration component, not just an
application hosted on the server.

## Automation Components

| Component | Role |
| --- | --- |
| cron | Schedule backup, verification and other operational work |
| n8n | Process operational results and classify status |
| Telegram integration | Deliver operational notifications |
| Unattended upgrades | Automate package update handling |
| systemd / Docker Compose | Manage host services and container projects |

## Notification Paths

| Input | Processing | Output |
| --- | --- | --- |
| Security scan/log results | n8n status classification | Telegram notification |
| Backup/verification results | n8n success, warning or failure classification | Telegram notification |

Workflow architecture is documented without exporting complete n8n workflows,
credentials, identifiers or message contents. No recent end-to-end delivery
test is asserted by this snapshot.

## Scheduled Backup Operations

The daily backup runs at 04:00; verification follows at 05:00 in server-local
time. Verification includes temporary extraction and cleanup rather than
restoring services in place.

See [backup and recovery](backup-and-recovery.md) for destination, retention
and database-consistency boundaries.

## Troubleshooting Approach

The architecture provides a practical investigation path:

| Symptom | Relevant evidence to check |
| --- | --- |
| Service unavailable | Container/service state, logs, listener and proxy path |
| DNS or VPN connectivity failure | Service state, network membership, host rules and upstream routing |
| Missing metrics | Exporter status, scrape results and recent data |
| Backup warning/failure | Job result, destination mount state, archive checks and copy hashes |
| Missing notification | Source result, n8n execution status and notification delivery |

This is an investigation map, not a record of completed tests. Container logs,
host service logs and metrics need to be correlated before assigning a cause.

## Maintenance Evidence

The repository records the operational design rather than retaining raw logs or
execution histories. Recent job success, monitoring health, pending updates and
notification delivery remain runtime facts to check on the server when needed.
Checks requiring elevated privileges should be performed manually by the owner.
