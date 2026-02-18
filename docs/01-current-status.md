---
type: status
area: homelab
status: active
updated: 2026-02-18
---
# 01 Current Status

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
- AP management remains on transitional legacy LAN IP (`192.168.1.x`) pending next-session move to VLAN30.
- Temporary anti-lockout admin-host rule is still in place pending static trusted-client mappings.
- Backup artifacts were moved from `[private-file-redacted]` to repo-local private vault (`private/raw-backups/`).
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
| U6 Pro Controller State | AP mgmt on legacy LAN (transition) | `192.168.1.x` | legacy LAN | legacy LAN | Online in new controller | Pass |
| VLAN40 Isolation | Wi-Fi SSID | `10.0.40.x` | `10.0.40.1` | `10.0.40.1` | Pass | RFC1918/MGMT blocked |
| VLAN50 Isolation | Wi-Fi SSID | `10.0.50.x` | `10.0.50.1` | `10.0.50.1` | Pass | RFC1918/MGMT blocked |

## Current Blocker
- No hard blocker. Remaining work is transition cleanup and management-IP finalization.

## Next 3 Steps
1. Move AP management from legacy LAN to VLAN30, then remove temporary LAN control-plane exceptions.
2. Replace temporary admin-host allow with static trusted-device mappings after migration stabilization.
3. Tighten OPNsense GUI/listen scope to intended steady-state interfaces only.

## Upcoming Ops Task
- 2026-02-19 priority: AP management migration to VLAN30 and firewall cleanup.
- Deploy secrets management baseline with Bitwarden and rotate all admin credentials (OPNsense, MikroTik, UniFi, Proxmox).
