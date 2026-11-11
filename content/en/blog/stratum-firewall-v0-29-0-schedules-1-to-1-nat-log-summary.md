---
title: "Stratum Firewall v0.29.0: Time-Based Rule Schedules, 1:1 NAT, and Firewall Log Analytics"
date: 2026-11-11
author: "Stratum Security"
translationKey: "v0-29-0-release"
description: "Stratum Firewall v0.29.0 introduces time-based firewall schedules with automatic state teardown, bidirectional 1:1 NAT on Virtual IPs, interactive Firewall Log Summary analytics, WAN scan log suppression, and PC speaker boot tunes."
lead: "Spanning releases v0.28.4 through v0.29.0, Stratum Firewall adds enterprise NAT and scheduling controls, visual traffic log intelligence, cleaner WAN logging, and classic hardware appliance boot chimes."
tags: ["stratum firewall", "freebsd 15.1", "pf", "1:1 nat", "firewall schedules", "log analytics", "v0.29.0"]
---

Following our unified blocklist reports and policy routing release, **Stratum Firewall v0.29.0** brings six focused updates (`v0.28.4` through `v0.29.0`) built for offices, campuses, and multi-IP server deployments running on [Stratum Firewall](https://stratumfw.com/). Every detail is available in the [Official Changelog](https://stratumfw.com/resources/changelog/).

### 1. Time-Based Firewall Schedules (`v0.29.0`)

Administrators can now define reusable time windows under **Firewall -> Schedules** (such as `Mon-Fri 08:00-18:00; Sat 09:00-13:00`) and attach them to any `pass` or `block` rule:

* **Clock-Driven Enforcement Without Manual Apply:** Managed by `stratum-pfrefresh` every minute alongside policy routing, rules activate and deactivate automatically in the appliance's local time zone.
* **Automatic State Cleanup:** When a scheduled `pass` window closes, existing connections opened by that rule are terminated immediately so sessions cannot linger overnight.
* **Safe OPNsense Import:** Scheduled rules imported from an OPNsense `config.xml` are brought in disabled and highlighted in the migration summary so time-restricted rules never accidentally stay open 24/7.

### 2. Bidirectional 1:1 NAT for Public Subnet Blocks (`v0.28.9`)

Organizations with `/29` or `/28` public IP blocks or routed subnets can now map internal servers 1:1 to dedicated external addresses under **Firewall -> NAT -> 1:1** ([Features](https://stratumfw.com/product/features/)):

* **Symmetric Addressing with Strict Firewall Control:** Mapped hosts egress using their dedicated public IP while inbound traffic still obeys explicit WAN `pass` rules (displaying live rule counts or a `closed` indicator).
* **Anti-Lockout Validation:** The WAN's primary interface IP is refused for 1:1 NAT so administrative reachability to the appliance itself is protected.

### 3. Interactive Firewall Log Summary (`v0.28.8`) & WAN Noise Control (`v0.28.4`)

**Diagnostics -> Firewall Log -> Summary** adds an instant 1-hour and 24-hour visual breakdown beside the live packet stream:

* **Top Offenders, Countries & Services:** View top source IPs, countries (powered by license-free 5-RIR GeoIP data), target ports (`SSH`, `RDP`, etc.), matched rules, and per-interface 5-minute or hourly histograms—clicking any IP or port filters the live log immediately.
* **Quieter WAN Logs:** A new **Also on the WAN** toggle under **System -> Settings -> Firewall & NAT** lets you silence background internet scanner noise on WAN default-block rules while still logging blocked internal VLAN and VPN traffic.

### 4. Hardware Appliance Boot & Shutdown Tunes (`v0.28.5` – `v0.28.6`)

Bare-metal x86-64 appliances equipped with a PC speaker now play rising start-up and falling shutdown melodies—selectable between **Stratum**, **Classic beep**, **Westminster Chime**, **Ode to Joy**, **Für Elise**, and **Arcade** with an instant **Test** button under **System -> Settings -> General**.

### Upgrade in Seconds or Explore Live

Installed appliances can update directly to **v0.29.0** via **System -> Update** with an automatic bootable ZFS Boot Environment snapshot. New to Stratum? Test-drive the live FreeBSD 15.1 VM at [demo.stratumfw.com](https://demo.stratumfw.com/) or grab the installers on the [Download](https://stratumfw.com/download/) page.
