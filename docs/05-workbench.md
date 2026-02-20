---
type: dashboard
area: operations
status: active
updated: 2026-02-20
tags:
  - area/operations
  - type/workbench
icon: "LiClipboardList"
---
# 05 Workbench

Use this note for every focused build session.

Navigation:
- [Start Here](00-start-here.md)
- [Operations](03-operations.md)
- [Kanban](06-kanban.md)

## Session Kickoff (5 min)
1. Read [Current Status](01-current-status.md).
2. Confirm today's objective in [Roadmap](04-roadmap.md).
3. Open the active runbook in [Operations](03-operations.md).
4. Confirm rollback path and latest backup before changes.

## Execution Rhythm
1. Apply one scoped change.
2. Validate immediately (IP, gateway, DNS, internet, intended access control).
3. Log results in [Change Log](operations/changelog.md).
4. Move to next change only after passing validation.

## Validation Shortcuts
```bash
obsidian vault=homelab unresolved total
obsidian vault=homelab orphans total
obsidian vault=homelab deadends total
```

## Current Priorities
- Execute hardening closeout run (`MGMT_GUI_PORTS` cleanup + full validation matrix).
- Remove or mark the legacy UniFi `Default` network object when dependency checks are clear.
- Prepare Bitwarden baseline and begin staged admin credential rotation plan.
- Define first production service rollout target on VLAN20 with explicit access policy.

## Session Closeout
1. Update [Current Status](01-current-status.md) and [Roadmap](04-roadmap.md).
2. Add final entry to [Change Log](operations/changelog.md).
3. Run [Publish Checklist](operations/publish-checklist.md) before public sync.
