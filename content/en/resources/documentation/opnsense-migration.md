---
title: "Migrating from OPNsense"
description: "Import an OPNsense config.xml into Stratum Firewall — interactive interface mapping, rules, NAT, Virtual IPs, GeoIP/Hostname aliases, Policy Routing, and zero-touch OpenVPN server takeover."
lead: "Move from OPNsense to Stratum Firewall in minutes — including your firewall rules, VLANs, NAT, Virtual IPs, and your entire OpenVPN server without touching a single client laptop."
---

Switching firewall platforms normally requires days of rewriting rules and re-issuing VPN profiles to every employee. Stratum Firewall (`v0.25.0` – `v0.28.3`) includes a built-in **OPNsense Migration Engine** under **System → Backup → Import from OPNsense** that converts an OPNsense `config.xml` into a pending Stratum configuration with an interactive pre-flight report.

## 1. What Gets Imported Automatically

| Category | Imported Elements |
| :--- | :--- |
| **Networking &amp; Routing** | Interfaces, 802.1Q VLANs, gateways, default gateway, Virtual IPs (`IP Alias`), static routes (gateways resolved by name), and interface groups. |
| **Firewall &amp; Policy Routing** | Interface &amp; group rules (both legacy and current OPNsense XML formats), rule-level WAN gateways ([Policy Routing](/resources/documentation/multi-wan/)), and bandwidth shaper limits. |
| **NAT &amp; Access Restrictions** | Port forwards (including Virtual IP targets and source restrictions preserved from associated WAN rules) and Outbound NAT rules. |
| **Dynamic &amp; Static Aliases** | Host, network, and port aliases (nested aliases resolved), **Hostname aliases** (FQDNs), and **GeoIP Country aliases** mapped directly to Stratum's license-free 5-RIR country tables. |
| **Core Services** | DHCP scopes &amp; static reservations (`dnsmasq` and `ISC dhcpd`), DNS host/domain overrides, hostname, domain, time zone, DNS, and NTP servers. |
| **OpenVPN Server Takeover** | *(Optional checkbox)* CA + private key, server certificate, `tls-crypt`/`tls-auth` key, all user certificates, tunnel/cipher/DNS/`block-outside-dns` settings, and VPN user accounts with their existing OPNsense password hashes (PHP `$2y$` bcrypt &amp; `argon2id`). |

## 2. Step-by-Step Migration Workflow

1. **Export `config.xml` from OPNsense:** In OPNsense, go to *System → Configuration → Backups* and download an unencrypted `config.xml` file.
2. **Upload &amp; Preview in Stratum FW:** Open **System → Backup → Import from OPNsense**, select your `config.xml`, and check **"Also import OpenVPN"** if you want Stratum to take over your OpenVPN remote-access server.
3. **Interactive Port Mapping:** Assign each OPNsense interface (e.g., `igb0`, `igb1`, VLANs) to the physical NICs on your Stratum appliance (or omit unused ports).
4. **Inspect the Pre-Flight Report:** Before anything is written, Stratum displays a detailed report showing:
   * Every rule, alias, NAT entry, VIP, route, and OpenVPN user carried over,
   * Warnings if the LAN IP changes or if an imported subnet overlaps with your current WAN address,
   * Any unsupported items (such as CARP VIPs, IPsec, or third-party plugins) and how they were handled safely (for example, unreferenced port forwards are imported disabled rather than opened to `any`).
5. **Apply &amp; Confirm with Safe-Apply:** Saving loads the imported setup as **pending changes**. Click **Apply changes** and verify connectivity within the 60-second **Safe-Apply** window before clicking **Keep changes**.

## 3. Zero-Touch OpenVPN Client Continuity

When **"Also import OpenVPN"** is enabled:
* Imported VPN users are created as **VPN-only** accounts (they cannot sign into the web admin panel; if an admin account with the same username already exists on Stratum, its Stratum admin password is kept).
* Both PHP `$2y$` bcrypt and `argon2id` password hashes work immediately (`argon2id` hashes are transparently re-stored as bcrypt upon first successful login).
* Any previous OpenVPN certificates on the Stratum box are archived safely in a separate folder, and existing `.ovpn` profiles on users' Windows, macOS, Linux, iOS, and Android devices connect right away with zero edits.
