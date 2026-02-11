---
type: changelog
area: operations
updated: 2026-02-11
---
# Homelab Change Log

Use this file for chronological, operator-grade change history.

## Entry format

Copy `docs/operations/change-entry-template.md` for each change.

---

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
  - VLAN20 test host: `10.0.20.125`, gateway `10.0.20.1`, DNS `10.0.20.1`, internet pass
  - VLAN30 test host: `10.0.30.125`, gateway `10.0.30.1`, DNS `10.0.30.1`, internet pass
- Identified follow-up: OPNsense GUI access from VLAN30/trusted-admin path requires rule/listen-interface review.
