---
type: runbook
area: operations
status: active
updated: 2026-02-19
tags:
  - area/operations
  - type/runbook
---
# Runbook: Change Validation

Use alongside:
- [Backup and Restore Runbook](backup-restore.md)
- [Change Log](../changelog.md)

## Pre-check

- Confirm backup exists and is restorable
- Define pass/fail checks
- Define rollback command path

## Execute

1. Apply change in smallest step possible.
2. Run connectivity and service checks.
3. Monitor logs for regressions.

## Post-check

- Capture before/after evidence
- Update `docs/operations/changelog.md`
- Mark residual risks and follow-up tasks
