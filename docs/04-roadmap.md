---
type: roadmap
area: homelab
status: active
updated: 2026-02-17
---
# 04 Roadmap

## Now
- Finalize management GUI access policy (trusted sources only).
- Complete wired cutover from temporary test ports to permanent roles.
- Clean temporary firewall rules and confirm intended isolation.
- Replace temporary admin-host allow with static trusted-device mappings.
- Export and catalog OPNsense + Proxmox backup artifacts after current policy cutover.

## Tomorrow (2026-02-18)
- Policy hardening session: finalize VLAN10 -> VLAN30 management access rules and validate trusted/untrusted matrix.
- Confirm OPNsense GUI listen interfaces are narrowed from transition-safe settings to intended steady-state.
- Log validation evidence and residual risk in changelog.

## Next
- Decide final Proxmox host management placement (stay VLAN10 trusted vs move to VLAN30 infra management).
- Migrate UniFi controller off Mac to dedicated host.
- Define service placement for reverse proxy, Grafana, Jellyfin.
- Stand up secrets baseline with Bitwarden (cloud first), then rotate admin credentials.

## Later
- Integrate DIY NAS for media and backups.
- Add VPN-first remote admin (Tailscale/WireGuard).
- Evaluate self-hosted Vaultwarden after core network and backup posture are stable.
- Formalize observability dashboards and backup restore drills.
