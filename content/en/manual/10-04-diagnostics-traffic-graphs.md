---
title: "Diagnostics > Traffic Graphs"
seo_title: "10.4 Diagnostics > Traffic Graphs — Stratum FW User Manual"
description: "Bandwidth graphs per interface for the last hour, 24 hours and 30 days, with average and peak rates."
weight: 1004
chapter_num: "10.4"
url: "/manual/diagnostics-traffic-graphs/"
aliases:
  - "/manual/10-04-diagnostics-traffic-graphs/"
translationKey: "manual-10-04-diagnostics-traffic-graphs"
---

[**Diagnostics > Traffic Graphs**](/manual/diagnostics-traffic-graphs/) shows how much traffic each interface carries - incoming and outgoing - over the
**last hour**, **24 hours** or **30 days**.

![Diagnostics > Traffic Graphs](/images/manual/traffic.png)

## The graphs

One graph per interface, with **In** and **Out**:

| Range | Resolution | Kept |
| :--- | :--- | :--- |
| **1 hour** | 10 seconds | in memory (starts empty after a restart) |
| **24 hours** | 5 minutes, average and peak | on disk |
| **30 days** | 1 hour, average and peak | on disk |

*In* and *Out* are seen from the firewall's interface: on a **WAN**, *In* is your download from the internet; on a
**LAN**, *In* is what the devices send - their upload.

The **peak** shows short bursts that the average hides - for example whether a 100 Mbit/s line is really full at
times, or only on average half used.

The [dashboard](/manual/dashboard/) shows the current rate of every interface; the AI Assistant and MCP can report averages and peaks
(*"which interface was busiest yesterday?"*).

> **If the page says the traffic collector is not running**, apply the pending changes (it is started
> automatically), or run `service stratum_traffic start` on the console.

## Per device

The graphs are per interface. To see **which device** uses the bandwidth:

- [**Diagnostics > States**](/manual/diagnostics-tools-states-arp/) - search for the network and look at the bytes of its connections;
- [**Diagnostics > Web Activity**](/manual/diagnostics-web-activity/) - which devices visit which sites;
- an SNMP monitoring system for long-term graphs ([**System > Settings > SNMP**](/manual/system-settings-snmp-mcp/)).

To limit bandwidth, see [**Firewall > Shaper**](/manual/firewall-shaper/).
