---
type: board
area: operations
status: active
updated: 2026-02-20
tags:
  - area/operations
  - type/kanban
icon: "LiKanbanSquare"
kanban-plugin: board
---
# 06 Kanban

## Backlog
- [ ] Build VLAN20 service deployment template (policy, backup, monitoring)

## Today
- [ ] Execute hardening closeout run (alias cleanup, dependency checks, validation matrix)
- [ ] Remove or mark legacy UniFi `Default` network object after dependency check
- [ ] Stand up Bitwarden baseline and draft staged credential-rotation checklist
- [ ] Define first production service on VLAN20 after network hardening

## In Progress
- [ ] Refine vault UX and navigation for faster daily execution

## Blocked
- [ ] Finalize secrets rotation schedule after Bitwarden baseline confirmation

## Done
- [x] Hardening closeout preflight completed (admin source, GUI listen scope, fallback path, backup, target aliases)
- [x] Moved fresh OPNsense backup from local Downloads staging into private backup vault and updated checksum manifest
- [x] Proxmox host management path active on VLAN30
- [x] UniFi OS Server moved to Proxmox VM and AP re-adopted
- [x] AP management migrated from legacy LAN to VLAN30
- [x] Temporary LAN AP-to-controller exceptions removed after validation
- [x] Admin rules switched from temp host alias to `ADMIN_TRUSTED_HOSTS`
- [x] OPNsense aliases and firewall rules categorized for filtering and audits
- [x] OPNsense GUI listen scope restricted to VLAN10 and VLAN30 only
- [x] Removed unused transitional aliases and exported post-hardening snapshots
- [x] MikroTik switch management moved to VLAN30 static IP (`10.0.30.82`)
- [x] Removed temporary `lab-temp` SSID after Trusted/Guest/IoT validation
- [x] Archived latest OPNsense and MikroTik backups with checksum updates
- [x] Removed duplicate switch management target from `MGMT_TARGETS` alias (`10.0.10.82`)
- [x] Converted legacy LAN to `LAN_BREAKGLASS` (`10.255.255.1/24`) with deny-all rule
- [x] Removed Dnsmasq legacy LAN interface binding and LAN DHCP scope
- [x] Archived fresh OPNsense + MikroTik backups after breakglass conversion

## Related
- [Start Here](00-start-here.md)
- [Workbench](05-workbench.md)
- [Roadmap](04-roadmap.md)
