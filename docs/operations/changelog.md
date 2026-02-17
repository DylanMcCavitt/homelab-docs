---
type: changelog
area: operations
updated: 2026-02-17
---
# Homelab Change Log

Use this file for chronological, operator-grade change history.

## Entry format

Copy `docs/operations/change-entry-template.md` for each change.

---

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
