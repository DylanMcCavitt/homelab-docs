---
type: roadmap
area: homelab
status: active
updated: 2026-02-18
---
# 04 Roadmap

## Now
- Execute final cleanup of strict management migration runbook: `docs/operations/runbooks/unifi-proxmox-standard-strict.md`.
- Migrate AP management from transitional legacy LAN path to VLAN30 management path.
- Remove temporary LAN control-plane exceptions after AP is confirmed stable on VLAN30.
- Replace temporary admin-host allow with static trusted-device mappings.
- Narrow OPNsense GUI listen interfaces from transition-safe settings to intended steady-state.
- Confirm final management target inventory and clean temporary legacy entries.

## Next
- Define service placement for reverse proxy, Grafana, Jellyfin.
- Stand up Tailscale for VPN-first remote admin baseline.
- Stand up secrets baseline with Bitwarden (cloud first), then rotate admin credentials.

## Later
- Integrate DIY NAS for media and backups.
- Add VPN-first remote admin (Tailscale/WireGuard).
- Evaluate self-hosted Vaultwarden after core network and backup posture are stable.
- Formalize observability dashboards and backup restore drills.
