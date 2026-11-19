---
title: "Diagnostics > Firewall Log"
seo_title: "10.1 Diagnostics > Firewall Log — Stratum FW User Manual"
description: "The live firewall log, the summary of blocked and passed traffic with top sources, ports, countries and rules, and the Between networks report for learning what traffic your internal networks need."
weight: 1001
chapter_num: "10.1"
url: "/manual/diagnostics-firewall-log/"
aliases:
  - "/manual/10-01-diagnostics-firewall-log/"
translationKey: "manual-10-01-diagnostics-firewall-log"
---

The firewall log shows the packets that pf logged: everything **blocked** by a rule or by the default rule, and
everything **passed** by rules that have **Log** switched on. It has three tabs: **Live**, **Summary** and
**Between networks**.

## Live

![Diagnostics > Firewall Log > Live](/images/manual/fwlog-live.png)

The newest entries first, up to 300, refreshed every 3 seconds. Each line shows **Time**, **Action** (block or
pass), **Interface**, **Protocol**, **Source**, **Destination** and the **Rule** that matched - click the rule to
open it.

Filters:

- **All actions**, **Blocked** or **Passed**;
- **All interfaces** or one;
- **Search** for an address, port, protocol or rule - for example `192.168.1.50`, `:443` or `udp`;
- **Pause** stops the refresh so you can read; **Resume** continues.

**Log broadcast and multicast packets** - broadcasts of the local networks (discovery protocols and the like) are
normally blocked without a log line; switch this on to see them. The same setting is on [[**System > Settings > Firewall & NAT**](/manual/system-settings-firewall-networking-tunables/)](/manual/system-settings-firewall-networking-tunables/).

### What is logged

| Packets | Logged when |
| :--- | :--- |
| Blocked by the default rule (nothing allowed them) | **Log packets blocked by the default rule** is on (default), on the WAN only with **Also on the WAN** (default on) - [**System > Settings > Firewall & NAT**](/manual/system-settings-firewall-networking-tunables/) |
| Blocked or passed by your rules | The rule has **Log** switched on ([**Firewall > Rules**](/manual/firewall-rules/)) |
| Blocked by the IP blocklists | Logging is switched on for the IP blocklists ([**Firewall > Blocklists**](/manual/firewall-blocklists/)); the rule shows as *IP blocklist: inbound* or *outbound* |

The log on the firewall is rotated at 5 MB with 3 old files by default ([**System > Settings > Logging**](/manual/system-settings-logging-notifications/)). For
long-term records, send the firewall log to a syslog server.

> **If the page says the log collector is not running**, apply the pending changes (it is started
> automatically), or run `service stratum_fwlog start` on the console.

### Reading the log

- **Many blocks on the WAN from addresses all over the world, to random ports** - internet-wide scans. Normal
  and harmless: the firewall drops them. Switch off **Also on the WAN** for a quieter log.
- **A device in your network is blocked going out** - a rule is missing, or the block is intended. The Rule column
  says which rule blocked it.
- **Something does not work and nothing appears in the log** - the traffic is passed (passes are only logged with
  **Log** on the rule), or it never reaches the firewall.

## Summary

![Diagnostics > Firewall Log > Summary](/images/manual/fwlog-summary.png)

Totals of the **Last hour** or the **Last 24 hours**, for **Blocked**, **Passed (logged rules)** or both, on all
interfaces or one:

- **Blocked packets**, **Different sources**, **All blocked**, **All passed (logged)**;
- a **Per hour** chart of blocked and passed packets;
- **Top sources**, **Top destination ports**, **Top countries**, **Top rules**, **Top destinations** and **By
  interface**.

Click an address, port or rule to see its entries on the **Live** tab.

The AI Assistant (**Diagnostics > AI Assistant**) can explain the summary - *"what was blocked in the last hour
and should I worry?"*

## Between networks

![Diagnostics > Firewall Log > Between networks](/images/manual/fwlog-networks.png)

Traffic **between your internal networks** that the firewall logged - for the **Last 24 hours**, **7 days** or
**30 days**, from any network or one, passed, blocked or both:

- **Network to network**: **From**, **To**, **Passed**, **Blocked**, **Devices** and **Last seen**;
- **Destinations and ports**: which destination and port each network used, with the number of **Connections**
  and **Devices**, **First seen** and **Last seen**.

The entries are kept for 30 days after they were last seen, independent of the firewall log's rotation, and
updated every minute.

### Closing off a network without breaking it

This report is built to answer *"what does this network actually need?"* before you isolate it - for example an
IoT or camera network that today may reach everything:

1. On [**Firewall > Rules**](/manual/firewall-rules/) of that network, add a **pass** rule with **Log** on, **from** the network **to** the
   other internal networks (an alias or interface group), **above** its *allow all* rule.
2. Let it collect for a week or two.
3. Look at **Between networks > Destinations and ports**: these are the connections the network really uses - for
   example the cameras to the recorder on port 554.
4. Add pass rules for exactly these, then a **block** rule to the other internal networks, and remove the logging
   rule.
5. The blocked tries now appear here too - check that nothing needed is among them.
