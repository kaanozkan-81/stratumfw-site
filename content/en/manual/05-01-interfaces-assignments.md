---
title: "Interfaces > Assignments"
seo_title: "05.1 Interfaces > Assignments — Stratum FW User Manual"
description: "Network ports and their roles: moving WAN and LAN to another port, adding OPT interfaces as internal networks or extra internet connections, addresses, PPPoE, MTU and removing interfaces."
weight: 401
chapter_num: "05.1"
url: "/manual/interfaces-assignments/"
aliases:
  - "/manual/05-01-interfaces-assignments/"
translationKey: "manual-05-01-interfaces-assignments"
---

An *interface* is a network the firewall is part of. It has a **role** - *WAN*, *LAN*, *OPT1*, *OPT2* ... - and
it uses a **network port**: a physical port such as `igb2`, or a VLAN on a port such as `igb1.20`. Everything
else in the panel - firewall rules, DHCP, DNS, VPN access, NAT - is attached to the role, not to the port. That
is why a network can be moved to another port without touching its rules.

**Interfaces > Assignments** lists the interfaces and the ports that are still free.

![Interfaces > Assignments](/images/manual/if-assignments.png)

## The list

**Assigned interfaces** shows one line per interface:

| Column | Meaning |
| :--- | :--- |
| **Interface** | The description you gave (large) and the role (small) - for example *GUESTS* / *OPT2*. OPT interfaces also show whether they are a **WAN** or a **LAN**. **you are here** marks the interface your browser reaches the panel through. A greyed-out line is a disabled interface. |
| **Network port** | The port the interface uses; it can be changed here (see below). |
| **Link** | Whether a cable is connected and the link is up. |
| **IPv4** | The address (and how it was obtained). |
| **MAC address** | The port's hardware address. |
| **Actions** | **Edit**; **Remove** for OPT interfaces. |

**Available network ports** lists the physical ports and VLANs that no interface uses yet, with their MAC
address, link and media (speed). The list refreshes itself every 10 seconds, so you can plug a cable in and
watch which port comes up.

## WAN, LAN and OPT

| Role | Type | What it is |
| :--- | :--- | :--- |
| **WAN** | WAN | The main internet connection. It carries the default route and cannot be disabled or removed. |
| **LAN** | LAN | The main internal network. The *anti-lockout* rule always lets it reach the panel. It cannot be removed, only moved to another port. |
| **OPT1**, **OPT2** ... | LAN or WAN | Every further network. You choose the type when you add it. |

The type decides how the firewall treats the network:

- **LAN** (internal network): it has its own firewall rules, can run a DHCP server, is reached by the DNS
  resolver, and its traffic to the internet is translated (outbound NAT). A new LAN-type OPT starts **without
  firewall rules** - nothing from it passes until you add rules on **Firewall > Rules**.
- **WAN** (additional internet connection): it gets outbound NAT and the same protection as the WAN - incoming
  traffic is blocked unless a rule allows it. The default route stays on the main WAN; traffic is sent through
  an additional WAN by failover or by policy routing ([**Interfaces > Gateways**](/manual/interfaces-gateways/), **Firewall > Rules**).

## Add an interface

1. Under **Available network ports**, click **Add interface** on the port. A VLAN appears here after you create
   it on [**Interfaces > VLANs**](/manual/interfaces-vlans/).
2. Choose the **Interface type**: **WAN** (an internet connection) or **LAN** (an internal network with its own
   clients and DHCP server). The type cannot be changed later - remove the interface and add it again instead.
3. Fill in the settings (next section) and click **Add interface**.
4. **Apply changes** and **Keep changes**.

The new interface takes the next free OPT number.

For a LAN-type interface, then:

- add firewall rules for it (**Firewall > Rules**, its tab) - until then everything from it is blocked;
- switch the DHCP server on if its devices need addresses (**Services > DHCP Server**).

## Interface settings

Click **Edit** on an interface. If you reach the panel through it, a notice reminds you that a change which
breaks the connection is reverted automatically 120 seconds after **Apply changes**.

![Editing an interface](/images/manual/if-edit.png)

| Setting | Meaning |
| :--- | :--- |
| **Description** | The name shown everywhere in the panel, up to 40 characters, for example *Guests* or *Fibre*. Empty: the role (*OPT2*). |
| **Status** | **Interface is enabled**. A disabled interface keeps its settings but carries no traffic; its rules and DHCP server are not used. The WAN is always enabled. |
| **IPv4 configuration** | How the interface gets its address - see below. |
| **MTU** | The largest packet size on this network. Empty means the default: 1500, or 1492 for PPPoE. Allowed: 576 to 9216. Change it only for a reason - a provider's instruction, or jumbo frames on a storage network whose switches support them. |

**IPv4 configuration:**

| Type | Settings | Notes |
| :--- | :--- | :--- |
| **Static** | **IPv4 address / prefix** - the firewall's own address in this network, for example `192.168.20.1/24` (the prefix must be /30 or shorter); for a WAN also the **Gateway**, the provider's router, inside that network. | The usual choice for internal networks. |
| **DHCP** | None. | The address comes from another DHCP server. On an additional WAN, the gateway of its lease is used for failover, replies and policy routing; it never takes over the default route or the DNS servers by itself. |
| **PPPoE** | **Username**, **Password**, **Service name** (only when the provider asks). | WAN type only. The connection appears as a separate `pppoe` interface; rules and NAT follow it. The password is kept in a root-only file and never shown again - leave the field empty to keep it. |
| **None** | None. | No address: for a port that only carries VLANs, or one you prepare for later. |

### When the network of an interface changes

Changing the address of an internal interface to another network carries its DHCP settings along:

- If the DHCP range no longer fits, it is moved to the new network (to `.100 - .199` in a /24) and the panel says
  so.
- Static DHCP mappings keep their host part (`192.168.1.50` becomes `192.168.20.50`). If a mapping cannot be
  moved, the change is refused - edit or remove the mapping on **Services > DHCP Server** first.
- If the interface no longer has a static address, its DHCP server is switched off.

Firewall rules that use *the network of OPT2* follow the new network automatically. Rules or aliases with the
old addresses written out by hand have to be changed by hand.

## Move an interface to another port

Each line of **Assigned interfaces** has a **Network port** selector. Choose another port and click **Save
assignments**. The whole configuration of the interface - address, firewall rules, DHCP server - moves with it.
Each port can be used by one interface only.

Typical uses: a port failed, the appliance was replaced by a model with other port names, or a network moves
from a physical port to a VLAN.

> **Warning:** Moving the interface you are connected through cuts your connection until the cable is in the new
> port. The change is reverted automatically if you cannot click **Keep changes** within 120 seconds - so either
> move the cable quickly, or make the change from another interface or from the console (menu option
> **1) Assign interfaces**).

## Remove an interface

OPT interfaces can be removed with **Remove** on their line (click again on the same line to confirm). The
interface's own firewall rules and DHCP server go with it. The port becomes free again.

The panel refuses to remove an interface that something else still uses, and lists what: for example
*OPT2 is still used by: LAN rule #3, Port forward #1*. Change or delete those first. WAN and LAN cannot be
removed; they can only be moved to another port.

