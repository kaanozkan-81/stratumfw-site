---
title: "Diagnostics > Ping / Traceroute / DNS, States and ARP Table"
seo_title: "10.3 Diagnostics > Ping / Traceroute / DNS, States and ARP Table — Stratum FW User Manual"
description: "Testing reachability, paths and name resolution from the firewall, viewing and killing connections in the state table, the ARP table and waking devices with Wake-on-LAN."
weight: 1003
chapter_num: "10.3"
url: "/manual/diagnostics-tools-states-arp/"
aliases:
  - "/manual/10-03-diagnostics-tools-states-arp/"
translationKey: "manual-10-03-diagnostics-tools-states-arp"
---

## Ping / Traceroute / DNS

![Diagnostics > Ping](/images/manual/diag-tools.png)

These tools run **on the firewall** - they show what the firewall itself can reach, which is often the quickest
way to tell whether a problem is on the firewall, on the internet side or on a device.

Three tabs:

| Tool | Fields |
| :--- | :--- |
| **Ping** | **Target** (address or name), **Source interface**, **Count** (1-10, default 4). |
| **Traceroute** | **Target**, **Source interface**, **Maximum hops** (1-30, default 20). |
| **DNS lookup** | **Target** (a name, for example `freebsd.org` or `nas.lan`), **Record type** (A, AAAA, MX, TXT, NS, CNAME, SOA, PTR), **DNS server** (empty: this firewall). |

**Source interface** - **Automatic (routing table)**, or an interface whose address is used as source. Use it to
test a particular WAN, or to test **through a VPN tunnel**: a ping with a LAN as source to an address at the other
site goes through the site-to-site tunnel, as the devices' traffic does.

**Run** shows the output as it comes; **Stop** ends it. Operators can use these tools too.

Typical checks:

| Question | Test |
| :--- | :--- |
| Does the firewall reach the internet? | Ping `1.1.1.1`. |
| Do names resolve? | DNS lookup of `freebsd.org`. Works, but ping to `1.1.1.1` does not? The internet is down. Ping works, but the lookup does not? DNS is the problem. |
| Does WAN 2 work on its own? | Ping `1.1.1.1` with WAN 2 as source interface. |
| Does the tunnel to the branch work? | Ping a device at the branch with the LAN as source. |
| Where does the path break? | Traceroute to the target. |

## States

![Diagnostics > States](/images/manual/diag-states.png)

The **state table**: every connection the firewall tracks now. Each line shows the **Interface** with the
direction, **Protocol**, **Source**, **Destination** (with the address after NAT), **State**, **Age**, **Packets**,
**Bytes** and the **Rule** that allowed it.

**Search** for an address, port, protocol, interface or rule. The page shows the total number of states and the
first 500 matches, refreshed every 10 seconds.

**Kill** ends one connection at once (after a confirmation) - for example a connection that must stop now after
you changed a rule: changed rules apply to new connections, existing ones keep running until they end.

The number of states in use and the maximum are also shown on [**System > Settings > Firewall & NAT**](/manual/system-settings-firewall-networking-tunables/) and on the [dashboard](/manual/dashboard/).

## ARP Table

![Diagnostics > ARP Table](/images/manual/diag-arp.png)

The devices the firewall has seen recently in its networks: **IP address**, **MAC address**, **Name** (from DHCP),
**Interface** and when the entry **Expires**. **Search** for an address, MAC, name or interface.

**Delete** removes an entry; the device is looked up again on its next packet - useful after a device was replaced
and took over an address.

ARP changes (an address moving to another MAC address) are logged - see [**System > Settings > Networking**](/manual/system-settings-firewall-networking-tunables/).

### Wake-on-LAN

Switch on a computer over the network:

- **Wake** next to a device in the ARP table or in **Devices with a DHCP reservation**, or
- **Wake a device**: choose the **Interface**, enter the **MAC address** and click **Send wake-up**.

The device must have Wake-on-LAN switched on (BIOS / network card) and be connected by cable.
