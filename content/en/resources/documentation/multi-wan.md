---
title: "Multi-WAN, Policy Routing & Virtual IPs"
description: "Configure 1-second gateway monitoring, automatic Multi-WAN failover, rule-based Policy Routing, Virtual IPs (VIPs), and Static Routes in Stratum Firewall."
lead: "Keep your network online across ISP outages and steer traffic per VLAN or rule — gateway monitoring, automatic failover, policy routing, Virtual IPs, and static routes."
---

Stratum Firewall provides complete multi-ISP survivability and multi-subnet routing through **Interfaces → Gateways**, **Interfaces → Virtual IPs**, **Interfaces → Routes**, and **Firewall → Rules**.

## 1. Real-Time Gateway Monitoring (`Interfaces → Gateways`, `v0.16.0`)

Every WAN-type interface (`WAN`, or an `OPT` interface assigned as WAN) automatically registers a gateway monitored **once per second** by Stratum's background gateway monitor:

* **Custom Monitor IP:** Probe your ISP's next-hop gateway or an external target (such as `1.1.1.1` or `8.8.8.8`) pinned strictly through that specific WAN link.
* **Configurable Thresholds:** Define packet-loss and latency limits (default `20%` loss or `500 ms` over 30 seconds marks a gateway `Down`; half thresholds trigger `Warning`) and inspect 1-hour latency/loss sparklines on the Gateways page and Dashboard.

## 2. Automatic WAN Failover &amp; Symmetric Reply Routing (`v0.16.1`)

* **Priority-Based Failover:** Assign a priority number to each WAN gateway. If the active default gateway goes down, Stratum immediately switches the default route to the highest-priority healthy gateway and optionally flushes stale states on the failed link.
* **Anti-Flap Hold-Down:** When the primary line recovers, it reclaims the default route only after remaining continuously healthy for the hold-down window (`60 seconds` by default).
* **Symmetric Inbound Replies (`reply-to`):** Inbound connections (port forwards, OpenVPN `multihome`, WAN rules) on *any* WAN automatically send return packets back out the exact same WAN they arrived on, regardless of which ISP currently holds the default route.

## 3. Per-Rule Policy Routing (`v0.27.2`)

Any **Pass** rule on an internal interface or [Interface Group](/resources/documentation/pf-rules/) can select a specific WAN gateway (static or PPPoE)—for example, routing a Guest Wi-Fi VLAN out `WAN2` while keeping servers on `WAN1`:

* **Automatic Local Subnet Bypass:** Traffic matching that rule destined for your own internal LANs/VLANs, WireGuard/OpenVPN networks, static routes, or the firewall itself automatically uses normal local routing instead of being sent out to the ISP.
* **Gateway Down Behavior:** Choose whether policy-routed traffic falls back to the default route (including Multi-WAN failover) within seconds when its chosen gateway is down, or stays strictly pinned to that gateway.

## 4. Virtual IPs &amp; Static Routes (`stratum-netextra`, `v0.27.0`)

* **Virtual IPs (`Interfaces → Virtual IPs`):** Bind additional public IPv4 addresses (such as a routed `/29` WAN subnet) to an interface. Target individual Virtual IPs in **Port Forwards** (with automatic NAT reflection) or translate internal subnets out specific Virtual IPs in **Outbound NAT**.
* **Static Routes (`Interfaces → Routes`):** Define routes to internal subnets behind downstream routers; Stratum automatically includes internal routed networks in outbound internet NAT.
* **Clean Kernel Reconciliation:** `stratum-netextra` synchronizes active kernel VIPs and routes at boot, Apply, rollback, and interface restarts without polluting `/etc/rc.conf`.
