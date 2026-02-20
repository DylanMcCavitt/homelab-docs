---
type: runbook
area: services
status: active
updated: 2026-02-20
tags:
  - area/services
  - type/runbook
---
# Runbook: Reverse Proxy + TLS Baseline

Use this runbook to establish a shared ingress platform before publishing individual services.

## Goal

1. Define one stable ingress pattern for homelab services.
2. Standardize DNS hostname model and certificate trust model.
3. Avoid per-service temporary domains and rework.

## Scope

- This is a platform step, not a single-service deployment.
- Complete this once, then add service routes incrementally.

## Preflight

1. Decide proxy host placement (`VLAN20` workload host or dedicated ingress VM).
2. Decide DNS model (for example `*.[redacted-hostname]` host overrides).
3. Decide cert trust model:
   - internal CA trusted on admin/client devices, or
   - public CA/ACME if public DNS/domain is available.
4. Confirm firewall policy for inbound HTTPS from `VLAN10`/`VLAN30` only.

## Baseline Deliverables

1. Reverse proxy runtime deployed and persistent.
2. DNS record for first service hostname resolves correctly.
3. TLS endpoint reachable with expected trust behavior.
4. Policy validation: trusted segments allowed, guest/iot blocked.

## Validation

1. Trusted client can open test HTTPS route.
2. Guest/IoT cannot reach management or restricted ingress paths.
3. Service route updates do not require changing hostname strategy.

## Rollback Trigger

- If TLS routing or DNS path breaks access to active admin services, revert to prior proxy config snapshot and re-validate policy matrix.

## References

- [Operations](../../03-operations.md)
- [Vaultwarden Baseline Rollout](vaultwarden-baseline.md)
