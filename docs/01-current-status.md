---
type: status
area: homelab
status: active
updated: 2026-02-20
tags:
  - area/status
  - type/status
icon: "LiGauge"
---
# 01 Current Status

Navigation:
- [Start Here](00-start-here.md)
- [Workbench](05-workbench.md)

## Snapshot
- Core VLAN segmentation is stable; routing, DHCP, and DNS are working on VLAN10/20/30/40/50.
- Management plane is hardened: OPNsense GUI listens only on `VLAN10_Trusted` and `VLAN30_Mgmt`.
- Primary management targets are stable on VLAN30 (Proxmox host, UniFi OS Server, MikroTik switch, U6 Pro AP management).
- Vaultwarden runtime VM (`Debian 13`) is provisioned on `VLAN20` with Docker runtime and persistent data path.
- Vaultwarden baseline is deployed (`vaultwarden/server:1.35.3`); owner account exists and `SIGNUPS_ALLOWED` is set to `false`.
- Current Vaultwarden access is bootstrap-only through local/tunneled path; stable LAN HTTPS hostname is not published yet.
- Legacy LAN is now `LAN_BREAKGLASS` with deny-all policy; legacy LAN DHCP/DNS bindings are removed.

## Validated Results
- Trusted and management admin paths are reachable for OPNsense, Proxmox, UniFi OS, and MikroTik.
- VLAN40 and VLAN50 isolation is intact (internet pass, RFC1918/management blocked).
- VLAN20 server-path baseline remains healthy with no expected management access regressions.
- Vaultwarden local health check passes (`HTTP 200` on VM localhost path) and initial owner login completed.

## Current Blocker
- Vaultwarden ingress is not finalized: access still depends on SSH tunnel/bootstrap path and shows non-production trust warnings.
- Remaining closeout item: UniFi `Default` network object dependency decision (remove or mark legacy).

## Next 3 Steps
1. Deliver reverse proxy/TLS baseline as a dedicated platform step (hostname strategy, DNS, cert trust model).
2. Publish Vaultwarden on stable LAN HTTPS path (no tunnel) and validate VLAN10/30 allow + VLAN40/50 deny.
3. Capture Vaultwarden backup + restore checkpoint, then begin staged admin credential rotation.

## Session Entry Point
- Use [Workbench](05-workbench.md) before applying changes.
