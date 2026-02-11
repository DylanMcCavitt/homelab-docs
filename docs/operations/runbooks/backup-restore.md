# Runbook: Backup and Restore

## Backup standard

- Backup after every significant config change
- Store encrypted backups in private location
- Keep at least 3 historical points per critical system

## Restore drill cadence

- Monthly spot test for one component
- Quarterly full-path restore simulation

## Restore steps template

1. Identify target backup and timestamp.
2. Validate integrity/checksum where available.
3. Restore in maintenance window (or lab simulation).
4. Validate core connectivity and policy behavior.
5. Record results and remediation notes.
