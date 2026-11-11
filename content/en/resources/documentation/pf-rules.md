---
title: "pf Rules, Schedules, 1:1 NAT & Aliases"
description: "Configure FreeBSD pf firewall rules, Time-Based Rule Schedules, Bidirectional 1:1 NAT, Interface Groups, Port Forwards, NAT Reflection, Dynamic Hostname & License-Free GeoIP Country Aliases, and dummynet Bandwidth Shaping."
lead: "Control packet flow at wire speed — stateful pf rules, minute-accurate time schedules, bidirectional 1:1 NAT, dynamic GeoIP & Hostname aliases, and per-device bandwidth limits."
aliases:
  - /resources/documentation/firewall-rules/
---

Stratum Firewall compiles your declarative settings directly into FreeBSD's kernel-resident **pf (Packet Filter)** and **dummynet** traffic shaper—with live packet/byte hit counters, drag-and-drop ordering, and zero-lockout validation.

## 1. Rule Evaluation Order, WAN Log Noise &amp; Interface Groups (`v0.17.0`, `v0.28.4`)

![pf Rules](/images/docs/pf-rules.png)

Under **Firewall → Rules**, rules are evaluated **top-to-bottom (first match wins)** with a strict **Default Deny** baseline (plus a guaranteed **IPv6 safety drop baseline** until native IPv6 support is enabled):

1. **System &amp; Anti-Lockout Rules:** Shown transparently at the top of each interface tab (bogon/private WAN blocks, DHCP, anti-lockout).
2. **Interface Group Rules (`Interfaces → Groups`):** Group multiple interfaces (e.g., all internal VLANs) into a single tab; group rules evaluate *before* individual member interface rules while preserving per-WAN `reply-to`.
3. **Interface Rules:** Per-interface **Pass**, **Block** (silent drop), or **Reject** (`TCP RST` / `ICMP unreachable`) rules with one-click enable/disable switches and optional [Multi-WAN Policy Routing](/resources/documentation/multi-wan/) gateway selection.
4. **WAN Default-Block Noise Control (`System → Settings → Firewall & NAT`, `v0.28.4`):** Uncheck **"Also on the WAN"** under default-block logging to silence constant internet background scans and late closed-connection replies on WAN while still logging blocked packets on internal LANs, VLANs, and VPNs.

## 2. Time-Based Firewall Schedules (`Firewall → Schedules`, `v0.29.0`)

Define reusable named time windows—such as `"Mon-Fri 08:00-18:00; Sat 09:00-13:00"`—and attach them to any `Pass`, `Block`, or `Reject` rule:

* **True pfSense-Style Presence:** A rule with a schedule only exists during its active window. Outside its times, a `pass` rule stops allowing traffic and a `block` rule stops blocking it—the rules below decide.
* **Automatic Open-State Teardown:** The moment a schedule window ends, **any open connections allowed by its `pass` rules are closed automatically** so sessions cannot linger overnight.
* **Zero-Apply Minute Evaluation (`stratum-pfrefresh`):** Rules follow the firewall's local time zone clock within one minute without needing an Apply; active schedules display a green badge in the rule list alongside next start/end times.

## 3. Static, Hostname &amp; License-Free Country (GeoIP) Aliases (`v0.26.0`)

Keep rules clean and readable under **Firewall → Aliases**:

* **Hosts, Networks &amp; Ports:** Group IPs, CIDRs, and port ranges into reusable named aliases.
* **Hostname Aliases (FQDNs):** Resolved every **5 minutes** via Stratum's local DNS resolver and retained for **60 minutes** after last seen so CDN IP rotation never drops active sessions.
* **Country (GeoIP) Aliases:** Restrict port forwards (e.g., *"only allow Türkiye"*) or block hostile regions using weekly IPv4 country blocks fetched directly from all five Regional Internet Registries (**ARIN, RIPE NCC, APNIC, LACNIC, AFRINIC**)—100% free with **no external license key required**.

## 4. Port Forwards, Bidirectional 1:1 NAT &amp; Outbound NAT (`v0.28.9`)

Under **Firewall → NAT**, NAT precedence follows **Port Forwards → 1:1 NAT → Outbound NAT**:

* **Port Forwards &amp; NAT Reflection:** Forward incoming WAN (or [Virtual IP](/resources/documentation/multi-wan/)) ports to internal servers with automatic pass rules, hit counters, and optional **NAT Reflection**.
* **Bidirectional 1:1 NAT (`Firewall → NAT → 1:1`, `v0.28.9`):** Map an internal host (or subnet) 1:1 to a dedicated public [Virtual IP](/resources/documentation/multi-wan/) on WAN (or a routed public subnet of matching size) in both directions:
  * **Anti-Lockout Guard:** The WAN interface's own primary IP is refused so management access to the firewall itself is never lost.
  * **Explicit WAN Rules:** Like pfSense, 1:1 NAT opens nothing by itself—inbound traffic requires an explicit WAN `pass` rule to the internal IP (the 1:1 table shows how many WAN rules permit traffic to each host, or `"closed"` when none).
* **Outbound NAT:** Choose **Automatic**, **Hybrid** (automatic + custom VIP rules), or **Manual** mode, with optional static source ports for gaming/VoIP.

## 5. Bandwidth Limits (`Firewall → Shaper`, `v0.18.0`)

Throttle download and upload rates using FreeBSD's kernel **dummynet** pipes directly from `pf`—either as a **shared network limit** (e.g., `250 Mbit/s` total for an office VLAN) or a **per-device limit** (e.g., `20 Mbit/s` per guest client).
