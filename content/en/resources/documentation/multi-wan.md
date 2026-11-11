---
title: "Multi-WAN (DHCP/PPPoE/Static), Policy Routing & Virtual IPs"
description: "Configure 1-second gateway monitoring, dynamic DHCP secondary WANs (cable, 4G/5G, Starlink), automatic Multi-WAN failover, rule-based Policy Routing, Virtual IPs (VIPs), and Static Routes in Stratum Firewall."
lead: "Keep your network online across ISP outages and steer traffic per VLAN or rule — 1-second gateway monitoring, dynamic DHCP additional WANs, automatic failover, policy routing, Virtual IPs, and static routes."
---

Stratum Firewall provides complete multi-ISP survivability and multi-subnet routing through **Interfaces**, **Interfaces → Gateways**, **Interfaces → Virtual IPs**, **Interfaces → Routes**, and **Firewall → Rules**.

## 1. Static, PPPoE &amp; Dynamic DHCP Additional WANs (`v0.29.1`)

Every WAN-type interface (`WAN`, or an `OPT` interface assigned as an additional WAN) supports **Static IPv4**, **PPPoE (`mpd5`)**, and **Dynamic DHCP (`v0.29.1`)**:

* **Cable, 4G/5G LTE &amp; Starlink Secondary WANs:** An additional DHCP WAN (`WAN2`, `WAN3`) obtains its IP and gateway lease automatically **without hijacking your default route or system DNS servers**—which WAN carries the default route remains strictly the gateway monitor's decision.
* **Zero-Apply Lease Gateway Tracking (`stratum-pfrefresh`):** The gateway from the DHCP lease is automatically wired into failover, inbound symmetric replies (`reply-to`), and per-rule Policy Routing. Whenever the ISP hands out a new gateway IP on lease renewal, firewall rules follow **within seconds without needing an Apply**.

## 2. Real-Time Gateway Monitoring (`Interfaces → Gateways`, `v0.16.0`)

Each WAN gateway is probed **once per second** by Stratum's background gateway monitor:

* **Custom Monitor IP:** Probe your ISP's next-hop gateway or an external target (such as `1.1.1.1` or `8.8.8.8`) pinned strictly through that specific WAN link.
* **Configurable Thresholds:** Define packet-loss and latency limits (default `20%` loss or `500 ms` over 30 seconds marks a gateway `Down`; half thresholds trigger `Warning`) and inspect 1-hour latency/loss sparklines on the Gateways page and Dashboard.

## 3. Automatic WAN Failover &amp; Symmetric Reply Routing (`v0.16.1`)

* **Priority-Based Failover &amp; Anti-Flap Hold-Down:** If the active default gateway goes down, Stratum immediately switches the default route to the highest-priority healthy gateway (and optionally flushes stale states). When the primary line recovers, it reclaims the default route only after staying healthy for the hold-down window (`60 seconds` by default).
* **Symmetric Inbound Replies (`reply-to`):** Inbound connections (port forwards, [1:1 NAT](/resources/documentation/pf-rules/), OpenVPN `multihome`, WAN rules) on *any* WAN—static, PPPoE, or DHCP—always send return packets back out the exact WAN they arrived on.

## 4. Per-Rule Policy Routing (`v0.27.2`, `v0.29.1`)

Any **Pass** rule on an internal interface or [Interface Group](/resources/documentation/pf-rules/) can select a specific WAN gateway (including a **DHCP WAN**—until its initial lease arrives, the rule gracefully uses normal routing):

* **Automatic Local Subnet Bypass:** Traffic destined for your own internal LANs/VLANs, WireGuard/OpenVPN networks, static routes, or the firewall itself automatically stays on local routing instead of exiting to the ISP.
* **Gateway Down Behavior:** Choose whether policy-routed traffic falls back to the active default route within seconds when its chosen gateway goes down, or stays strictly pinned to that gateway.

## 5. Virtual IPs &amp; Static Routes (`stratum-netextra`, `v0.27.0`)

* **Virtual IPs (`Interfaces → Virtual IPs`):** Bind additional public IPv4 addresses (such as a routed `/29` WAN subnet) to an interface for **Port Forwards**, **[Bidirectional 1:1 NAT](/resources/documentation/pf-rules/)**, or **Outbound NAT**.
* **Static Routes (`Interfaces → Routes`):** Define routes to internal subnets behind downstream routers; `stratum-netextra` reconciles VIPs and routes cleanly in the kernel without cluttering `/etc/rc.conf`.
