---
type: runbook
area: network
status: active
updated: 2026-02-18
---
# Runbook: UniFi Controller Migration to Proxmox (Standard Strict)

## Objective
Move from transitional networking to strict segmentation without lockout:
1. Proxmox host management on `VLAN_MGMT` (VLAN30).
2. UniFi controller VM on `VLAN_MGMT` (VLAN30).
3. Keep `VLAN_SERVERS` (VLAN20) for non-management workloads.
4. Preserve trusted-admin access from `VLAN_TRUSTED` (VLAN10) using explicit pass rules.

## Scope and Safety Model
- This runbook is execution-focused and lockout-safe.
- This file is public-safe and uses aliases/placeholders.
- Concrete values for your environment are in:
  - `private/runbooks/unifi-proxmox-standard-strict-private.md`

## Execution Status (2026-02-18)
- Completed:
  - Proxmox management transition path to VLAN30
  - UniFi OS Server deployment on management VM
  - Controller backup restore and AP rebind to new controller
- Remaining:
  - AP management-IP migration from legacy LAN to VLAN30
  - Firewall cleanup to remove temporary legacy LAN exceptions

## Inputs Required Before You Start
Fill these values in the private worksheet first:
1. `ADMIN_TRUSTED_HOST_IP`
2. `PROXMOX_OLD_IP` (current VLAN10 management IP)
3. `PROXMOX_HOST_IP` (new VLAN30 management IP)
4. `UNIFI_CONTROLLER_HOST_IP` (new UniFi VM IP on VLAN30)
5. `UNIFI_AP_HOST_IP` (current AP management IP)
6. `VLAN10_GW_IP`
7. `VLAN30_GW_IP`

## Phase 0: Preconditions (Do Not Skip)
1. Confirm console access to Proxmox host (keyboard/monitor attached).
2. Confirm OPNsense GUI is open and reachable from trusted client.
3. Confirm Proxmox GUI is reachable on current address.
4. Keep rescue switch path unchanged.
5. Confirm backups exist:
   - OPNsense backup in `private/raw-backups/opnsense/`
   - UniFi backup in `private/raw-backups/unifi/`

Stop condition:
1. If either OPNsense GUI or Proxmox GUI is unreachable, stop and restore access first.

## Phase 1: OPNsense Aliases and Rules (Before Network Cutover)
Create aliases first.

Important UI note:
1. `Destination Port` is only valid when protocol is `TCP`, `UDP`, or `TCP/UDP`.
2. If protocol is `any`, OPNsense will reject destination port input.

## Alias Set
1. `PROXMOX_HOST` -> `PROXMOX_HOST_IP`
2. `UNIFI_CONTROLLER_HOST` -> `UNIFI_CONTROLLER_HOST_IP`
3. `UNIFI_AP_HOSTS` -> `UNIFI_AP_HOST_IP`
4. `UNIFI_CTRL_TCP_PORTS` -> `8080`
5. `UNIFI_CTRL_UDP_PORTS` -> `3478,10001`
6. `UNIFI_OS_GUI_PORT` -> `11443`

## Rule A: Admin VLAN10 -> Proxmox GUI/SSH
| Field | Value |
|---|---|
| Interface | `VLAN10_Trusted` |
| Action | `Pass` |
| Direction | `In` |
| Version | `IPv4` |
| Protocol | `TCP` |
| Source | `ADMIN_TEMP_TRUSTED_HOST` (or admin alias) |
| Source Port | `any` |
| Destination | `PROXMOX_HOST` |
| Destination Port | `8006,22` |
| Description | `Allow Admin VLAN10 to Proxmox Mgmt` |

## Rule B: Admin VLAN10 -> UniFi OS GUI
| Field | Value |
|---|---|
| Interface | `VLAN10_Trusted` |
| Action | `Pass` |
| Direction | `In` |
| Version | `IPv4` |
| Protocol | `TCP` |
| Source | `ADMIN_TEMP_TRUSTED_HOST` |
| Source Port | `any` |
| Destination | `UNIFI_CONTROLLER_HOST` |
| Destination Port | `11443` |
| Description | `Allow Admin VLAN10 to UniFi OS GUI` |

## Current-State Shortcut (Use This If Already Present)
If your ruleset already has both:
1. `Allow Temp Admin To FW GUI`
2. `Allow Temp Admin To Mgmt Targets` using `MGMT_TARGETS + MGMT_GUI_PORTS`

Then Rule A and Rule B are already functionally covered for this migration window.
Action:
1. Keep temporary admin rules in place.
2. Do not duplicate Rule A/B now.
3. Add VLAN30 UniFi control-plane rules first (Rule C/D + dependency rules as needed).

## Rule C: UniFi Controller -> AP (Inform)
| Field | Value |
|---|---|
| Interface | `VLAN30_MGMT` |
| Action | `Pass` |
| Direction | `In` |
| Version | `IPv4` |
| Protocol | `TCP` |
| Source | `UNIFI_CONTROLLER_HOST` |
| Source Port | `any` |
| Destination | `UNIFI_AP_HOSTS` |
| Destination Port | `8080` |
| Description | `Allow UniFi Inform TCP` |

## Rule D: UniFi Controller -> AP (STUN/Discovery)
| Field | Value |
|---|---|
| Interface | `VLAN30_MGMT` |
| Action | `Pass` |
| Direction | `In` |
| Version | `IPv4` |
| Protocol | `UDP` |
| Source | `UNIFI_CONTROLLER_HOST` |
| Source Port | `any` |
| Destination | `UNIFI_AP_HOSTS` |
| Destination Port | `3478,10001` |
| Description | `Allow UniFi STUN/Discovery UDP` |

## Transitional Rule Set (If AP Management Is Still on Legacy LAN)
If AP management IP is still in `192.168.1.0/24`, add temporary rules on `LAN`:

1. `LAN` pass `TCP` source `UNIFI_AP_HOSTS` -> destination `UNIFI_CONTROLLER_HOST` port `8080`
2. `LAN` pass `UDP` source `UNIFI_AP_HOSTS` -> destination `UNIFI_CONTROLLER_HOST` ports `3478,10001`

Cleanup trigger:
1. Remove these `LAN` exceptions after AP management is migrated to VLAN30 and validated stable.

## Rule E/F/G/H: UniFi Controller Outbound Dependencies
Create separate rules on `VLAN30_MGMT` from `UNIFI_CONTROLLER_HOST` to `any`:
1. DNS UDP `53`
2. DNS TCP `53`
3. HTTPS TCP `443`
4. HTTP TCP `80`
5. NTP UDP `123`

Practical note for current config:
1. If `VLAN30_MGMT` currently has a broad `Internet out` allow rule, these dependency rules are optional during migration.
2. Keep them if you want explicit documentation now, or defer until later hardening.

Ordering requirement:
1. Keep these explicit pass rules above broad management deny rules.

## Exact Rule Placement Guide (When CSV Exports Show `opt*`)
CSV exports show internal interface ids:
1. `opt1` = `VLAN10_Trusted`
2. `opt3` = `VLAN30_MGMT` (in your current migration context)

Placement by intent:
1. On `VLAN10_Trusted`:
   - keep admin temp allow rules at the top
   - keep `Block VLAN10 to Mgmt Targets Non Admin` below those allows
   - keep broad temporary `Trusted allow any` below the management block
2. On `VLAN30_MGMT`:
   - place `Allow UniFi Inform TCP` and `Allow UniFi STUN/Discovery UDP` after DHCP/DNS/NTP
   - place them before `Block old LAN` / other broad internal deny rules
   - keep `Internet out` below those rules

## Phase 2: MikroTik Port2 Move to Trunk (Proxmox Uplink)
Assumptions:
1. Port1 = OPNsense trunk
2. Port2 = Proxmox host
3. Port3 = AP trunk

## SwOS VLAN Membership (`VLANs` tab)
1. VLAN10: include Ports `1,2` (plus existing required ports).
2. VLAN20: include Ports `1,2` (server VLAN path available).
3. VLAN30: include Ports `1,2` (management VLAN path).
4. VLAN40/50: keep current AP/trunk memberships unchanged.

## SwOS Per-Port VLAN (`VLAN` tab) for Port2
| Field | Value |
|---|---|
| VLAN Mode | `strict` |
| VLAN Receive | `any` |
| Default VLAN ID | `10` (temporary transition value) |
| Force VLAN ID | unchecked |

Apply and checkpoint:
1. Confirm Proxmox remains reachable on current VLAN10 management IP.

## Phase 3: Proxmox Host Management Move to VLAN30 (Two-Step)
## Step 3.1: Add VLAN30 Path First
1. In Proxmox node networking:
   - Edit `vmbr0`, enable `VLAN aware`.
   - Keep existing VLAN10 host config temporarily.
2. Add interface `vmbr0.30`:
   - IPv4/CIDR: `PROXMOX_HOST_IP/24`
   - Gateway: blank for first apply
   - VLAN raw device: `vmbr0`
3. Apply configuration.

Validation:
1. `https://PROXMOX_HOST_IP:8006` reachable.
2. Old Proxmox GUI IP still reachable.

## Step 3.2: Move Default Gateway to VLAN30
1. Remove default gateway from old host interface.
2. Set gateway on `vmbr0.30` to `VLAN30_GW_IP`.
3. Apply and re-test GUI + outbound connectivity.

Stabilization:
1. Keep old VLAN10 host IP one full session as rollback path.
2. Remove old VLAN10 host IP only after stable validation.

Validation note:
1. From a VLAN10 admin client, ICMP/ping to `VLAN30_GW_IP` may be blocked by policy.
2. Do not use VLAN10->VLAN30 gateway ping as a migration health signal.
3. Use GUI reachability from admin client plus Proxmox host-side routing tests instead.

## Phase 4: Build UniFi VM on VLAN30
VM spec:
1. Debian 12 or Debian 13 (`amd64` netinst ISO)
2. vCPU `2`
3. RAM `4 GB`
4. Disk `32 GB`
5. NIC bridge `vmbr0`, VLAN tag `30`
6. Static guest network:
   - IP: `UNIFI_CONTROLLER_HOST_IP/24`
   - Gateway: `VLAN30_GW_IP`
   - DNS: `VLAN30_GW_IP`

Hardening baseline:
1. Update packages.
2. Install/enable QEMU guest agent.
3. Snapshot name: `clean-debian`.

Version note:
1. For UniFi OS Server deployment path, Debian 12 and Debian 13 are both valid.
2. The prior Debian 13 APT issue applied to the legacy `unifi` package repository path, not UniFi OS Server.

## Phase 4A: Debian Installer (Screen-by-Screen)
Use this exact sequence inside the VM console while installing Debian.

1. Boot menu:
   - Select `Graphical install` (recommended for first-time setup).
2. Language / location / keyboard:
   - Pick your normal preferences.
3. Network:
   - If DHCP works, accept DHCP for install time.
   - Hostname: `unifi-controller`
   - Domain: `[redacted-hostname]` (or leave blank if you prefer)
4. Users:
   - Set root password or leave root disabled and use sudo user (either is fine).
   - Create one admin user.
5. Time zone:
   - Select your local zone.
6. Partition disks:
   - Choose `Guided - use entire disk`
   - Choose VM disk
   - `All files in one partition` (simple baseline)
   - `Finish partitioning and write changes to disk`
7. Package mirror:
   - Use default Debian mirror.
8. Popularity contest:
   - Optional; either choice is fine.
9. Software selection (important):
   - Keep `standard system utilities`
   - Keep `SSH server`
   - Do not select desktop environment for this controller VM
10. GRUB bootloader:
   - Install GRUB to the primary disk when prompted.
11. Reboot:
   - Remove/eject ISO after installation completes.

## Phase 4B: Post-Install Static Network (Debian Guest)
Set static networking in the guest OS after first boot.

Privilege note:
1. Some Debian installs do not include `sudo` by default.
2. If `sudo: command not found`, switch to root shell first:
```bash
su -
```
3. Then run the same commands without `sudo`.
4. Optional later: install sudo and add your user:
```bash
apt-get update && apt-get install -y sudo
usermod -aG sudo <your-user>
```

1. Identify interface name:
```bash
ip -br a
```
2. Configure static IP (if using ifupdown):
```bash
sudo cp /etc/network/interfaces /etc/network/interfaces.bak
sudo nano /etc/network/interfaces
```
3. Minimal config template:
```text
auto lo
iface lo inet loopback

allow-hotplug <NIC_NAME>
iface <NIC_NAME> inet static
    address <UNIFI_CONTROLLER_HOST_IP>/24
    gateway <VLAN30_GW_IP>
    dns-nameservers <VLAN30_GW_IP>
```
4. Apply:
```bash
sudo systemctl restart networking
```
5. Validate:
```bash
ip -4 a
ip route
ping -c 3 <VLAN30_GW_IP>
ping -c 3 1.1.1.1
```

## Phase 5: Install UniFi OS Server and Restore Backup
Use the official UniFi OS Server self-hosted installer path.

1. Install dependencies:
```bash
apt-get update
apt-get install -y podman slirp4netns curl ca-certificates
```
2. Download Linux x64 UniFi OS Server binary from the official download page.
3. Run installer:
```bash
chmod +x /path/to/uosserver-binary
/path/to/uosserver-binary
```
4. When prompted, type `yes` (some builds reject short `y` input).
5. Validate service:
```bash
systemctl status uosserver --no-pager
```

Post-install checks:
1. UI reachable on `https://UNIFI_CONTROLLER_HOST_IP:11443`.
2. Complete local onboarding (cloud account optional).
3. Open/install `Network` app and restore backup from `private/raw-backups/unifi/`.
4. Snapshot name after restore: `unifi-restored`.

Legacy note:
1. The APT `unifi` package path is optional legacy flow and not required for this runbook.

## Phase 6: AP Rebind and Cutover
1. Keep old Mac controller running until restore is complete.
2. SSH to AP and run:
```bash
set-inform http://UNIFI_CONTROLLER_HOST_IP:8080/inform
```
3. Repeat once after AP appears pending adoption.
4. Wait for `Adopting` -> `Provisioning` -> `Online`.
5. After 10-15 minutes stable, stop old Mac controller service.

## Phase 7: Validation Matrix (Must Pass)
1. From trusted VLAN10 admin host:
   - Proxmox GUI reachable at `PROXMOX_HOST_IP:8006`
   - UniFi GUI reachable at `UNIFI_CONTROLLER_HOST_IP:11443`
2. UniFi AP status: `Online` without repeated flap/disconnect.
3. VLAN10/40/50 client behavior unchanged (IP, gateway, DNS, internet, isolation).
4. OPNsense logs show no recurring drops on UniFi control-plane traffic.

## Phase 8 (Next Session): AP Management Migration to VLAN30
1. Pre-check:
   - AP is currently `Online` in controller.
   - Port3 (AP uplink) carries VLAN30 in MikroTik VLAN membership.
   - VLAN30 DHCP has an available address or static mapping ready for AP MAC.
2. In UniFi device settings for U6 Pro:
   - set Management VLAN to `30`
   - apply changes and wait for reprovision/reconnect
3. Validate post-change:
   - AP management IP is now `10.0.30.x`
   - AP remains `Online` after 10+ minutes
   - SSIDs for VLAN10/40/50 still pass client tests
4. Cleanup after successful migration:
   - remove temporary `LAN` AP->controller allow rules
   - remove legacy AP IP from `UNIFI_AP_HOSTS` alias
   - keep only steady-state VLAN30 management policy

## Phase 9: Rollback Plan
1. Keep and use old Proxmox VLAN10 host IP until end of stabilization.
2. Revert Port2 to prior access config if trunk cutover fails.
3. Re-enable old Mac controller and re-point AP inform URL if needed.
4. Restore VM snapshot (`unifi-restored` or `clean-debian`) for fast retry.

## Evidence Capture Checklist
1. Screenshot: OPNsense rules and ordering.
2. Screenshot: SwOS VLAN and Port2 settings.
3. Screenshot: Proxmox networking (`vmbr0` + `vmbr0.30`).
4. Screenshot: UniFi AP `Online` state.
5. Command outputs:
   - GUI reachability checks
   - DHCP/gateway checks for VLAN10/40/50 sample clients
6. Backup export after cutover:
   - New `.unf` in `private/raw-backups/unifi/`
   - Updated checksum manifest.

## References
- Self-hosting UniFi OS Server: https://help.ui.com/hc/en-us/articles/34210126298775-Self-Hosting-UniFi
- UniFi OS Server downloads: https://ui.com/download/software/unifi-os-server
- UniFi required ports: https://help.ui.com/hc/en-us/articles/218506997-UniFi-Network-Required-Ports-Reference
- UniFi remote/L3 adoption (`set-inform`): https://help.ui.com/hc/en-us/articles/204909754-UniFi-Device-Adoption-Methods-for-Remote-UniFi-Controllers
- Proxmox admin guide (networking/VLAN): https://pve.proxmox.com/pve-docs-9-beta/pve-admin-guide.html
