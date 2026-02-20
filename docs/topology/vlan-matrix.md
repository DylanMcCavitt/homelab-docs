---
type: reference
area: network
status: active
updated: 2026-02-19
tags:
  - area/network
  - type/reference
---
# VLAN Matrix (Sanitized)

Use aliases in public docs. Keep real subnet/IP details private.

| VLAN Alias   | VLAN ID | Purpose                      | DHCP           | Internet | Lateral Access |
| ------------ | ------- | ---------------------------- | -------------- | -------- | -------------- |
| VLAN_MGMT    | 30      | Network and infra management | Yes            | Yes      | Restricted     |
| VLAN_SERVERS | 20      | Hypervisor, core services    | Yes/Static mix | Yes      | Controlled     |
| VLAN_TRUSTED | 10      | Trusted user devices         | Yes            | Yes      | Limited        |
| VLAN_IOT     | 50      | IoT and untrusted devices    | Yes            | Yes      | Deny to MGMT   |
| VLAN_GUEST   | 40      | Guest wireless               | Yes            | Yes      | Internet-only  |

## Policy intent

- Default deny between VLANs
- Explicit allow rules per service dependency
- Management plane isolated and minimally exposed

See also:
- [Topology](../02-topology.md)
- [UniFi + Proxmox Strict Runbook](../operations/runbooks/unifi-proxmox-standard-strict.md)
