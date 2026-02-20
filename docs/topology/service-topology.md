---
type: diagram
area: services
status: active
updated: 2026-02-19
---
# Service Architecture Board

![service-topology.canvas](service-topology.canvas)

Open in full canvas view: [service-topology.canvas](service-topology.canvas)

## Notes
- Clients: V10/V30 trusted paths; V40/V50 blocked to mgmt.
- Control: OPNsense, DNS/DHCP, MikroTik, UniFi OS, U6 Pro.
- Services: Proxmox -> V20 workloads -> NAS (planned), plus Bitwarden and VPN (planned).

## Related
- [Start Here](../00-start-here.md)
- [Current Status](../01-current-status.md)
- [VLAN Matrix](vlan-matrix.md)
