# VLAN Matrix (Sanitized)

Use aliases in public docs. Keep real subnet/IP details private.

| VLAN Alias   | VLAN ID | Purpose                      | DHCP           | Internet | Lateral Access |
| ------------ | ------- | ---------------------------- | -------------- | -------- | -------------- |
| VLAN_MGMT    | TODO    | Network and infra management | Yes            | Yes      | Restricted     |
| VLAN_SERVERS | TODO    | Hypervisor, core services    | Yes/Static mix | Yes      | Controlled     |
| VLAN_TRUSTED | 10      | Trusted user devices         | Yes            | Yes      | Limited        |
| VLAN_IOT     | 50      | IoT and untrusted devices    | Yes            | Yes      | Deny to MGMT   |
| VLAN_GUEST   | 40      | Guest wireless               | Yes            | Yes      | Internet-only  |

## Policy intent

- Default deny between VLANs
- Explicit allow rules per service dependency
- Management plane isolated and minimally exposed
