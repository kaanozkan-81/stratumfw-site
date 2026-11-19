---
title: "Services > DHCP Server"
seo_title: "07.2 Services > DHCP Server — Stratum FW User Manual"
description: "Handing out addresses on internal networks: range, lease time, gateway and DNS for clients, static mappings, and the active leases."
weight: 702
chapter_num: "07.2"
url: "/manual/services-dhcp-server/"
aliases:
  - "/manual/07-02-services-dhcp-server/"
translationKey: "manual-07-02-services-dhcp-server"
---

The DHCP server hands out addresses to the devices of an internal network, with the gateway and DNS servers they
should use. Each LAN-type interface with a static address has its own DHCP server, on its own tab of
**Services > DHCP Server**.

![Services > DHCP Server](/images/manual/dhcp.png)

## Settings

| Setting | Meaning |
| :--- | :--- |
| **Enable the DHCP server on this interface** | Switches the server on for this network. |
| **Range start**, **Range end** | The addresses handed out to devices without a static mapping. They must be inside the network. The range may contain the interface's own address and the gateway: they are never handed out. |
| **Lease time** | How long a device keeps its address before it must renew it: 1 hour, 4 hours, 12 hours (default), 1 day or 7 days. Short leases free addresses quickly on busy guest networks; long leases suit stable office networks. |
| **Gateway for clients** | Empty: the firewall's address in this network - the usual choice. Set another address only when the devices must use another router. |
| **DNS servers for clients** | Empty: the firewall, which also resolves the names of static mappings and applies the [DNS blocklists](/manual/firewall-blocklists/) and the [Web Filter](/manual/firewall-web-filter/). Up to three addresses, separated by commas, to give other servers instead. |

Click **Save settings**, then **Apply changes** and **Keep changes**.

> **Note:** Leave **DNS servers for clients** empty unless you have a reason. Devices that get other DNS servers
> bypass the DNS blocklists and the Web Filter, and their names are not shown in Web Activity. The captive
> portal refuses a zone whose DHCP server gives other DNS servers.

**Size the range for the network.** A guest network of a hotel with hundreds of phones needs a large network
(for example a /22 with about 1000 addresses) and a short lease time; a /24 with a 12-hour lease fills up
quickly when many guests pass through. [**Interfaces > Assignments**](/manual/interfaces-assignments/) sets the network size; when it changes, the
range is moved to fit.

## Static mappings

A static mapping makes the DHCP server always give the same address to one device, recognised by its MAC
address. Use it for printers, servers, cameras, access points and anything other devices or firewall rules refer
to.

**Add mapping**, or - faster - **Make static** on a device in **Active leases**: the form opens with its current
address and name filled in.

| Field | Meaning |
| :--- | :--- |
| **MAC address** | The device's hardware address. Any common notation works (`aa:bb:cc:dd:ee:ff`, `aa-bb-cc-dd-ee-ff`, `aabb.ccdd.eeff`). Each MAC can have one mapping. |
| **IP address** | The address to give it, inside the interface's network. It may be inside or outside the dynamic range. Each address can belong to one mapping. |
| **Hostname** | Optional; letters, digits and hyphens. The firewall's DNS resolves this name in the local domain - `printer-2f.lan`. |
| **Description** | Your note, for example *2nd floor printer*. |

Click **Save** and **Apply changes**. The device gets the new address when it renews its lease or reconnects -
unplug it or switch its network off and on to make it happen at once.

> **Tip:** Phones and laptops often use a *random* (private) MAC address per network, which can change. A
> static mapping for such a device works only while its MAC stays the same; switch the random address off for
> that network on the device.

## Active leases

**Active leases** lists the devices that have an address now: **IP address**, **MAC address**, **Hostname** (as
the device reported it), when the lease **Expires** and its **Type** (dynamic or static). The list refreshes
itself every 15 seconds.

The [dashboard's](/manual/dashboard/) **DHCP** panel shows the devices and how full each network's range is.
