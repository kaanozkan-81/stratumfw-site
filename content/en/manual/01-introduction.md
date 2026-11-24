---
title: "Introduction"
seo_title: "01. Introduction — Stratum FW User Manual"
description: "What Stratum FW is, how its configuration works, the panel at a glance, users and roles, and the conventions of this manual."
weight: 1
chapter_num: "01"
url: "/manual/introduction/"
aliases:
  - "/manual/00-introduction/"
  - "/manual/01-introduction/"
translationKey: "manual-00-introduction"
---

## About this manual

This manual is for the people who install and run a Stratum FW firewall: network and system administrators,
and anyone who has used pfSense, OPNsense or a commercial firewall before. It assumes you know what an IP
address, a subnet, a gateway and a firewall rule are; it does not assume you know FreeBSD.

Conventions used throughout:

| You see | It means |
| :--- | :--- |
| **Firewall > Rules** | A page of the panel: the menu section, then the page. |
| **Apply changes** | A button, a field or a message, written exactly as the panel shows it. |
| `stratum-config apply` | A command typed on the firewall's console or over SSH. |
| `/usr/local/etc/stratum/config.json` | A file on the firewall. |
| *LAN*, *OPT1* | An interface's role (see [The panel at a glance](#the-panel-at-a-glance)). |

> **Note:** The panel, its messages and its logs are in English. This manual quotes them as they are, so what you read here
> is what you see on the screen.

## What Stratum FW is

Stratum FW is a firewall and router operating system. It turns a PC, a server, a network appliance or a virtual
machine into the gateway between your networks and the internet, and it is managed entirely from a web panel.
It is free to use: there is no subscription, no licence key and no paid feature tier.

Under the panel it is **FreeBSD 15.1** with well-known, proven components:

| Job | Component |
| :--- | :--- |
| Packet filter, NAT, policy routing | FreeBSD **pf** |
| Bandwidth limits | FreeBSD **dummynet** |
| DNS resolver, DNS blocklists, Web Filter | **Unbound** (or dnsmasq) |
| DHCP server | **dnsmasq** |
| Remote-access VPN | **OpenVPN** and kernel **WireGuard** |
| Site-to-site VPN | **IPsec** (strongSwan) and **WireGuard** |
| Intrusion detection and prevention | **Suricata** |
| Application control and DPI | **stratum-dpi** (nDPI in a FreeBSD Capsicum sandbox) |
| PPPoE | **mpd5** |
| The web panel | **nginx** in front of a Python (FastAPI) application |

The system is installed on **ZFS**. Every update creates a *boot environment* - a snapshot of the whole system
you can boot back into - so an update can always be undone.

## How a change reaches the system

This is the most important idea in Stratum FW, and it is worth understanding before you change anything.

### One configuration

Everything you set in the panel - interfaces, rules, VPNs, services - is stored in one file,
`/usr/local/etc/stratum/config.json`. The system's own configuration files (`/etc/pf.conf`, the DNS and DHCP
configuration, the VPN configuration and so on) are **generated** from it. Do not edit those files by hand: the
next Apply writes them again.

Secrets are the exception: passwords, private keys, preshared keys, RADIUS secrets and API keys are kept in
separate files that only the system can read. They are never stored in `config.json` and never sent back to
the browser - a page shows that a secret is saved, never the secret itself.

### Save, then Apply

Saving a page changes the configuration, not the running system. As soon as there is a saved change that is
not active yet, a bar appears under the top of every page:

> The configuration has been changed. Apply the changes to make them active.  **Apply changes**  **Discard**

You can make several changes on several pages and apply them together. **Discard** throws away everything saved
since the last Apply (click it twice - the first click asks for confirmation).

![The apply bar with a saved change](/images/manual/intro-applybar-pending.png)

### Apply: checked first, then activated

When you click **Apply changes**, Stratum FW first checks the new configuration without touching the running
system: the packet filter rules, the web server, the DNS and DHCP configuration, SSH and intrusion detection are
tested with their own checkers, and the programs and kernel modules a change needs (VPN, bandwidth limits ...)
must be present. If any check fails, nothing is changed and the bar shows why; the full output
is under [**Diagnostics > System Logs**](/manual/diagnostics-system-logs/), *Configuration* tab.

If the checks pass, the current state is saved as a snapshot and the new configuration is activated. Only the
services that are affected by the change are reloaded or restarted.

### Keep changes, or it is undone

A new configuration can cut you off - a wrong rule, a changed panel port, a wrong interface. So after every
Apply from the panel, the bar counts down:

> New configuration is active. Keep it within **120** s, otherwise it is reverted automatically.
> **Keep changes**  **Revert now**

- Click **Keep changes** when everything still works - including the page you are on. The change becomes
  permanent.
- Click **Revert now** to go back at once.
- Do nothing - for example because you can no longer reach the panel - and after 120 seconds the previous
  configuration is restored automatically. You lose nothing but the change itself.

![The countdown after Apply](/images/manual/intro-applybar-confirm.png)

> **Rule of thumb:** Before you click **Keep changes**, check the thing you changed: open the page that should now be reachable, ping
> the host that should now be blocked. If you changed the panel's own port or address, the bar shows the new
> address - open it and keep the changes there.

When you apply from the console with `stratum-config apply`, the timer is 180 seconds; confirm with
`stratum-config confirm` or go back with `stratum-config rollback` (see the chapter on the console and command
line).

### Changes that take effect at once

Some settings only concern the panel itself or run on a schedule, and they work as soon as they are saved,
without Apply: the session timeout and the sign-in notice, notifications, automatic backups, MCP access, the
update schedules of the blocklists, the Web Filter and intrusion detection, and the intrusion detection
categories and switched-off rules. **Discard** leaves them as they are.

The same goes for operational actions and account lists - closing connections, disconnecting a captive portal
device, captive portal user accounts and logos, Application Control block page settings, and Web Filter exceptions and keyword actions. Pages that work this way say so next to the setting.

## The panel at a glance

The panel is reached over HTTPS at the firewall's LAN address, for example `https://192.168.1.1/`. On a new
installation the certificate is self-signed, so the browser warns once; you can install your own certificate
later under **System > Certificates**.

![The panel: menu on the left, top bar, page](/images/manual/intro-panel.png)

**The menu** on the left has seven sections:

| Section | What you find there |
| :--- | :--- |
| **Overview** | The **Dashboard**: the state of the firewall in panels you arrange yourself. |
| **Interfaces** | Network ports and their roles, VLANs, interface groups, gateways and multi-WAN, virtual IPs, static routes. |
| **Firewall** | Rules, NAT, aliases, schedules, IP and DNS blocklists, Web Filter, bandwidth limits (Shaper), Application Control. |
| **Services** | Service status, DHCP server, DNS resolver, NTP, intrusion detection, captive portal. |
| **VPN** | OpenVPN, WireGuard, WireGuard site-to-site, IPsec. |
| **System** | Settings, panel users, certificates, backup, update, power. |
| **Diagnostics** | Firewall and system logs, network tools, traffic graphs, Web Activity, states, ARP table, packet capture, AI Assistant. |

**The top bar** shows, on the right: the **notifications** bell (alerts such as a WAN going down or a failed
backup - the same events that can be sent by Telegram or e-mail), the **theme** switch (light, dark or the
system's setting, remembered per user), your **account** (password, two-factor sign-in) and **Sign out**.

**Pages and forms.** Lists show what exists; adding or editing opens a full page of its own, never a pop-up
window. After **Save** you return to the list, where the new entry is shown. Deleting asks for a confirmation
on the same button or row.

**Interface names.** Every interface has a *role* - *WAN*, *LAN*, *OPT1*, *OPT2* ... - and a description you
choose, such as *GUEST* or *CAMERAS*. The panel shows the description large and the role small underneath, so
two VLANs can never be confused. Rules, DHCP settings and everything else stay attached to the role.

## Users and roles

Every person who signs in to the panel has an account of their own (**System > Users**) with one of three roles:

| Role | Can see | Can change |
| :--- | :--- | :--- |
| **Administrator** | Everything. | Everything, including Apply, users, updates and backups. |
| **Operator** | Everything except the administrator-only pages below. | Day-to-day actions only: the network tools (ping, traceroute, DNS lookup), closing connections (states), Wake-on-LAN, ARP entries, and captive portal user accounts and sessions. No configuration change. |
| **Read-only** | Everything except the administrator-only pages below. | Nothing but their own password, two-factor sign-in, theme and dashboard. |

Some pages are for administrators only, even to look at, because they show private data or give control of the
system: **Settings** pages for admin access, MCP and SNMP, **Users**, **Backup**, **Update**, **Power**,
**Web Activity**, the blocklists' **Blocked** report, **Packet Capture**, the **AI Assistant**, the IPsec log and
the downloads of VPN client profiles.

A few safeguards apply to everyone:

- **Sign-in protection:** by default, 5 failed sign-ins from one address within 5 minutes block that address
  for 15 minutes (adjustable under **System > Settings > Admin Access**).
- **Two-factor sign-in (TOTP):** each user can turn it on under their account, with recovery codes. The QR
  code is generated on the firewall itself.
- **Session timeout:** a session ends after 4 hours by default (adjustable from 15 minutes to 24 hours, also
  under **Admin Access**).
- **Logging:** every sign-in and every change is written to the system log, with the user and the address it
  came from.

## Safety nets

Stratum FW is built so that a mistake is easy to undo:

| If ... | then ... |
| :--- | :--- |
| a change cuts you off | it is reverted after 120 seconds (see [Keep changes, or it is undone](#keep-changes-or-it-is-undone)). |
| a change does not pass the checks | it is not applied at all, and the bar says why. |
| an update misbehaves | boot the previous boot environment from the boot menu. |
| the hardware fails | restore a backup ([**System > Backup**](/manual/system-backup/)) - manual downloads or scheduled, encrypted copies on your own server. |
| you forget the admin password | reset it from the console menu (**3) Reset admin password**). |

## Where to go next

- First sign-in and setup: continue with [**02 Initial Configuration**](/manual/initial-configuration/).
- Installing onto a new machine: see [**03 Installation**](/manual/installation/).
- Moving from OPNsense: the chapter [**12 Migrating from OPNsense**](/manual/migrating-from-opnsense/) imports your existing configuration.
- Already running: jump to the chapter of the menu you need from the contents.
