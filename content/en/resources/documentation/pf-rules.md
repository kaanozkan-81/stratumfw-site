---
title: "pf Rules, NAT & Aliases"
description: "Configure FreeBSD pf firewall rules, Interface Groups, Port Forwards, NAT Reflection, Dynamic Hostname & License-Free GeoIP Country Aliases, and dummynet Bandwidth Shaping."
lead: "Control packet flow at wire speed — stateful pf rules, interface groups, dynamic GeoIP & Hostname aliases, NAT reflection, and per-device bandwidth limits."
---

Stratum Firewall compiles your declarative settings directly into FreeBSD's kernel-resident **pf (Packet Filter)** and **dummynet** traffic shaper—with live packet/byte hit counters, drag-and-drop ordering, and zero-lockout validation.

## 1. Rule Evaluation Order &amp; Interface Groups (`v0.17.0`)

![pf Rules](/images/docs/pf-rules.png)

Under **Firewall → Rules**, rules are evaluated **top-to-bottom (first match wins)** with a strict **Default Deny** baseline (plus a guaranteed **IPv6 safety drop baseline** until native IPv6 support is enabled):

1. **System &amp; Anti-Lockout Rules:** Shown transparently at the top of each interface tab (bogon/private WAN blocks, DHCP, anti-lockout).
2. **Interface Group Rules (`Interfaces → Groups`):** Group multiple interfaces (e.g., all internal VLANs) into a single tab; group rules evaluate *before* individual member interface rules while preserving per-WAN `reply-to`.
3. **Interface Rules:** Per-interface **Pass**, **Block** (silent drop), or **Reject** (`TCP RST` / `ICMP unreachable`) rules with one-click enable/disable switches and optional [Multi-WAN Policy Routing](/resources/documentation/multi-wan/) gateway selection.

## 2. Static, Hostname &amp; License-Free Country (GeoIP) Aliases (`v0.26.0`)

Keep rules clean and readable under **Firewall → Aliases**:

* **Hosts, Networks &amp; Ports:** Group IPs, CIDRs, and port ranges into reusable named aliases.
* **Hostname Aliases (FQDNs):** Enter domain names such as `update.example.com`. Stratum resolves them every **5 minutes** through its own local DNS resolver and retains resolved IPs for **60 minutes** after last seen so CDN IP rotation never drops active sessions.
* **Country (GeoIP) Aliases:** Restrict port forwards (e.g., *"only allow Türkiye"*) or block hostile regions using weekly IPv4 country blocks fetched directly from all five Regional Internet Registries (**ARIN, RIPE NCC, APNIC, LACNIC, AFRINIC**)—100% free and with **no external license key required**.

Both Hostname and Country aliases back live file-based `pf` tables that refresh in memory without requiring a firewall Apply.

## 3. Port Forwards, Outbound NAT &amp; NAT Reflection

Under **Firewall → NAT**:

* **Port Forwards:** Forward incoming WAN (or [Virtual IP](/resources/documentation/multi-wan/)) ports to internal servers with automatic pass rules and hit counters. Enabling **NAT Reflection** (`System → Settings → Firewall & NAT`) allows internal LAN clients to reach forwarded services via your public IP while still enforcing interface filter rules.
* **Outbound NAT:** Choose **Automatic** (translates all LANs, VLANs, VPNs, and static internal routes), **Hybrid** (automatic + custom rules, e.g., translating a server out a dedicated Virtual IP), or **Manual** mode, with optional static source ports for gaming/VoIP.

## 4. Bandwidth Limits (`Firewall → Shaper`, `v0.18.0`)

Throttle download and upload rates using FreeBSD's kernel **dummynet** pipes directly from `pf`:
* Set a **shared network limit** (e.g., `250 Mbit/s` total for an office VLAN) or a **per-device limit** (e.g., `20 Mbit/s` per guest phone) in either direction.
