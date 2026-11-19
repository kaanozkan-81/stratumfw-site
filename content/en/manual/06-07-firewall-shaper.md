---
title: "Firewall > Shaper"
seo_title: "06.7 Firewall > Shaper — Stratum FW User Manual"
description: "Download and upload limits per internal network - shared by the whole network or for each device - with dummynet, and how they combine with the captive portal's per-device limits."
weight: 607
chapter_num: "06.7"
url: "/manual/firewall-shaper/"
aliases:
  - "/manual/06-07-firewall-shaper/"
translationKey: "manual-06-07-firewall-shaper"
---

The Shaper limits how much bandwidth an internal network may use, in each direction. Typical uses:

- the guest Wi-Fi together never uses more than 100 Mbit/s, so the staff network always has room;
- every guest phone gets at most 20 Mbit/s, so one download does not slow everybody else down;
- a camera network's uploads stay below what the line can carry.

The limits use FreeBSD's **dummynet** through the packet filter. Nothing else changes in your firewall rules.

![Firewall > Shaper](/images/manual/fw-shaper.png)

## Set a limit

**Firewall > Shaper** lists every internal network in one table. For each network:

| Column | Meaning |
| :--- | :--- |
| **Network** | The internal interface (one limit per interface). |
| **On** | Switches the limit on. |
| **Download (Mbit/s)** | What the devices receive. Empty or 0: no limit in that direction. Decimal values are allowed (from 0.1 to 100000). |
| **Upload (Mbit/s)** | What the devices send. Empty or 0: no limit. |
| **Applies to** | **Whole network** - all devices share the limit, for example the office VLAN together never uses more than 255 Mbit/s. **Each device** - every device gets the limit on its own, for example 20 Mbit/s per guest phone. |
| **Note** | Your description. |

A limit needs at least one direction. Click **Save**, then **Apply changes** and **Keep changes**.

## How the limits behave

- **Download** is what the devices of the network receive, **upload** what they send - always seen from the
  devices.
- **Traffic between two local networks counts too.** A file copied from the server network to a limited guest
  network is held to the guest limit as well.
- **Each device** gives every address in the network its own queue at the limit's rate; the limit does not grow
  or shrink with the number of devices. **Whole network** shares one queue between them.
- Limits are rates, not quotas: nothing is counted over a day or a month.

> **Tip:** To keep a shared line responsive, give the busy networks a *whole network* limit a little below the
> line's real capacity (for example 90 % of the measured speed). When the queue sits on the firewall instead of
> in the provider's modem, latency stays low for everybody.

## With the captive portal

The captive portal can set its own per-device limits - the zone's download and upload limits, or the values a
RADIUS server sends for a user (**Services > Captive Portal**). For a signed-in device, the portal's limit takes
the place of the Shaper limit of its network. Use one of the two for a given network to keep the behaviour easy
to predict.

## See the limits at work

The [dashboard](/manual/dashboard/) panel **Bandwidth limits** shows each limit with the network's current rate. **Diagnostics >
Traffic Graphs** shows the traffic of an interface over time.
