---
title: "Changelog"
lead: "Complete version history for Stratum Gateway — from the initial infrastructure build through the current release."
---

All notable changes to the Stratum Gateway project, from the initial infrastructure build through the first working install/update pipeline.

---

## v0.1.0 — Core Infrastructure

The foundational bare-metal build on Debian 13.

- Network infrastructure: physical interface roles (WAN/LAN/OPT), VLAN trunking, `systemd-networkd` configuration
- nftables firewall core: sets, objects, filter and NAT tables built from scratch
- DHCP (Kea) and DNS (Unbound) services
- QoS foundation (traffic shaping groundwork)
- IDS/IPS integration (Suricata)
- Category-based web filtering foundation (RPZ-style DNS blocklists)

## v0.2.0 — Web Panel Foundation

- FastAPI + Jinja2 + vanilla JS admin panel scaffolded
- Authentication with bcrypt-hashed credentials
- Initial dashboard layout and design system
- SNMP, NetFlow, and Loki/Grafana monitoring stack wired in
- Granular NAT rule control

## v0.3.0 — Panel Core Features

- Dashboard widgets
- NAT, Rules, Interfaces, Web Filter, and Objects pages
- Custom firewall rule engine (`fw-rule` CLI tool) with live nftables generation
- Aliases / Objects system (`fw-object` CLI tool) for reusable address and port groups

## v0.4.0 — Security Hardening & VLANs

- DNSBL feed system with per-VLAN scoping
- Fixed a delimiter-injection vulnerability in rule and object handling
- VLAN creation and management UI
- Interface configuration improvements

## v0.5.0 — Multi-WAN & Physical Interface Management

- Physical interface configuration system: OPT ports, free custom naming
- WAN connection modes: DHCP, Static IP, PPPoE, and Reserved Networks
- Default-deny security policy hardening
- Interface role tracking (`interface-roles.conf`)
- Multi-WAN failover monitoring (`wan-monitor.sh`)
- Services management page
- Fixed nftables.conf structural corruption, a CrowdSec crash-loop, and a dashboard performance regression

## v0.6.0 — Threat Detection & Traffic Control

- CrowdSec panel (collaborative IPS)
- Suricata IDS panel, including crash-loop fix and a suppress/whitelist workflow
- SNMP panel
- NetFlow panel
- Multi-WAN Load Balancing (ECMP)
- Traffic Shaping / QoS built on HTB + CAKE
- Fixed persistence issues in `nic-configure`, `nat-rules-apply`, `wan-monitor.sh`, and `filter.nft`

## v0.7.0 — VPN, Governance & Update Groundwork

- QoS v2: per-interface CAKE architecture rework
- User management, audit logging, backup/restore, and general settings pages
- Country-based IP blocking
- WireGuard VPN server
- Safe Search enforcement for web filtering
- First version of the Software Update server (`update.stratumfw.com`) and update-check UI
- Web-based terminal (xterm.js)
- Customizable dashboard widgets
- Fixed a Grafana crash-loop
- Configuration migration analysis for common firewall platforms (106 aliases, 103 rules, 31 NAT rules mapped)
- Initial ISO / unattended-installer planning

## v1.0.0 — Installable Product: ISO, Website & Backup/Restore

The project became a distributable product for the first time.

- Preseed-based Debian installer ISO with a first-boot provisioning pipeline (network bootstrap, secrets / CA / TLS generation, service startup)
- Public corporate website and software-update server migrated to a permanent host (`stratumfw.com` / `update.stratumfw.com`) with real TLS certificates
- Interactive product demo pages (Dashboard, VPN, Web Filter, Geo-blocking)
- Backup/restore system, with fixes for a `pipefail` bug that rejected valid backups, an audit-middleware bug that corrupted large file uploads, and a systemd self-kill deadlock during restore
- Restore progress page with live status polling
- Extensive VirtualBox ISO testing, surfacing and fixing over a dozen first-boot and provisioning issues: PPPoE/DHCP network file naming, VPN peer file format, empty nftables set syntax, CrowdSec / networkd-dispatcher / NetFlow services not starting on first boot, and more

## v1.1.0 — Critical WAN Fix & Factory Reset

- **Root-caused and fixed the WAN assignment "hang":** applying a WAN role restarted `systemd-networkd` wholesale, briefly cutting the panel's own connection (since the panel's IP lives on the same daemon-managed interface) — the backend was actually succeeding, but the browser appeared frozen. Fixed by switching to a targeted `networkctl reload`.
- Fixed CrowdSec firewall bouncer never completing real registration on first boot (was stuck issuing a placeholder API key)
- Added the Factory Reset feature: restores nftables / DNS / Suricata / SNMP to clean templates, wipes generated secrets, VPN config, and interface roles, then reboots into first-boot
- Built the first real download-and-apply Software Update mechanism: HTTPS-only manifest fetch, mandatory SHA-256 verification, automatic backup of the current panel before applying, atomic file replacement with rollback on failure, and a self-restart sequence designed to avoid the same self-inflicted-deadlock class of bug found in restore

## v1.2.0 — Update Mechanism: First Live Validation

- First end-to-end real-world test of the download-and-apply update pipeline (v1.1.0 → v1.2.0), including a visible on-screen marker to confirm the new code was actually running after the update — completed successfully with no errors

## v1.3.0 — Factory Reset Progress Screen

- Added a guided countdown / progress screen for Factory Reset, replacing the previous "silent reboot": polls the panel's health endpoint and automatically redirects to the login screen once the gateway is back online
- Shipped via the update mechanism itself (v1.2.0 → v1.3.0), serving as its second successful live validation
