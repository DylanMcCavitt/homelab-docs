# Redaction Policy

## Never publish

- Public IP addresses
- Real internal subnets if they can identify your environment
- FQDNs/DDNS domains tied to your home network
- Device serial numbers and MAC addresses
- API tokens, keys, cert material, shared secrets
- Usernames/email addresses tied to admin systems
- SSID names if personally identifying
- Raw config backups from firewall/switch/AP

## Replace with

- Aliases (`VLAN_MGMT`, `fw-lab-01`, `10.x.y.0/24`)
- Masked values (`xx:xx:xx:12:34:56`)
- Sanitized screenshots with identifiers blurred/removed

## Public publishing rule

- Share architecture intent and outcomes, not sensitive implementation details.

## Verification before publish

- Run through `docs/operations/publish-checklist.md`
- Peer-review your own diff for accidental secrets
