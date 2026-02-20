---
type: runbook
area: services
status: active
updated: 2026-02-20
tags:
  - area/services
  - type/runbook
---
# Runbook: Vaultwarden Baseline Rollout

Use this runbook to deploy and harden Vaultwarden as the first production service on `VLAN20`.

## Objectives

1. Stand up Vaultwarden with persistent storage and least-privilege network exposure.
2. Publish access through reverse proxy + TLS (no direct WAN exposure of container port).
3. Validate login/sync behavior and segmentation policy expectations.
4. Capture backup + restore checkpoint before broad credential rotation.

## Preconditions (Do Not Skip)

1. Segmentation baseline is validated and stable.
2. OPNsense + Proxmox rollback backups are current.
3. Vaultwarden workload host placement on `VLAN20` is decided (VM/LXC + container runtime).
4. DNS and TLS ingress path are defined (prefer shared baseline from `reverse-proxy-tls-baseline`).
5. Secrets handling plan exists for admin token and any SMTP or notification credentials.

Stop condition:
- If any precondition is unknown, stop and resolve it before deploy commands.

## Service Card (Fill Before Deploy)

- Name: Vaultwarden
- Purpose: Secrets baseline for staged admin credential rotation
- Dependencies: Proxmox, VLAN20 routing, reverse proxy/TLS, backup target
- VLAN placement: `VLAN20`
- Inbound path: reverse proxy over HTTPS
- Auth model: vault master password + 2FA policy as enabled
- Backup method: persistent `/data` backup + restore checkpoint
- Rollback trigger: failed login/sync path after deployment or update

## Phase 1: Host and Runtime Baseline

1. Prepare workload host on `VLAN20`.
2. Confirm host can resolve DNS and reach required update registries.
3. Ensure storage path for Vaultwarden `/data` is persistent and included in backup scope.

Validation:
- Host IP/gateway/DNS/internet checks pass on `VLAN20`.
- Persistent data path is writable and survives restart.

## Phase 2: Vaultwarden Container Baseline

1. Deploy Vaultwarden with a pinned stable image tag.
2. Mount persistent `/data`.
3. Bind service internally (localhost or service network path) for reverse proxy handoff.
4. Configure required environment values per upstream docs.

Validation:
- Container is healthy and restarts cleanly.
- Vaultwarden is reachable through intended internal path.

## Phase 3: Ingress and TLS

If ingress platform is not already standardized, complete `reverse-proxy-tls-baseline` first.

1. Publish Vaultwarden through reverse proxy with HTTPS.
2. Enforce TLS and expected hostname.
3. Keep admin/control-plane paths restricted to trusted/mgmt clients.

Validation:
- Trusted client can load web vault over HTTPS and complete sign-in.
- Guest/IoT paths cannot reach management endpoints.

## Phase 4: Security Baseline

1. Set strong admin and service secrets (stored outside repo/docs).
2. Enable only required features for initial rollout.
3. Restrict any admin interface exposure to management paths.

Validation:
- Admin access works only from intended paths.
- No plain-text secrets are committed or documented in repo.

## Phase 5: Backup and Restore Checkpoint

1. Capture backup of Vaultwarden persistent data.
2. Perform a restore test to a checkpoint environment or equivalent safe target.
3. Record restore outcome before beginning mass credential rotation.

Validation:
- Backup artifact exists in approved private location.
- Restore test demonstrates expected recovery behavior.

## Phase 6: Post-Deploy Verification Matrix

1. Trusted client:
   - HTTPS load and login pass
   - vault sync pass
2. Untrusted segments (guest/iot):
   - management path denied per policy
3. Operations:
   - service restart preserves data
   - backup job trigger/flow passes

## Rollback Plan

1. Trigger:
   - Login/sync failure after change
   - TLS ingress failure
   - policy regression exposing unintended access
2. Steps:
   - Revert proxy or service change
   - restore previous Vaultwarden data snapshot if needed
   - re-validate trusted and untrusted access matrix

## References

- [Operations](../../03-operations.md)
- [Roadmap](../../04-roadmap.md)
- [Reverse Proxy + TLS Baseline](reverse-proxy-tls-baseline.md)
- https://github.com/dani-garcia/vaultwarden?tab=readme-ov-file
- https://github.com/dani-garcia/vaultwarden/wiki
