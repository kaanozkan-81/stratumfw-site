---
title: "Diagnostics > Web Activity"
seo_title: "10.5 Diagnostics > Web Activity — Stratum FW User Manual"
description: "Which sites the devices look up, per device and per site, the DHCP history of who held which address, forcing all DNS through the firewall, retention and legal records for public networks."
weight: 1005
chapter_num: "10.5"
url: "/manual/diagnostics-web-activity/"
aliases:
  - "/manual/10-05-diagnostics-web-activity/"
translationKey: "manual-10-05-diagnostics-web-activity"
---

Web Activity records **which sites the devices in your networks look up** - per device and per day - and **which
device held which address when**. It answers questions such as *"which device keeps contacting this site?"*,
*"what did 192.168.1.57 do yesterday?"* or, for a public network, *"who had this address at that time?"*.

The page has four tabs: **Devices**, **Sites**, **DHCP history** and **Settings**.

> **Privacy:** Web Activity shows what people in your network do. Only administrators can open these pages, and
> every look at the data is written to the system log. Inform your users as your local law requires.

## How it works

Every device asks the firewall's DNS resolver for the name of a site before it connects. Web Activity counts these
lookups per device and day: which names, how often, first and last time.

- It records **names**, not pages: `www.example.com`, not what was read there. Connections by address only, without
  a lookup, are not seen.
- It needs the **Unbound** resolver ([**Services > DNS Resolver**](/manual/services-dns-resolver/)); the page says so if dnsmasq is in use.
- Devices that use another DNS server, or DNS over HTTPS built into an app, are not seen - see **Send all DNS to
  the firewall** below.

## Settings

![Diagnostics > Web Activity > Settings](/images/manual/webact-settings.png)

| Setting | Meaning |
| :--- | :--- |
| **Record which sites the devices look up** | Switches Web Activity on. Off by default. |
| **Send all DNS of the internal networks to the firewall and block DNS over TLS (port 853)** | Catches devices set to another DNS server (8.8.8.8 and the like): their queries are answered by the firewall without them noticing. It also tells Firefox not to use its own DNS over HTTPS (`use-application-dns.net`). Other browsers and apps with DNS over HTTPS built in cannot be seen this way. This is **Force DNS** ([**Services > DNS Resolver**](/manual/services-dns-resolver/)) for every internal network at once. |
| **Keep the records (days)** | 1-730, default 30. Applies to the sites and the DHCP history; older records are removed. |

Takes effect after **Apply**.

> **Public networks:** hotels, cafés and other public networks may be required by law to keep address
> assignments for a set period - in Turkey by law no. 5651. Set **Keep the records** accordingly, and consider
> sending the logs to a syslog server as well ([**System > Settings > Logging**](/manual/system-settings-logging-notifications/)).

## Devices

![Diagnostics > Web Activity > Devices](/images/manual/webact-devices.png)

Every device that looked something up in the period: **Device** (host name and MAC address), **Address**, number of
**Sites**, **Lookups** and **Last seen**.

- Period: **Today**, **Last 7 days**, **Last 30 days**, **Last 90 days** or **Last year** (within the retention).
- **All networks** or one; **Search** for an address, name or MAC address.

Click a device to see **its sites**: **Site**, the **Names looked up** in it (for example `www.`, `api.`,
`cdn.`), **Lookups**, **First** and **Last**. The page says which device held the address - *Held by ...* - and
whether other devices held it before in the period.

## Sites

The same the other way round: every site with the number of **Devices**, **Lookups** and **Last seen**. **Search
site** finds one. Click a site to see **which devices** looked it up, with **Lookups**, **First** and **Last**.

Names are grouped into sites by their registrable domain - `www.example.com` and `cdn.example.com` are both
*example.com*, `news.bbc.co.uk` is *bbc.co.uk*.

## DHCP history

Which device (**MAC address**, **Host name**) held each **Address** and when - **From** and **Until**. Search by
**Address** or **MAC address**.

It also covers devices with a fixed address that never ask DHCP: they are recorded from the ARP table.

This is the record to answer *"who had 192.168.20.57 on 3 March at 21:15?"*.

## Typical questions

| Question | Where |
| :--- | :--- |
| A device contacts a suspicious site (an IDS alert, a blocklist hit) - which one? | **Sites** > the site > its devices |
| What does this device do? Is it a camera calling home to a cloud? | **Devices** > the device |
| Who had this address at that time? | **DHCP history**, then the device on **Devices** |
| Which sites use most lookups in the guest network? | **Sites**, filtered by the network |

To block sites, use [**Firewall > Web Filter**](/manual/firewall-web-filter/) (categories) or [**Firewall > Blocklists**](/manual/firewall-blocklists/) (DNS lists).
