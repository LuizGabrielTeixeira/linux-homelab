# Host Platform

The host, represented here as `home-server`, runs Ubuntu Server 24.04 LTS.
Docker services share this operating system with native administration,
scheduling and security components.

## Host Services

| Component | Role |
| --- | --- |
| Ubuntu Server 24.04 LTS | Linux operating system |
| systemd | Host service management |
| Docker Engine / Compose | Container runtime and project management |
| SSH | Remote administration |
| UFW | Host firewall policy |
| cron | Scheduled operational jobs |
| Unattended upgrades | Automated package update mechanism |
| NTP / time synchronization | Host time synchronization |

## Host and Container Responsibilities

systemd manages native services; Compose projects organize container workloads.
cron schedules the daily filesystem backup and its subsequent verification.
Time synchronization supports interpreting logs, metrics and scheduled activity.
Backup times use server-local time.

AdGuard Home and Netdata are containers using host networking. They remain
container-managed services even though they share the host network namespace.

## Administration and Maintenance

Administration covers SSH access, service state, logs, storage mounts,
container lifecycle and package maintenance. Troubleshooting correlates these
layers to distinguish host, container and network problems. Unattended upgrades
handle automated updates; update results and pending reboots remain maintenance
items to check.

## Related Documentation

- [Security](security.md): effective SSH configuration and security controls.
- [Networking](networking.md): UFW and Docker port publishing.
- [Operations](operations.md): schedules, notifications and troubleshooting.
