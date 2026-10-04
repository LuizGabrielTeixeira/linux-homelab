# Backup and Verification Flow

```mermaid
flowchart TD
    Data[Main homelab directory] --> Backup[04:00 daily: backup.sh]
    Backup --> Archive[Compressed filesystem archive]
    Archive --> Local[Local backup directory]
    Archive --> USB1[USB backup target A: mount checked]
    Archive --> USB2[USB backup target B: mount checked]
    Local --> Retention[Approx. seven-day retention per destination]
    USB1 --> Retention
    USB2 --> Retention
    Retention --> Verify[05:00 daily: verify_backup.sh]
    Verify --> Integrity[gzip / tar integrity test]
    Verify --> Extract[Temporary extraction and structure check]
    Verify --> Hash[Local / USB archive hash comparison]
    Integrity --> Result[Verification result and temporary cleanup]
    Extract --> Result
    Hash --> Result
    Result --> Automation[n8n: status classification]
    Automation --> Telegram[Telegram notification]
```

The schedule is server-local time. Retention is a destination policy; verification
selects the latest archive. Copies remain local. Verification includes an
extraction-based restore simulation; application recovery is a separate step.

See [backup and recovery](../docs/backup-and-recovery.md).
