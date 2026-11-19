---
title: "Services > NTP"
seo_title: "07.4 Services > NTP — Stratum FW User Manual"
description: "Keeping the firewall's clock right: time servers, time zone, serving time to internal networks, and the synchronisation status."
weight: 704
chapter_num: "07.4"
url: "/manual/services-ntp/"
aliases:
  - "/manual/07-04-services-ntp/"
translationKey: "manual-07-04-services-ntp"
---

A correct clock matters on a firewall more than anywhere: log entries are compared by their time, schedules
switch rules on time, certificates and two-factor codes are only valid within their time, and a wrong clock can
even stop VPN connections. **Services > NTP** keeps the clock synchronised with time servers on the internet and
can serve the time to your networks.

![Services > NTP](/images/manual/ntp.png)

## Settings

| Setting | Meaning |
| :--- | :--- |
| **Time servers** | One per line, up to six. Default `pool.ntp.org`. Pool names - such as `tr.pool.ntp.org` or `de.pool.ntp.org`, a pool of the country you are in - connect to several servers at once, which is more reliable than a single server. |
| **Timezone** | Used for the system clock, log timestamps and scheduled tasks. The same setting as on **System > Settings > General**. |
| **Act as time server for internal networks** | Devices on LAN-type interfaces can get the time from the firewall, and DHCP clients are told to use it automatically. Requests from the WAN are always refused. Off by default. |

Click **Save settings**, **Apply changes** and **Keep changes**.

> **Tip:** Switch **Act as time server** on in networks with cameras, recorders, access control or industrial
> devices. They often cannot reach the internet - and should not - but still need the right time for their
> recordings and logs.

## Status

The **Status** card shows the **System time**, whether the clock is synchronised, and one line per time server:

| Column | Meaning |
| :--- | :--- |
| **Server** | The server's address. |
| **State** | **Selected** is the server the clock follows; others are candidates or rejected. |
| **Reference** | Where that server gets its own time from. |
| **Stratum** | Its distance from a reference clock (1 = directly attached to one). |
| **Reach** | Whether recent requests were answered (377 = all of the last eight). |
| **Delay**, **Offset**, **Jitter** (ms) | Round-trip time to the server, the difference between its clock and the firewall's, and how much that varies. |

The card refreshes itself every 10 seconds. Right after a start, it takes a few minutes until a server is
selected.

**If no server is ever reached:** the firewall needs outgoing UDP port 123. Check the WAN connection, and on a
network behind another firewall, that NTP is allowed out there.
