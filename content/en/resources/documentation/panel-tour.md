---
title: "Panel Tour & Operations"
description: "Explore the Stratum Firewall web interface — customizable 12-column dashboard, per-user notifications, sticky Safe-Apply, Service Status, RBAC, and encrypted SFTP backups."
lead: "Navigate the Stratum web panel — customize your dashboard, manage running daemons, assign user roles, and automate encrypted off-site backups."
---

The Stratum management panel is built with Python 3.12, FastAPI, server-rendered templates, and lightweight vanilla JavaScript modules (supporting both **Dark** and **Light** themes, mobile screens, and seamless **EN / TR** switching).

## 1. Customizable 12-Column Dashboard (`v0.23.0` – `v0.28.2`)

![Stratum Dashboard](/images/docs/dashboard.png)

Every administrator and operator has their own personal dashboard layout saved immediately without needing an Apply:

* **Drag &amp; Resize (`gridstack`):** Click **Customize** to drag widgets by their header or resize them across the 12-column grid; click **Reset layout** anytime to restore defaults.
* **Dashboard → Panels:** Add or remove live widgets including *System*, *WAN*, *CPU*, *Memory*, *Disk*, *Temperature*, *Connections*, *Traffic*, *Gateways*, *Bandwidth Limits*, *Intrusion Detection*, *Blocklists (`v0.28.2`)*, *DHCP*, *VPN Connections*, *Backup*, and *Web Activity*.

## 2. Inline &amp; Sticky Safe-Apply Workflow (`v0.28.0`)

* Whenever you save an actual configuration change, an **Apply changes** button appears right beside the page's **Save** button (non-config actions like Wake-on-LAN or packet capture never trigger unnecessary applies).
* The **Apply changes / Keep changes** banner stays pinned below the top bar while scrolling long pages. Once applied, confirm connectivity within **60 seconds** or Stratum automatically reverts to the last confirmed `config.json` snapshot.

## 3. Per-User Notifications &amp; External Alerts (`v0.12.4`, `v0.28.0`)

* **Top-Right Notification Bell:** Displays unread system events (WAN failover/recovery, blocked brute-force logins, high-severity Suricata IPS alerts, certificate expiry, SFTP backup status, available updates). Opening the menu preserves unread states until you click an item or **Mark all read**; clearing your list is personal and never affects other users.
* **Telegram &amp; SMTP Alerts (`System → Settings → Notifications`):** Forward critical alerts in real time to a Telegram bot or STARTTLS/TLS SMTP server with built-in test buttons.

## 4. Service Supervision (`Services → Service Status`, `v0.28.1`)

Inspect every firewall service in one table showing its status (`running`, `stopped`, `switched off`), uptime, RAM usage, and `PID`:
* Administrators can **Start**, **Stop** (with two-click confirmation), and **Restart** services directly.
* Core services (`pf`, Nginx, web panel, SSH, syslogd, scheduler) safely allow **Restart** only; restarting `pf` reloads `/etc/pf.conf` and repopulates IP blocklist tables cleanly.

## 5. Role-Based Access Control &amp; Encrypted SFTP Backups (`v0.22.0`)

* **Users &amp; Roles (`System → Users`):** Assign **Administrator** (full read/write), **Operator** (view state + run diagnostics &amp; Wake-on-LAN), **Read-Only** (inspection only, used in our [Live Demo](https://demo.stratumfw.com)), or **VPN-only** (OpenVPN access with zero web UI login).
* **Encrypted Backups (`.stbk`) &amp; Scheduled SFTP (`System → Backup`):** Export password-encrypted (`AES-256-GCM` + `scrypt`) `.stbk` archives that optionally bundle all private keys, OpenVPN/TLS certificates, WireGuard keys, and tokens—and schedule automatic SFTP uploads to a remote NAS or Linux/Windows OpenSSH server with retention limits.
