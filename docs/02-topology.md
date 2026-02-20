---
type: guide
area: network
status: active
updated: 2026-02-19
tags:
  - area/network
  - type/guide
icon: "LiNetwork"
---
# 02 Topology

Navigation:
- [Start Here](00-start-here.md)
- [Workbench](05-workbench.md)

## Diagrams
- [Physical Network Topology](topology/physical-network.md)
- [Service Topology + Trust Boundaries](topology/service-topology.md)

## VLAN Reference
- [VLAN Matrix](topology/vlan-matrix.md)

## Working Context
- Start from [Current Status](01-current-status.md) before topology changes.
- Use [Change Validation Runbook](operations/runbooks/change-validation.md) after each network change.

## Device Roles
- Protectli + OPNsense: edge routing, firewall policy, DHCP/DNS
- MikroTik CSS: VLAN trunk/access switching
- UniFi U6 Pro: SSID to VLAN mapping
