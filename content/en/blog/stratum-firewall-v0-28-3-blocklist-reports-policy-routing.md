---
title: "Stratum Firewall v0.28.3: Unified Blocklist Reports, Multi-WAN Policy Routing, and Service Control"
date: 2026-11-11
author: "Stratum Security"
translationKey: "v0-28-3-release"
description: "Stratum Firewall v0.28.3 introduces pfBlockerNG-style unified DNSBL and IP blocklist reports, Multi-WAN policy routing, centralized service status management, and per-user notifications."
lead: "Spanning releases v0.27.2 through v0.28.3, Stratum Firewall brings deep visibility into blocked DNS and IP traffic, rule-based Multi-WAN policy routing, live service management, and a faster configuration workflow."
tags: ["stratum firewall", "stratum fw", "freebsd", "pfblockerng", "dnsbl", "policy routing", "multi-wan", "unbound"]
---

With **Stratum Firewall v0.28.3**, day-to-day network visibility, multi-ISP traffic steering, and appliance administration take a major step forward on [Stratum Firewall](https://stratumfw.com/). Covering the rapid release sequence from `v0.27.2` to `v0.28.3` detailed in our [Changelog](https://stratumfw.com/resources/changelog/), this update delivers pfBlockerNG-style unified blocklist reporting, rule-based Policy Routing, centralized service supervision, and streamlined UI workflows.

### 1. Unified Blocklist Reports & Dashboard Widget (v0.28.2 & v0.28.3)

Blocking hundreds of thousands of malicious domains and threat IPs is only half the story—administrators also need instant answers when investigating what was stopped and which client triggered it. **Firewall > Blocklists > Blocked** introduces a comprehensive, pfBlockerNG-inspired reporting suite ([Features](https://stratumfw.com/product/features/)):

* **DNSBL, Web Filter & IP Blocklists in One View:** Inspect inbound and outbound `pf` IP blocklist hits alongside Unbound DNS blocklist and category Web Filter blocks with hourly and daily trend charts ([Changelog](https://stratumfw.com/resources/changelog/)).
* **List Attribution & Top Offenders:** See exactly which feed or custom blocklist stopped each request, review the most frequently blocked domains and IP addresses, and identify internal devices generating the highest block counts over the last 1 hour, 24 hours, or 7 days.
* **Instant Filtering & Dashboard Panel:** Search and filter every event by block type, list, domain, IP address, or device, and add the new **Blocklists** widget from **Dashboard > Panels** for a 24-hour live overview on your home screen.
* **Unbound 1.26 Precision & Privacy:** Collected every minute from Unbound's dedicated block log (even when full Web Activity logging is disabled) and the firewall log, `v0.28.3` adds full parser support for Unbound 1.26's `rpz-nxdomain` rule prefix format while ensuring allowed `rpz-passthru` queries are never counted as blocks. Access is restricted to administrators and every report view is audit-logged.

### 2. Multi-WAN Policy Routing (v0.27.2)

Multi-ISP deployments can now steer specific traffic flows across designated WAN links directly from firewall pass rules ([Architecture](https://stratumfw.com/product/architecture/)):

* **Per-Rule WAN Selection:** Any pass rule on an internal interface or interface group can route matching traffic through a specific static-gateway or PPPoE WAN—for instance, sending a Guest Wi-Fi VLAN out a secondary broadband line while keeping production traffic on your primary fiber ([Changelog](https://stratumfw.com/resources/changelog/)).
* **Automatic Local Subnet Bypass:** Traffic matching a policy-routed rule that targets internal LANs/VLANs, WireGuard/OpenVPN subnets, static routes, or the firewall itself automatically retains normal local routing rather than being hairpinned toward the upstream ISP gateway.
* **Live Failover & OPNsense Import Support:** When a chosen WAN gateway goes down, the gateway monitor switches rules to the default route (including Multi-WAN failover) within seconds without dropping open states—or keeps traffic pinned to that gateway if configured. **Import from OPNsense** also carries rule gateways over to the matching WAN automatically.

### 3. Centralized Service Status & Safe Process Control (v0.28.1)

**Services > Service Status** consolidates every daemon on the appliance into a single operational view ([Changelog](https://stratumfw.com/resources/changelog/)):

* **Live Process Metrics:** Check whether each service is running, stopped, or switched off, alongside its uptime, memory footprint, and process ID (`PID`).
* **Guarded Start, Stop & Restart Controls:** Administrators can manage services directly with two-click confirmation before stopping a daemon. Services disabled in configuration link directly to their settings page instead of starting unconfigured, while core infrastructure services (`pf`, Nginx, the web panel, SSH, syslog, and the scheduler) safely expose restart-only controls—restarting `pf` reloads rules and repopulates IP blocklist tables cleanly.

### 4. Per-User Notifications & Sticky Apply Workflow (v0.28.0)

Two quality-of-life improvements make multi-admin operations smoother ([Changelog](https://stratumfw.com/resources/changelog/)):

* **Personal Notification Lists:** In the top-right notification bell, marking items as read, removing alerts, or clearing the list is now stored per user—clearing your own notifications never wipes another administrator's unread queue or the underlying event history.
* **Inline Apply Button & Sticky Confirmation Bar:** Saving an actual configuration change now reveals an **Apply changes** button right beside **Save** (just like OPNsense and pfSense), and the **Apply changes / Keep changes** banner stays pinned below the top navigation bar as you scroll long rule tables.

### Upgrade Today or Test-Drive the Live Demo

Existing [Stratum Firewall](https://stratumfw.com/) appliances can upgrade to **v0.28.3** directly from **System > Update**, which automatically snapshots the current system into a ZFS boot environment before applying the signed update. Want to explore the interface first? Jump into our isolated FreeBSD 15.1 [Live Demo](https://demo.stratumfw.com/) or grab the installer images from the [Download](https://stratumfw.com/download/) page.
