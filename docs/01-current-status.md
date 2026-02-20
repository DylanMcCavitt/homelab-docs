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
- Core VLANs are configured and routing/DHCP/DNS are working.
- OPNsense policy hardening is applied for VLAN10, VLAN40, VLAN50 with expected allow/deny behavior.
- SSID validation passed for VLAN10, VLAN40, VLAN50.
- Wired validation passed for VLAN20 and VLAN30 on temporary Port8.
- Proxmox host management migration is in-progress and stable:
  - legacy management path still reachable on VLAN10
  - primary gateway moved to VLAN30 (`vmbr0.30`)
- UniFi OS Server is running on a Proxmox VM in VLAN30 and is reachable from trusted admin clients.
- UniFi backup restore completed; U6 Pro is online in the new controller.
- Old Mac-hosted UniFi controller process was stopped to remove dual-controller conflict.
- AP management is now on VLAN30 (`10.0.30.172`) and stable in the UniFi controller.
- Temporary LAN AP-to-controller exceptions were removed after post-migration validation.
- VLAN10 admin access rules now use `ADMIN_TRUSTED_HOSTS` instead of the temporary host alias.
- OPNsense aliases and firewall rules were categorized for faster filtering and audit.
- OPNsense Web GUI listen scope is now restricted to `VLAN10_Trusted` and `VLAN30_Mgmt`.
- Legacy GUI path on `192.168.1.1` is no longer reachable by design.
- MikroTik switch management is now static on VLAN30 (`10.0.30.82`) and reachable.
- Temporary `lab-temp` SSID was removed after permanent SSID validation (Trusted/Guest/IoT).
- `MGMT_TARGETS` alias cleanup removed duplicate switch entry (`10.0.10.82`); VLAN30 target remains (`10.0.30.82`).
- Legacy LAN was converted to `LAN_BREAKGLASS` on `10.255.255.1/24` with explicit deny-all policy.
- Dnsmasq DHCP/DNS interface binding no longer includes legacy LAN; only VLAN interfaces are active.
- Backup artifacts were moved from `[private-file-redacted]` to repo-local private vault (`private/raw-backups/`).
- Hardening closeout preflight passed for next OPNsense policy edits:
  - admin source/VLAN confirmed on trusted path
  - GUI listen scope/port confirmed
  - fallback console path confirmed
  - fresh rollback backup confirmed
  - management target aliases reviewed
- Fresh OPNsense backup export `[redacted-hostname]-20260220014906.xml` was moved from local Downloads staging into `private/raw-backups/opnsense/` and checksum inventory was updated.
- Execution runbook is active for strict management segmentation:
  - Proxmox host management target: VLAN30
  - UniFi controller VM target: VLAN30
  - Detailed operator guide: `docs/operations/runbooks/unifi-proxmox-standard-strict.md`

## Validated Results
| Segment | Path | IP | Gateway | DNS | Internet | Result |
|---|---|---|---|---|---|---|
| VLAN10 Trusted | Wi-Fi SSID | `10.0.10.x` | `10.0.10.1` | `10.0.10.1` | Pass | Pass |
| VLAN40 Guest | Wi-Fi SSID | `10.0.40.x` | `10.0.40.1` | `10.0.40.1` | Pass | Pass |
| VLAN50 IoT | Wi-Fi SSID | `10.0.50.x` | `10.0.50.1` | `10.0.50.1` | Pass | Pass |
| VLAN20 Servers | Wired Port8 | `10.0.20.x` | `10.0.20.1` | `10.0.20.1` | Pass | Pass |
| VLAN30 Mgmt | Wired Port8 | `10.0.30.x` | `10.0.30.1` | `10.0.30.1` | Pass | Pass |
| Proxmox Host Mgmt | Wired Port2 (trunk transition) | `10.0.10.x` + `10.0.30.x` | primary `10.0.30.1` | via OPNsense | GUI Reachable | Pass |
| UniFi OS Server | VM in VLAN30 | `10.0.30.x` | `10.0.30.1` | `10.0.30.1` | GUI Reachable (`:11443`) | Pass |
| U6 Pro Controller State | AP mgmt on VLAN30 | `10.0.30.172` | `10.0.30.1` | via OPNsense | Online in new controller | Pass |
| MikroTik Switch Mgmt | SwOS static mgmt | `10.0.30.82` | n/a (SwOS reply model) | n/a | GUI Reachable | Pass |
| VLAN40 Isolation | Wi-Fi SSID | `10.0.40.x` | `10.0.40.1` | `10.0.40.1` | Pass | RFC1918/MGMT blocked |
| VLAN50 Isolation | Wi-Fi SSID | `10.0.50.x` | `10.0.50.1` | `10.0.50.1` | Pass | RFC1918/MGMT blocked |

## Current Blocker
- No hard blocker. Remaining work is service-readiness and secrets rollout.

## Next 3 Steps
1. Execute hardening closeout: remove legacy `8443` from `MGMT_GUI_PORTS` after dependency check.
2. Remove or mark the legacy UniFi `Default` network object after dependency audit.
3. Stand up Bitwarden baseline, then start staged admin credential rotation.

## Upcoming Ops Task
- 2026-02-20 priority: execute hardening closeout run phases, then start service rollout planning.
- After hardening validation passes, deploy secrets management baseline with Bitwarden and rotate admin credentials (OPNsense, MikroTik, UniFi, Proxmox).

## Session Entry Point
- Use [Workbench](05-workbench.md) before applying changes.
