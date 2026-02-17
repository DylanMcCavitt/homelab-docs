---
type: status
area: homelab
status: active
updated: 2026-02-17
---
# 01 Current Status

## Snapshot
- Core VLANs are configured and routing/DHCP/DNS are working.
- SSID validation passed for VLAN10, VLAN40, VLAN50.
- Wired validation passed for VLAN20 and VLAN30 on temporary Port8.
- Proxmox host management was moved off installer subnet (`192.168.1.0/24`) to VLAN10 and GUI access is restored.
- Temporary anti-lockout firewall alias/rules are in place for the current trusted admin host.

## Validated Results
| Segment | Path | IP | Gateway | DNS | Internet | Result |
|---|---|---|---|---|---|---|
| VLAN10 Trusted | Wi-Fi SSID | `10.0.10.x` | `10.0.10.1` | `10.0.10.1` | Pass | Pass |
| VLAN40 Guest | Wi-Fi SSID | `10.0.40.x` | `10.0.40.1` | `10.0.40.1` | Pass | Pass |
| VLAN50 IoT | Wi-Fi SSID | `10.0.50.x` | `10.0.50.1` | `10.0.50.1` | Pass | Pass |
| VLAN20 Servers | Wired Port8 | `10.0.20.x` | `10.0.20.1` | `10.0.20.1` | Pass | Pass |
| VLAN30 Mgmt | Wired Port8 | `10.0.30.x` | `10.0.30.1` | `10.0.30.1` | Pass | Pass |
| Proxmox Host Mgmt | Wired Port2 (access VLAN10) | `10.0.10.x` | `10.0.10.1` | via OPNsense | GUI Reachable | Pass |

## Current Blocker
- Management policy is still in transition (temporary host-based allow and broad GUI listen settings during cutover).

## Next 3 Steps
1. Tomorrow (2026-02-18): finish alias-driven management policy hardening (`ADMIN_TRUSTED`, `MGMT_TARGETS`, `MGMT_GUI_PORTS`) with lockout-safe rule order.
2. Re-test trusted vs non-trusted GUI access matrix across VLAN10/40/50.
3. Replace temporary admin-host allow with static trusted-device mappings once DHCP reservations are in place.

## Upcoming Ops Task
- Deploy secrets management baseline with Bitwarden and rotate all admin credentials (OPNsense, MikroTik, UniFi, Proxmox).
