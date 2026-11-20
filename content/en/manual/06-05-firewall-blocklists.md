---
title: "Firewall > Blocklists"
seo_title: "06.5 Firewall > Blocklists — Stratum FW User Manual"
description: "DNS blocklists (ads, tracking, malware, phishing) and IP blocklists (botnets, scanners, attackers): choosing lists, updates, allow and block entries, checking a domain or address, and the Blocked report."
weight: 605
chapter_num: "06.5"
url: "/manual/firewall-blocklists/"
aliases:
  - "/manual/06-05-firewall-blocklists/"
translationKey: "manual-06-05-firewall-blocklists"
---

Blocklists stop known bad destinations and sources using lists published by threat-intelligence projects.
Stratum FW has two kinds, on two tabs of **Firewall > Blocklists**:

| Tab | Blocks | How | Catalogue |
| :--- | :--- | :--- | :--- |
| **DNSBL** | Domains: ads, tracking, malware, phishing, crypto-mining ... | The firewall's DNS resolver answers *does not exist* (NXDOMAIN) for a blocked name and all its subdomains. | 93 lists |
| **IP** | Addresses: botnet controllers, scanners, brute-force attackers, spam sources, compromised hosts ... | The packet filter drops traffic to and from the listed addresses. | 63 lists |

They work for every device in your networks - phones, TVs, IoT devices - with nothing to install on them.

A third tab, **Blocked**, is the report of what both have stopped (administrators only).

## DNS blocklists (DNSBL)

> **Note:** DNS blocklists work inside **Unbound**. Choose **Unbound** as the DNS resolver under
> **Services > DNS Resolver** and apply first; until then the switch on this tab cannot be turned on.

![Firewall > Blocklists > DNSBL](/images/manual/bl-dnsbl.png)

### Switch on and choose lists

1. Switch on **Block the domains in the selected lists**.
2. Choose the lists. They are grouped by category (EasyList and regional lists, malicious domains, Firebog's
   advertising, tracking and suspicious lists, phishing, crypto-mining, DNS over HTTPS servers, HaGeZi ...). Lists
   marked **recommended** are a good start: StevenBlack (ads), Disconnect.me malvertising, Spam404, OpenPhish,
   NoCoin, Dandelion Sprouts and HaGeZi Multi Normal.
3. Set **Automatic updates** (see below) and click **Save**.
4. **Apply changes**. The lists are downloaded and loaded.

> **Tip:** More lists are not always better. Large advertising and tracking lists also block things users want -
> links in marketing e-mails, shopping comparison sites, some apps. Start with the recommended lists, watch the
> Blocked report for a week, then add.

**Getting around DNS blocks.** A device that uses its own DNS server (`8.8.8.8`, DNS over HTTPS in a browser)
bypasses the firewall's resolver. To prevent that:

- force the DNS of chosen networks to the firewall (**Services > DNS Resolver**, or *Send all DNS to the
  firewall* on [**Diagnostics > Web Activity**](/manual/diagnostics-web-activity/)) - devices then cannot reach another DNS server on port 53;
- add the **DoH** category, which blocks the names of public DNS-over-HTTPS servers, so browsers fall back to
  normal DNS.

### Your own entries

| Field | Meaning |
| :--- | :--- |
| **Allow list** | Domains that are never blocked, one per line - whatever a list contains. A domain allows its subdomains too. |
| **Block list** | Domains that are always blocked, one per line. |
| **Custom lists** | Lists from other sources, one per line as `name \| https://address`. Hosts-file and plain domain-list formats are understood. |

The firewall's own update servers and the list sources are never blocked, so a list can never cut off the
firewall's updates.

### Check a domain

**Check a domain** tells you whether a name is blocked, and by which list - the quickest answer to *"why does
this site not open?"*. If a list blocks something it should not, add the name to the **Allow list**.

## IP blocklists

![Firewall > Blocklists > IP](/images/manual/bl-ip.png)

1. Switch on **Block the addresses in the selected lists**.
2. Choose the lists. The **recommended** ones block what no network wants to talk to: abuse.ch Feodo Tracker
   (botnet command-and-control servers), Emerging Threats block and compromised-host lists, and Spamhaus DROP
   (networks run by criminals). Further categories cover scanners, blocklist.de's attack reports, mail spam,
   Tor exit nodes, open proxies, VPN services, DNS-over-HTTPS servers and torrent trackers - for networks with
   stricter policies.
3. **Direction:**
   - **Both directions (recommended)** - attackers from the internet are dropped on the WAN, and a device inside
     that tries to reach a listed address is stopped as well - often the first sign of an infected device.
   - **Incoming only (from the internet)** or **Outgoing only (from local devices)**.
4. **Log blocked connections (Firewall Log)** - every block in [**Diagnostics > Firewall Log**](/manual/diagnostics-firewall-log/) as well.
5. Set **Automatic updates**, **Save**, **Apply changes**.

The lists are loaded into the packet filter as tables: later updates take effect without a restart or an Apply.

### Your own entries

| Field | Meaning |
| :--- | :--- |
| **Allow list** | Addresses and networks that are never blocked. Put the addresses you connect from here - your offices, your VPN users' locations, your monitoring service: if a list ever contains them, they stay reachable. |
| **Block list** | Addresses and networks that are always blocked. |
| **Custom lists** | `name \| https://address`, one per line. |

**Never blocked, whatever a list contains:** private and reserved ranges (`10.x`, `172.16-31.x`, `192.168.x`,
`127.x`, multicast ...) and networks larger than a /8. A list with a mistake in it cannot cut off your own
networks or half of the internet.

### Check an address

**Check an address** tells you whether an address is blocked and by which list.

## Automatic updates

Both tabs have the same update settings:

| Setting | Meaning |
| :--- | :--- |
| **Automatic updates** | Every hour, every 6 hours, every 12 hours, daily, weekly, or **Off (manual updates only)**. |
| **On**, **At** | The day (weekly) and the time of the update. |

The tab shows the state of the last update, the next automatic one, and the **Update log**. **Update lists now**
updates at once.

**A failed download keeps the previous list**, so protection never drops out because a source was unreachable.
Failed downloads are reported in the notifications.

The update schedule takes effect as soon as it is saved; switching the lists on or off and choosing lists needs
**Apply changes**.

## The Blocked report

**Firewall > Blocklists > Blocked** (administrators only) shows what the DNS and IP blocklists stopped.

![Firewall > Blocklists > Blocked](/images/manual/bl-blocked.png)

- **Period:** last hour, last 24 hours or last 7 days; **Refresh** updates it.
- **Totals:** DNS queries blocked, inbound and outbound packets blocked, blocks in the last hour.
- **Blocks per hour** - a graph of DNS and IP blocks.
- **By list** - which lists did the work. A list that never blocks anything may not be worth its memory.
- **Top blocked domains**, **Top blocked addresses**, **Devices with the most blocks**. A device with many
  outgoing IP blocks or blocked malware domains deserves a look: it may be infected.
- **Blocked queries and packets** - the individual entries, filtered by type (DNS, IP) and list, with time,
  domain or address, device and list. **Load more** shows older entries.

Entries are kept for 7 days. Because the report shows which device looked up which name, it is visible to
administrators only, and every view is logged. **Clear report** deletes the collected entries.

