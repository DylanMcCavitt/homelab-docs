# Runbook: Backup and Restore

## Backup standard

- Backup after every significant config change
- Store backups in private repo-local vault: `private/raw-backups/<system>/`
- Move artifacts out of `[private-file-redacted]` after capture
- Keep at least 3 historical points per critical system
- Keep a checksum manifest per system (`SHA256SUMS.txt`)

## Restore drill cadence

- Monthly spot test for one component
- Quarterly full-path restore simulation

## Restore steps template

1. Identify target backup and timestamp.
2. Validate integrity/checksum where available.
3. Restore in maintenance window (or lab simulation).
4. Validate core connectivity and policy behavior.
5. Record results and remediation notes.
