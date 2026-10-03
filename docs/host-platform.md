# Host Platform

The host, represented here as `home-server`, runs Ubuntu Server 24.04 LTS.
Docker services share this operating system with native administration,
scheduling and security components.

## Platform Snapshot

| Component | Documented role |
| --- | --- |
| Ubuntu Server 24.04 LTS | Linux operating system |
| systemd | Host service management |
| Docker Engine / Compose | Container runtime and project management |
| SSH | Remote administration |
| UFW | Host firewall policy |
| cron | Scheduled operational jobs |
| Unattended upgrades | Automated package update mechanism |
| NTP / time synchronization | Host time synchronization |

Exact package versions, hardware identifiers and update history are not part of
this snapshot. The presence of unattended upgrades does not establish that every
update has succeeded or that no reboot is pending.

## Host and Container Responsibilities

systemd manages native services; Compose projects organize container workloads.
cron schedules the daily filesystem backup and its subsequent verification.
Time synchronization supports interpreting logs, metrics and scheduled activity.
Backup times in this repository use server-local time without publishing a
specific timezone.

AdGuard Home and Netdata are containers using host networking. They remain
container-managed services even though they share the host network namespace.

## Administration and Maintenance

The administration surface includes SSH, service state, logs, storage mounts,
container lifecycle and package maintenance. Troubleshooting correlates these
layers instead of assuming every failure originates inside a container.

This repository records architecture and operational boundaries; it does not
include server configuration files, raw logs or authentication material.

## Related Documentation

- [Security](security.md): effective SSH configuration and security controls.
- [Networking](networking.md): UFW and Docker port publishing.
- [Operations](operations.md): schedules, notifications and troubleshooting.
