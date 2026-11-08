---
title: "Stratum Firewall v0.27.1: Seamless OPNsense & OpenVPN Migration, Virtual IPs, Country Aliases, and Diagnostics"
date: 2026-11-08
author: "Stratum Security"
description: "Stratum Firewall v0.27.1 introduces complete OPNsense configuration and OpenVPN server migration with zero client profile changes, alongside Virtual IPs, static routes, GeoIP country aliases, and Wireshark packet capture."
lead: "Spanning releases v0.24.3 through v0.27.1, Stratum Firewall delivers drop-in OPNsense and OpenVPN migration, Virtual IP management, license-free GeoIP country filtering, and built-in packet capture diagnostics."
tags: ["stratum firewall", "stratum fw", "freebsd", "opnsense", "openvpn", "virtual ip", "geoip", "suricata"]
---

With the release of [Stratum Firewall](https://stratumfw.com/) **v0.27.1**, migrating from an existing appliance and managing complex multi-WAN or multi-subnet networks becomes significantly faster. Covering the rapid sequence of updates from `v0.24.3` to `v0.27.1` detailed in our [Changelog](https://stratumfw.com/resources/changelog/), this milestone introduces complete OPNsense configuration and OpenVPN server takeovers, Virtual IPs (VIPs), static routing, dynamic hostname and country aliases, and built-in packet capture tools.

### 1. Drop-In OPNsense & OpenVPN Migration (v0.25.0 & v0.27.1)

The biggest hurdle when switching firewalls is recreating years of firewall rules and re-deploying VPN profiles to every user's laptop and phone. **System > Backup > Import from OPNsense** reads an existing OPNsense `config.xml` and converts it directly into a pending [Stratum Firewall](https://stratumfw.com/) configuration:

* **Comprehensive Network & Rule Import:** Interfaces, VLANs, gateways, interface groups, nested aliases, firewall rules, port forwards (preserving source access restrictions from associated WAN rules), outbound NAT, DHCP scopes and reservations (`dnsmasq` and `ISC dhcpd`), bandwidth limits, and DNS overrides are all carried over ([Changelog](https://stratumfw.com/resources/changelog/)).

* **Interactive Port Mapping & Pre-Flight Report:** Before anything is saved, you can map the OPNsense hardware ports to your target machine's network interfaces and inspect a detailed report showing everything imported, adjusted, or skipped ([Architecture](https://stratumfw.com/product/architecture/)).

* **Complete OpenVPN Server Takeover (`v0.27.1`):** Enabling *"Also import OpenVPN"* transfers the Certificate Authority (CA) and private key, server certificate, `tls-crypt` or `tls-auth` key, user certificates, tunnel settings, and VPN user accounts complete with their existing OPNsense password hashes (supporting both PHP `$2y$` bcrypt and `argon2id`). Existing `.ovpn` profiles on users' devices connect **unchanged** immediately after cutover ([Changelog](https://stratumfw.com/resources/changelog/)).

* **Multi-WAN OpenVPN & Client Hardening:** OpenVPN now replies from the exact WAN address the client connected to (`multihome` paired with `pf` reply routing), supports multiple ordered `remote` lines in client profiles, and can enforce `block-outside-dns` for Windows clients ([Features](https://stratumfw.com/product/features/)).

### 2. Virtual IPs (VIPs) and Static Routes (v0.27.0)

For deployments with routed public subnets (such as a `/29` WAN block) or downstream internal routers, **Interfaces > Virtual IPs** and **Interfaces > Routes** bring first-class multi-address and static routing control ([Features](https://stratumfw.com/product/features/)):

* **Virtual IPs for Port Forwards & Outbound NAT:** Assign additional public or internal addresses to any interface, target specific Virtual IPs in port forward rules (with automatic NAT reflection), or translate outbound traffic to a dedicated Virtual IP ([Changelog](https://stratumfw.com/resources/changelog/)).

* **Static Routes with Automatic Outbound NAT:** Define static routes to subnets behind downstream internal routers; Stratum automatically includes those routed internal networks in outbound internet NAT rules.

* **Clean Kernel Sync via `stratum-netextra`:** Virtual IPs and static routes are never hardcoded into `rc.conf`. Instead, `stratum-netextra` reconciles active kernel addresses and routes at boot, after every Apply or rollback, and across interface restarts ([Architecture](https://stratumfw.com/product/architecture/)).

### 3. Dynamic Hostname & License-Free Country (GeoIP) Aliases (v0.26.0 & v0.26.1)

**Firewall > Aliases** now supports two dynamic, file-backed `pf` table types that update live without requiring a firewall reload ([Features](https://stratumfw.com/product/features/)):

* **Hostname Aliases:** Resolve FQDNs (such as `update.example.com`) every 5 minutes through the firewall's own local DNS resolver. Resolved IP addresses are retained for 60 minutes after they were last seen to gracefully handle rapid CDN IP rotation ([Changelog](https://stratumfw.com/resources/changelog/)).

* **Country (GeoIP) Aliases:** Restrict port forwards to specific countries or block hostile regions using weekly address blocks sourced directly from the five Regional Internet Registries (ARIN, RIPE NCC, APNIC, LACNIC, AFRINIC)—completely free and with no external API key required.

### 4. Packet Capture (.pcap), ARP Table, Wake-on-LAN & Power Controls (v0.25.1)

New built-in operational tools under **Diagnostics** and **System** eliminate the need for command-line troubleshooting ([Changelog](https://stratumfw.com/resources/changelog/)):

* **Wireshark Packet Capture (`Diagnostics > Packet Capture`):** Capture traffic on any interface into a downloadable `.pcap` file using structured protocol, host/network, and port filters, while automatically excluding your own administrative web panel and SSH session.

* **ARP Table & Wake-on-LAN (`Diagnostics > ARP Table`):** Inspect every active neighbor with its MAC address, interface, DHCP/DNS hostname, and randomized mobile MAC indicator, or wake sleeping machines with a single click via **Wake-on-LAN**.

* **Remote-Safe Factory Reset (`System > Power`):** Reboot, power off, or reset to factory defaults while preserving the currently active physical interface assignments so a remote appliance comes back online reachable via WAN DHCP and LAN `192.168.1.1/24`.

### 5. Per-Interface Dual-Engine Suricata IPS/IDS (v0.24.3 & v0.24.4)

**Services > Intrusion Detection** now lets you mix **Watch** (IDS) and **Block** (inline `netmap` IPS) modes per interface—for example, watching WAN traffic while actively blocking threats on internal LAN/VLAN interfaces ([Features](https://stratumfw.com/product/features/)). When both modes are combined, Stratum supervises two independent Suricata processes (`netmap` for blocking ports and `pcap` for watch-only ports), aggregates their alerts into a unified view, and reloads rule category changes live via FreeBSD `SIGUSR2` ([Changelog](https://stratumfw.com/resources/changelog/)).

### Upgrade or Download Today

Existing [Stratum Firewall](https://stratumfw.com/) deployments can upgrade to **v0.27.1** directly from **System > Update** in the web panel, which automatically snapshots the current system into a ZFS boot environment before applying the signed update. For new installations, grab the latest VGA or Serial installer images from the [Download](https://stratumfw.com/download/) page.
