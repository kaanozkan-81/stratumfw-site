---
title: "Interfaces > Virtual IPs"
seo_title: "05.5 Interfaces > Virtual IPs — Stratum FW User Manual"
description: "Extra addresses on an interface: more public addresses on the WAN for port forwards and outbound NAT, or a second address in an internal network."
weight: 505
chapter_num: "05.5"
url: "/manual/interfaces-virtual-ips/"
aliases:
  - "/manual/05-05-interfaces-virtual-ips/"
translationKey: "manual-05-05-interfaces-virtual-ips"
---

A virtual IP is an extra address on an interface (an *IP alias*). The firewall answers on it just as on the
interface's own address.

Typical uses:

- **A block of public addresses.** The provider gave you a /29 (five usable addresses). The WAN uses one; the
  others become virtual IPs, so that a second address can forward to a different server, or a whole internal
  network can leave through an address of its own.
- **A second address in an internal network,** for example while devices are moved from an old gateway address
  to a new one.

## The list

**Interfaces > Virtual IPs** shows each virtual IP with its **Address**, **Interface**, **Description**, the
NAT entries that use it (**Used by**) and its **State** (active, or pending until the change is applied).

![Interfaces > Virtual IPs](/images/manual/vip-list.png)

## Add a virtual IP

Click **Add virtual IP**.

| Setting | Meaning |
| :--- | :--- |
| **Interface** | The interface that gets the address. It must have an IPv4 address of its own (static or DHCP). |
| **Address** | The extra address, for example `203.0.113.11`. It cannot be the network or broadcast address of the interface's network, nor an address an interface already uses, and each address can be listed once. |
| **Prefix** | Only for an address **outside** the interface's own network: the prefix of that other network (8 to 32). An address inside the interface's network is always added as a single address (/32), as FreeBSD requires - whatever you enter. |
| **Description** | Your note, for example *Mail server address*. |

Click **Save**, **Apply changes** and **Keep changes**. A restart of the interface (or of the whole network
configuration) keeps the virtual IPs: the firewall adds them again on its own, also after a reboot.

## Using a virtual IP

| Where | How |
| :--- | :--- |
| **Port forwards** (**Firewall > NAT**) | Choose the virtual IP as the forward's destination address: connections to *that* address and port go to the internal server. Without it, a forward listens on the interface's own address. |
| **Outbound NAT** (**Firewall > NAT**, manual or hybrid mode) | Translate the traffic of an internal network or host to the virtual IP, so it leaves through its own public address - for example a mail server whose address must match its DNS records. |
| **1:1 NAT** (**Firewall > NAT**, 1:1) | Map a public address to one internal host in both directions. A single public address used for 1:1 NAT must be a virtual IP on that WAN. |
| **Firewall rules** | The firewall's own addresses (*This firewall* in rules) include its virtual IPs. WAN rules and the management access settings decide what is allowed to reach them. |

> **Note:** A virtual IP on the WAN is a public address of the firewall. Like the WAN address, nothing reaches it
> unless a port forward or a WAN rule allows it.

## Change or delete

- A virtual IP that a port forward or an outbound NAT rule uses cannot get another address and cannot be
  deleted: the panel names the entries - *This virtual IP is used by NAT - change it there first: Port forward
  Mail*. Change those entries first.
- The edit page lists what uses the address under **Used by**.

There can be up to 256 virtual IPs.

