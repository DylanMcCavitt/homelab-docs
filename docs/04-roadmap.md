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
- Complete hardening closeout run (legacy UniFi `Default` object decision + full validation matrix).
- Deliver reverse proxy/TLS baseline as a dedicated platform step (not per-service one-offs).
- Publish Vaultwarden from bootstrap mode to stable HTTPS LAN hostname and remove tunnel dependency.
- Capture Vaultwarden backup/restore checkpoint, then begin staged credential rotation.

Execution note:
- Run active session tasks from [Workbench](05-workbench.md).

## Next
- Define service placement for Grafana and Jellyfin after ingress baseline is stable.
- Stand up Tailscale for VPN-first remote admin baseline.
- Continue credential rotation waves after Vaultwarden HTTPS + restore checkpoint sign-off.

## Later
- Integrate DIY NAS for media and backups.
- Add VPN-first remote admin (Tailscale/WireGuard).
- Formalize observability dashboards and backup restore drills.
