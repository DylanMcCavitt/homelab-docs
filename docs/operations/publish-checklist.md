---
type: checklist
area: operations
status: active
updated: 2026-02-19
tags:
  - area/operations
  - type/checklist
---
# Publish Checklist

Use this before posting docs to GitHub/blog/social.

## Content safety

- [ ] No raw backup/config files included
- [ ] No backup filenames, backup paths, or checksum values shown
- [ ] No public IPs or real DDNS/FQDN values exposed
- [ ] No serials, MAC addresses, usernames, or emails exposed
- [ ] No keys, tokens, or cert/private material exposed
- [ ] Hostnames and SSIDs sanitized where needed
- [ ] No local machine paths (`[private-file-redacted]`, `[private-file-redacted]`) exposed

## Architecture clarity

- [ ] Change objective is clear
- [ ] Validation and outcome are documented
- [ ] Rollback/recovery path is documented for risky changes

## Professional quality

- [ ] Markdown headings and structure are consistent
- [ ] Date and timestamp references are clear
- [ ] Any screenshots are legible and redacted

## Related
- [Start Here](../00-start-here.md)
- [Operations](../03-operations.md)
- [Redaction Policy](redaction-policy.md)
