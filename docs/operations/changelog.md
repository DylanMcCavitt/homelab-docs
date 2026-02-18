---
type: changelog
area: operations
updated: 2026-02-18
---
# Homelab Change Log

Use this file for chronological, operator-grade change history.

## Entry format

Copy `docs/operations/change-entry-template.md` for each change.

---

## 2026-02-18 - UniFi OS Server Cutover Completed (VLAN30 Controller)

- Installed UniFi OS Server on the Proxmox-hosted management VM and confirmed console reachability on VLAN30.
- Restored controller backup in the new UniFi environment and verified SSIDs/networks came back as expected.
- Rebound U6 Pro to the new controller and confirmed steady `Online` status.
- Stopped legacy Mac-hosted UniFi controller process to avoid dual-controller conflicts.
- Re-validated client behavior on VLAN10, VLAN40, and VLAN50:
  - DHCP/gateway/DNS/internet checks passed
  - guest/IoT isolation behavior remained as expected
- Archived fresh OPNsense, UniFi, and Proxmox snapshot artifacts from `[private-file-redacted]` into `private/raw-backups/`.
- Recorded/updated private checksum manifests for newly captured artifacts.
- Set next-session priority (2026-02-19): move AP management off legacy LAN to VLAN30, then remove temporary LAN control-plane firewall exceptions.

## 2026-02-17 - Strict Segmentation Migration Runbook Implemented (Docs)

- Added operator-grade migration runbook:
  - `docs/operations/runbooks/unifi-proxmox-standard-strict.md`
- Added private concrete-value worksheet for execution:
  - `private/runbooks/unifi-proxmox-standard-strict-private.md`
- Locked target architecture decision in documentation:
  - Proxmox host management target = VLAN30
  - UniFi controller VM target = VLAN30
  - VLAN20 reserved for server workloads
- Updated status/operations/roadmap references to point to the new runbook.
- Scope note: this entry documents planning + runbook implementation only; no live network/device cutover was performed in this repo change.
- Clarified runbook rule-order guidance for current OPNsense export state:
  - when temporary admin mgmt allow already exists, do not duplicate VLAN10 admin pass rules during migration window
  - place UniFi controller-to-AP rules on VLAN30 above legacy/internal block rules
- Archived OPNsense alias/rule exports for this session in private evidence path:
  - `private/raw-backups/opnsense/exports/2026-02-17/`
- Updated UniFi migration runbooks to use Debian 13 netinst as default VM base image, with Debian 12 fallback if upstream UniFi package support lags.
- Added beginner-friendly Debian installer guidance to runbooks:
  - screen-by-screen VM installer selections
  - post-install static IP configuration path in Debian guest OS
- Added troubleshooting note for Debian installs where `sudo` is not present by default (`su -` root-shell path included in runbooks).
- Added typo-proof APT path guidance for UniFi install (`/etc/apt/sources.list.d` with dot) and explicit command block to reduce installer friction.
- Corrected UniFi VM OS baseline back to Debian 12 after live validation:
  - Debian 13 install path hit UniFi APT signature policy rejection (`SHA1 is not considered secure`) on 2026-02-17
  - runbooks now explicitly direct rebuild on Debian 12 for this controller deployment path

## 2026-02-17 - OPNsense Policy Hardening Validated + Backup Vault Cleanup

- Finalized rule ordering for VLAN10 management access controls:
  - temporary trusted-admin allow rules are evaluated before management deny
  - non-admin VLAN10 access to `MGMT_TARGETS` is blocked
- Applied and validated guest/IoT isolation policy:
  - VLAN40 and VLAN50 retain internet access
  - VLAN40 and VLAN50 are blocked from RFC1918 and management targets
- Updated management ports to restore expected access to MikroTik web UI from trusted admin host.
- Exported new OPNsense backup and moved firewall/switch/controller backup artifacts out of `[private-file-redacted]`.
- Organized backup storage into private vault paths under `private/raw-backups/` with per-system checksum manifests.
- Observed UniFi AP control-plane issue: AP continues broadcasting SSIDs, but controller reports AP offline pending controller migration/cutover.

## 2026-02-16 - Proxmox Host Config Backup Exported

- Proxmox host config backup created at: `[recorded-privately]`.
- Integrity checksum recorded privately.
- Archive size at export time: `14K`.
- Note: `tar` "Removing leading '/'" messages are expected behavior.

## 2026-02-17 - Next Session Planned

- Set next-session priority for 2026-02-18: OPNsense management policy hardening and access-matrix validation.
- Kept temporary anti-lockout controls in place until that validation session completes.

## 2026-02-14 - Proxmox Management Cutover + Firewall Safety Rules

- Moved Proxmox host management from installer subnet (`192.168.1.0/24`) to homelab trusted VLAN (`10.0.10.0/24`).
- Restored Proxmox GUI access on the new VLAN path.
- Added temporary OPNsense anti-lockout rules using a current admin host alias during rule migration.
- Kept GUI listener broad during transition to reduce lockout risk; hardening remains pending.
- OPNsense backup exported: `[recorded-privately]`.
- Integrity checksum recorded privately.
- Follow-up completed on 2026-02-16: Proxmox host backup exported and logged.

## 2026-02-14 - MikroTik Config Backup Captured

- Captured current MikroTik SwOS configuration backup before additional network changes.
- Backup intent: preserve known-good rollback point for switch VLAN/port policy state.
- Backup currently stored in `[private-file-redacted]` (move to long-term private backup location).
- Follow-up: record backup filename and checksum in private ops notes.

## 2026-02-14 - Secrets Management Planned (Bitwarden)

- Added Bitwarden as a planned service in roadmap and service topology docs.
- Recorded an upcoming ops task to rotate admin credentials after Bitwarden baseline is in place.
- Decision path documented: Bitwarden cloud first, optional Vaultwarden self-host later.

## 2026-02-10 - Documentation system initialized

- Established documentation structure under `docs/`
- Added privacy redaction policy and publish checklist
- Added templates for repeatable change, service, and runbook documentation

## 2026-02-10 - VLAN Validation Update (Wi-Fi + Wired)

- Confirmed SSID validation passes:
  - VLAN10 (`10.0.10.x`) gateway/DNS/internet pass
  - VLAN40 (`10.0.40.x`) gateway/DNS/internet pass
  - VLAN50 (`10.0.50.x`) gateway/DNS/internet pass
- Confirmed wired access-port validation on temporary Port8:
  - VLAN20 test host: `10.0.20.x`, gateway `10.0.20.1`, DNS `10.0.20.1`, internet pass
  - VLAN30 test host: `10.0.30.x`, gateway `10.0.30.1`, DNS `10.0.30.1`, internet pass
- Identified follow-up: OPNsense GUI access from VLAN30/trusted-admin path requires rule/listen-interface review.
