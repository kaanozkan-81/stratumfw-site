---
title: "Diagnostics, Firewall Log Summary & Command Line"
description: "Troubleshoot networks using Stratum Firewall's 1h/24h Firewall Log Summary analytics, Wireshark packet capture, ARP/Wake-on-LAN, Web Activity, Stratum CLI tools, pfctl, and ZFS bectl."
lead: "Troubleshoot from the web UI or SSH — 1h/24h Firewall Log Summary charts, Wireshark .pcap capture, ARP & Wake-on-LAN, Web Activity, Stratum CLI utilities, and native FreeBSD tools."
---

Stratum Firewall equips administrators with rich browser-based analytics and diagnostics alongside clean command-line utilities on the underlying FreeBSD 15.1 system.

## 1. Built-In Web Diagnostics (`Diagnostics` Menu)

* **Firewall Log &amp; 1h/24h Summary (`Diagnostics → Firewall Log → Summary`, `v0.28.8`):**
  * Inspect real-time `filterlog` hits with clear human-readable system rule names (*"IP blocklist: inbound (WAN)"*, *"Block bogon networks"*, *"Broadcast and multicast"*).
  * Switch to the **Summary** tab (covering the last **1 hour** or **24 hours**, reading rotated log files automatically) to see **top source IPs** (with GeoIP country), **top destination ports** (with service names like `SSH` or `RDP`), **top countries**, **top matched rules**, **top destinations**, and **per-interface 5-minute / hourly charts**—clicking any IP or port opens the live log filtered directly to it.
* **Packet Capture (`Diagnostics → Packet Capture`, `v0.25.1`):** Capture traffic on any interface into a downloadable `.pcap` file for **Wireshark** using structured protocol, host/network, and port filters (automatically excluding your own admin web/SSH session).
* **ARP Table &amp; Wake-on-LAN (`Diagnostics → ARP Table`, `v0.25.1`):** View every neighbor IP, MAC, interface, DHCP/DNS hostname, and randomized mobile MAC indicator—and wake sleeping machines with one-click **Wake-on-LAN**.
* **Web Activity &amp; DHCP History (`Diagnostics → Web Activity`, `v0.20.0`):** Opt-in per-device domain lookup auditing (including OpenVPN usernames and WireGuard peer names) and exportable CSV DHCP/ARP history. (If you want unbypassable DNS filtering *without* recording device lookups, use [Force DNS](/resources/documentation/blocklists/) under `Services → DNS Resolver` instead.)
* **Active `pf` States &amp; Network Tools:** Search/kill active `pf` states, view 1h/24h/30d traffic graphs, and run Ping, Traceroute, and DNS lookups.

## 2. Stratum CLI Utilities

Connect via SSH or choose **`8) Shell`** from the console menu (`stratum-menu`):

| Command | Purpose |
| :--- | :--- |
| `stratum-menu` | Interactive console setup and recovery menu—assign interfaces, set IPs, reset the `admin` password, **and optionally switch off TOTP 2FA (`v0.30.0`)** if an administrator loses their phone. |
| `stratum-config` | Validate, pre-check, apply, or roll back `/usr/local/etc/stratum/config.json` from the command line. |
| `stratum-pfrefresh` | Lightweight live `pf.conf` refresh (`v0.29.0` – `v0.29.1`) run every minute for [Firewall Schedules](/resources/documentation/pf-rules/) and immediately on WAN failover or DHCP WAN gateway changes—rewrites `pf.conf` without an Apply only when the running `pf.conf` matches the rendered configuration. |
| `stratum-update` | Check for signed online updates (`stratum-update run`) or verify/apply updates with automatic ZFS Boot Environment creation. |
| `stratum-netextra` | Reconcile active Virtual IPs (VIPs) and static routes with the applied configuration. |
| `stratum-cert` | Inspect, generate self-signed, install, or restore the previous web panel TLS certificate (`show`, `selfsigned`, `install`, `restore`). |
| `stratum-ipbl` / `stratum-dnsbl` | Refresh IP threat feeds (`pf` tables) or Unbound RPZ DNS blocklists on demand. |

## 3. Essential FreeBSD 15.1 Commands (`pfctl` &amp; `bectl`)

```bash
# Inspect active pf filter, 1:1 binat / NAT rules, states, and blocklist table counts
pfctl -sr
pfctl -sn
pfctl -si
pfctl -t stratum_ipbl -T show | wc -l

# List ZFS Boot Environments created automatically before each update
bectl list
zfs list
```
