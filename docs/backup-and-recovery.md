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
The snapshot does not establish an exact error-handling policy for each failure.

These are multiple local copies. No off-site backup is claimed.

## Data Boundary

The archive covers the main directory, represented as `~/homelab/`. Most
important service state is bind-mounted there, including proxy data/certificates,
application state, database files and VPN configuration.

Prometheus time-series data and Netdata cache/state use Docker named volumes
outside this backup boundary. The process does not back up every Docker volume
or constitute a complete host image.

## Automated Verification

The verification process:

1. Locates the latest backup.
2. Performs gzip/tar integrity checks.
3. Extracts the archive into a temporary directory.
4. Checks the expected directory structure.
5. Compares the local archive hash with the USB copies.
6. Cleans up the temporary extraction directory.

Results integrate with n8n and Telegram notifications. This is automated
integrity verification and an extraction-based restore simulation. Hash
comparison checks copy equality; it does not establish application consistency.

## PostgreSQL Limitation

The PostgreSQL data directory is archived while PostgreSQL is running. This is
a filesystem-level copy of live database files, not a database-consistent
PostgreSQL backup.

A successful archive check or extraction does not prove that PostgreSQL can
recover from those files. No application-level database restore validation is
documented in the supplied evidence.

## Recovery Claims

| Supported description | Not established |
| --- | --- |
| Automated daily filesystem backup | Complete host disaster recovery |
| Multiple local copies with retention | Off-site protection |
| Archive integrity and extraction checks | Successful application startup after restoration |
| Hash comparison of copies | Database-consistent PostgreSQL recovery |

The one-hour schedule gap does not itself prove that backup creation always
finishes before verification starts. Recent run results, duration and any
application-level recovery test should be checked manually before adding
stronger recovery claims.
