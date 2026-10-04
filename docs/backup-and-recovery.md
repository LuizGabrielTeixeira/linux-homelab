# Backup and Recovery

Daily automation archives the main homelab directory and maintains three local
copies. A separate verification job tests the archive, extracts it temporarily
and compares hashes across destinations.

See the [backup flow](../diagrams/backup-flow.md).

## Schedule and Destinations

| Server-local schedule | Job | Purpose |
| --- | --- | --- |
| Daily, 04:00 | `backup.sh` | Create a compressed archive of the main homelab directory |
| Daily, 05:00 | `verify_backup.sh` | Validate the latest backup and its copies |

| Destination | Retention | Write prerequisite |
| --- | --- | --- |
| Local backup directory | Approximately seven days | Local destination available |
| USB backup target A | Approximately seven days | Removable destination mounted |
| USB backup target B | Approximately seven days | Removable destination mounted |

The backup script checks that removable destinations are mounted before writing.
This avoids treating an unmounted USB mount point as the intended backup target.

All three copies are local; there is no off-site destination.

## Data Boundary

The archive covers `~/homelab/`, including bind-mounted state for Nginx Proxy
Manager, Vaultwarden, n8n, Obsidian / WebDAV, WireGuard configuration, Grafana,
the PostgreSQL data directory, Filebrowser's main database, Uptime Kuma,
CrowdSec, OpenVPN, AdGuard Home and Portainer.

Named-volume state outside that directory includes Prometheus historical
time-series data, Netdata cache/state and Filebrowser's `/config` volume
(a small `settings.json`). The Filebrowser main database is included; the
separate configuration volume is not. This is a directory backup, not a host image.

## Automated Verification

The verification process:

1. Locates the latest backup.
2. Performs gzip/tar integrity checks.
3. Extracts the archive into a temporary directory.
4. Checks the expected directory structure.
5. Compares the local archive hash with the USB copies.
6. Cleans up the temporary extraction directory.

Results integrate with n8n and Telegram notifications. This provides automated
integrity verification and an extraction-based restore simulation. Hash
comparison confirms that the destination copies match the local archive.

## PostgreSQL Limitation

The PostgreSQL data directory is archived while PostgreSQL is running. It is a
filesystem-level copy of live database files, not an application-consistent
database backup. Archive verification does not validate PostgreSQL recovery.

## Recovery Scope

Verification tests the archive and extracted directory structure. Restoring
services and checking application behavior remain separate from this automated
process; a full disaster recovery test has not been performed as part of it.
