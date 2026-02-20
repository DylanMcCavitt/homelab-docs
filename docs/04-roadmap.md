---
type: roadmap
area: homelab
status: active
updated: 2026-02-20
tags:
  - area/planning
  - type/roadmap
icon: "LiRoute"
---
# 04 Roadmap

Navigation:
- [Start Here](00-start-here.md)
- [Workbench](05-workbench.md)

## Now
- Execute hardening closeout run (`MGMT_GUI_PORTS` cleanup, legacy UniFi `Default` object decision, full validation matrix).
- Stand up Bitwarden baseline and begin staged credential rotation.
- Define first production service placement on VLAN20 with explicit allow-list policy.
- Review management GUI port alias and remove legacy `8443` if no longer required.
- Remove or clearly mark the legacy UniFi `Default` network object if no dependencies remain.

Execution note:
- Run active session tasks from [Workbench](05-workbench.md).

## Next
- Define service placement for reverse proxy, Grafana, Jellyfin.
- Stand up Tailscale for VPN-first remote admin baseline.
- Stand up secrets baseline with Bitwarden (cloud first), then rotate admin credentials.

## Later
- Integrate DIY NAS for media and backups.
- Add VPN-first remote admin (Tailscale/WireGuard).
- Evaluate self-hosted Vaultwarden after core network and backup posture are stable.
- Formalize observability dashboards and backup restore drills.
