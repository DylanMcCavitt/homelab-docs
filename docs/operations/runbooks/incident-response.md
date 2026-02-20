---
type: runbook
area: operations
status: active
updated: 2026-02-19
tags:
  - area/operations
  - type/runbook
---
# Runbook: Incident Response

Escalation trail:
- [Change Log](../changelog.md)
- [Redaction Policy](../redaction-policy.md)

## Trigger conditions

- Unexpected outage
- Security anomaly
- Misconfiguration causing segmentation failure

## Immediate actions

1. Stabilize: isolate impacted segment/service.
2. Preserve evidence: export relevant logs/snapshots.
3. Triage: scope impact and probable root cause.

## Recovery

- Roll back known-bad changes
- Validate restored service health and policy
- Document timeline, root cause, and prevention actions
