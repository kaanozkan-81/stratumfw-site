---
title: "Stratum Firewall v0.30.0: TOTP Two-Factor Authentication, Force DNS Enforcement, and Multi-WAN DHCP"
date: 2026-11-11
author: "Stratum Security"
translationKey: "v0-30-0-release"
description: "Stratum Firewall v0.30.0 delivers local QR-based TOTP two-factor authentication (2FA) with recovery codes, Force DNS redirection with Active Directory exceptions, dynamic DHCP on secondary WANs, 1:1 NAT, and rule schedules."
lead: "Reaching milestone v0.30.0, Stratum Firewall adds hardware-independent TOTP 2FA, tamper-proof DNSBL/Web Filter enforcement, plug-and-play 4G/5G & cable DHCP WAN failover, and expanded OPNsense migration intelligence."
tags: ["firewall", "stratum firewall", "freebsd 15.1", "totp 2fa", "force dns", "multi-wan dhcp", "v0.30.0"]
---

Hot on the heels of time-based schedules, 1:1 NAT, and Firewall Log Summary analytics, **Stratum Firewall v0.30.0** ships three highly requested security and multi-ISP capabilities (`v0.29.1`, `v0.29.2`, and `v0.30.0`) on [Stratum Firewall](https://stratumfw.com/). Full details are listed in the [Official Changelog](https://stratumfw.com/resources/changelog/).

### 1. Local TOTP Two-Factor Sign-In & Recovery Codes (`v0.30.0`)

Administrators and operators can now protect web panel accounts with standard 6-digit **TOTP two-factor authentication** under **My Account** (`Google Authenticator`, `Microsoft Authenticator`, `1Password`, `Bitwarden`, and any RFC 6238 app):

* **100% On-Box Privacy:** The provisioning QR code is rendered locally in your browser from a bundled library—your TOTP secret never leaves the firewall and is never displayed again after enrollment.
* **Replay & Brute-Force Hardening:** Each 6-digit token works strictly once (accepting a 30-second clock skew), enabling 2FA invalidates other active sessions immediately, and failed code attempts feed directly into sign-in lockout protection.
* **Zero-Lockout Recovery:** Enrollment generates **10 one-time recovery codes** (triggering an admin notification whenever one is used), while administrators and the physical/serial console recovery menu (`stratum-menu`) can reset 2FA for a lost phone.

### 2. Tamper-Proof Force DNS & Smarter OPNsense Imports (`v0.29.2`)

Under **Services -> DNS Resolver -> Force DNS through the firewall**, administrators can intercept outbound DNS queries on selected LAN/VLAN segments regardless of hardcoded client DNS settings (`8.8.8.8`, `1.1.1.1`, etc.) while blocking DNS over TLS (`DoT`):

* **Active Directory Exception Alias:** Exclude internal Active Directory domain controllers or specialized DNS servers via an exception alias while enforcing DNSBL and the 55-category Web Filter on every other host—completely independent of Web Activity logging.
* **Automatic OPNsense Rule & Feed Translation:** Importing an OPNsense `config.xml` now translates DNS redirect port forwards (including negated destination exception aliases) straight into native Force DNS, and maps OPNsense URL-table aliases matching catalog IP feeds (such as `CINS Army` or `blocklist.de`) directly to active inbound/outbound IP blocklists.

### 3. Dynamic DHCP on Secondary & Tertiary WANs (`v0.29.1`)

Connecting a secondary cable modem, Starlink dish, or 4G/5G LTE cellular router no longer requires static bridge IPs ([Multi-WAN Guide](https://stratumfw.com/resources/documentation/multi-wan/)):

* **Clean Default-Route Discipline:** Additional DHCP WANs never hijack the system default route or DNS servers on their own—failover priority stays strictly governed by the 1-second Gateway Monitor.
* **Live Lease Tracking Without Apply:** Whenever an ISP changes the DHCP lease gateway, `reply-to` return routing, failover targets, and per-rule Policy Routing update automatically within seconds.

### Update to v0.30.0 or Test Live in Your Browser

Existing systems can upgrade to **v0.30.0** under **System -> Update** (or via signed `.sfu` offline bundle) with an automatic ZFS Boot Environment snapshot. Explore the live FreeBSD 15.1 appliance at [demo.stratumfw.com](https://demo.stratumfw.com/) or download installers from the [Download](https://stratumfw.com/download/) page.
