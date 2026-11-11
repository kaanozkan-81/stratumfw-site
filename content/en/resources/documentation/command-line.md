---
title: "Diagnostics & Command Line"
description: "Troubleshoot networks using Stratum Firewall's built-in Wireshark packet capture, ARP/Wake-on-LAN, Web Activity, Stratum CLI tools, pfctl, and ZFS bectl."
lead: "Troubleshoot from the web UI or SSH — Wireshark .pcap capture, ARP & Wake-on-LAN, Web Activity, Stratum CLI utilities, and native FreeBSD tools."
---

Stratum Firewall equips administrators with rich browser-based diagnostics alongside clean command-line utilities on the underlying FreeBSD 15.1 system.

## 1. Built-In Web Diagnostics (`Diagnostics` Menu)

* **Packet Capture (`Diagnostics → Packet Capture`, `v0.25.1`):** Capture traffic on any interface into a downloadable `.pcap` file for **Wireshark** using structured protocol, host/network, and port filters (automatically excluding your own admin web/SSH session).
* **ARP Table &amp; Wake-on-LAN (`Diagnostics → ARP Table`, `v0.25.1`):** View every neighbor IP, MAC, interface, DHCP/DNS hostname, and randomized mobile MAC indicator—and wake sleeping machines with one-click **Wake-on-LAN**.
* **Web Activity &amp; DHCP History (`Diagnostics → Web Activity`, `v0.20.0`):** Audit per-device domain lookups (including OpenVPN usernames and WireGuard peer names) and exportable CSV DHCP/ARP history, with optional **Send all DNS to the firewall** redirection (plus DoT port 853 &amp; Firefox DoH canary blocking).
* **Live Firewall Log, States &amp; Network Tools:** Inspect real-time `filterlog` hits with matched rule names, search/kill active `pf` states, view 1h/24h/30d traffic graphs, and run Ping, Traceroute, and DNS lookups.

## 2. Stratum CLI Utilities

Connect via SSH or choose **`8) Shell`** from the console menu (`stratum-menu`):

| Command | Purpose |
| :--- | :--- |
| `stratum-menu` | Launch the interactive console setup and recovery menu (assign interfaces, set IPs, reset admin password, toggle WAN access). |
| `stratum-config` | Validate, pre-check, apply, or roll back `/usr/local/etc/stratum/config.json` from the command line. |
| `stratum-update` | Check for signed online updates (`stratum-update run`) or verify/apply updates with automatic ZFS Boot Environment creation. |
| `stratum-netextra` | Reconcile active Virtual IPs (VIPs) and static routes with the applied configuration. |
| `stratum-cert` | Inspect, generate self-signed, install, or restore the previous web panel TLS certificate (`show`, `selfsigned`, `install`, `restore`). |
| `stratum-ipbl` / `stratum-dnsbl` | Refresh IP threat feeds (`pf` tables) or Unbound RPZ DNS blocklists on demand. |

## 3. Essential FreeBSD 15.1 Commands (`pfctl` &amp; `bectl`)

```bash
# Inspect active pf filter, NAT rules, states, and blocklist table counts
pfctl -sr
pfctl -sn
pfctl -si
pfctl -t stratum_ipbl -T show | wc -l

# List ZFS Boot Environments created automatically before each update
bectl list
zfs list
```
