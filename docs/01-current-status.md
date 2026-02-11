---
type: status
area: homelab
status: active
updated: 2026-02-11
---
# 01 Current Status

## Snapshot
- Core VLANs are configured and routing/DHCP/DNS are working.
- SSID validation passed for VLAN10, VLAN40, VLAN50.
- Wired validation passed for VLAN20 and VLAN30 on temporary Port8.
- OPNsense GUI policy still needs trusted-admin access cleanup.

## Validated Results
| Segment | Path | IP | Gateway | DNS | Internet | Result |
|---|---|---|---|---|---|---|
| VLAN10 Trusted | Wi-Fi SSID | `10.0.10.x` | `10.0.10.1` | `10.0.10.1` | Pass | Pass |
| VLAN40 Guest | Wi-Fi SSID | `10.0.40.x` | `10.0.40.1` | `10.0.40.1` | Pass | Pass |
| VLAN50 IoT | Wi-Fi SSID | `10.0.50.x` | `10.0.50.1` | `10.0.50.1` | Pass | Pass |
| VLAN20 Servers | Wired Port8 | `10.0.20.125` | `10.0.20.1` | `10.0.20.1` | Pass | Pass |
| VLAN30 Mgmt | Wired Port8 | `10.0.30.125` | `10.0.30.1` | `10.0.30.1` | Pass | Pass |

## Current Blocker
- OPNsense GUI is still blocked from your intended trusted-admin path in VLAN30.

## Next 3 Steps
1. Fix GUI access policy in OPNsense (`ADMIN_TRUSTED_NETS` alias + explicit allow to `This Firewall:443`).
2. Re-test trusted vs non-trusted GUI access matrix.
3. Export backups (OPNsense, UniFi, SwOS evidence) and log in changelog.
