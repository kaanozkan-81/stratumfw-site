---
title: "Interfaces > VLANs"
seo_title: "05.2 Interfaces > VLANs — Stratum FW User Manual"
description: "802.1Q VLANs: several separate networks on one cable - creating a VLAN, assigning it as an interface, the switch side, and changing or deleting VLANs."
weight: 502
chapter_num: "05.2"
url: "/manual/interfaces-vlans/"
aliases:
  - "/manual/05-02-interfaces-vlans/"
translationKey: "manual-05-02-interfaces-vlans"
---

A VLAN (IEEE 802.1Q) carries a separate network over a shared cable. Each network is identified by a number,
its *tag*, from 1 to 4094. With VLANs, one physical port of the firewall can serve the staff network, the guest
Wi-Fi, the cameras and the phones, each with its own address range, DHCP server and firewall rules - as if each
had a port of its own.

Typical uses:

- Guest, staff and device networks on one LAN port connected to a managed switch.
- Two provider connections arriving on the same cable with different tags.
- An internet provider that requires a VLAN tag on the WAN (common with fibre: the provider tells you the tag).

## How it fits together

```
                       trunk: tags 10, 20, 30
 [ Stratum FW igb1 ] =========================== [ managed switch ]
     igb1.10  LAN-Staff   10.10.10.1/24              port 1-8   untagged VLAN 10
     igb1.20  Guests      10.20.0.1/22               access points: tagged 20 (+ 10 for management)
     igb1.30  Cameras     10.30.0.1/24               port 20-24 untagged VLAN 30
```

Three steps make a VLAN usable:

1. **Create the VLAN** on **Interfaces > VLANs**: the parent port and the tag. This creates the VLAN port
   (for example `igb1.20`).
2. **Assign it as an interface** on [**Interfaces > Assignments**](/manual/interfaces-assignments/): it appears under **Available network ports**;
   add it as a LAN (or WAN) with its address. From now on it is a network like any other - with its own rules,
   DHCP server and so on.
3. **Configure the switch**: the switch port the firewall is plugged into must be a *trunk* (tagged port) that
   carries the same tags.

## The VLAN list

![Interfaces > VLANs](/images/manual/if-vlans.png)

| Column | Meaning |
| :--- | :--- |
| **VLAN interface** | The VLAN port: parent port and tag, for example `igb1.20`. |
| **Parent port** | The physical port that carries it. |
| **Tag** | The 802.1Q VLAN ID. |
| **Description** | Your note for the VLAN. |
| **State** | *up* (the parent port has a link), *down* / *no carrier*, or *pending* (created, not applied yet). |
| **Assigned as** | The interface that uses it (*OPT3*), or *not assigned*. |

## Create a VLAN

Click **Add VLAN**.

| Setting | Meaning |
| :--- | :--- |
| **Parent port** | The physical port connected to the switch trunk. A VLAN cannot be placed on another VLAN. |
| **VLAN tag** | 1 to 4094. Must match the VLAN ID configured on the switch. A tag can be used only once per parent port. |
| **Description** | Optional note, up to 40 characters. |
| **Interface name** | Shown for information: created from the parent port and the tag (`igb1` + `20` = `igb1.20`). |

Click **Save**, then **Apply changes** and **Keep changes**. The VLAN port now exists - but carries no traffic
until it is assigned as an interface: go to **Interfaces > Assignments**, find it under **Available network
ports** and click **Add interface**.

> **Tip:** A port can carry VLANs and an untagged network at the same time: the port itself (`igb1`) can be the
> LAN while `igb1.20` and `igb1.30` are OPT interfaces. If you want the port to carry only tagged traffic, assign
> nothing to it, or assign it with the IPv4 type **None**.

## The switch side

The firewall sends and expects VLAN traffic *tagged*. On the switch:

- The port connected to the firewall: **trunk** / **tagged** for every VLAN the firewall should serve. If the
  parent port is also used untagged (as in the tip above), that network is the port's untagged (native/PVID)
  VLAN.
- Ports of ordinary devices (PCs, cameras, printers): **access** / **untagged** in their VLAN.
- Wi-Fi access points with several SSIDs: tagged for every SSID's VLAN, untagged (or tagged) for their own
  management network - as the access point's manual says.

The names differ between vendors (Cisco *trunk*/*access*, HP/Aruba *tagged*/*untagged*, MikroTik bridge VLAN
filtering), but the idea is the same.

## Change or delete a VLAN

- The **Description** can always be changed.
- **Parent port** and **VLAN tag** can only be changed while the VLAN is not assigned. If it is assigned, the
  page says so: *This VLAN is assigned to OPT3. Its port and tag can only be changed after the interface is
  removed.*
- **Delete** works the same way: remove the interface on [**Interfaces > Assignments**](/manual/interfaces-assignments/) first, then delete the
  VLAN.

To move a network from one VLAN to another without losing its settings, create the new VLAN and use the
**Network port** selector on [**Interfaces > Assignments**](/manual/interfaces-assignments/) to move the interface onto it; then delete the old
VLAN.

## When a VLAN does not work

| Symptom | Check |
| :--- | :--- |
| *State* is *down* | The parent port has no link: cable, switch port, speed. |
| Devices in the VLAN get no address | The DHCP server is switched on for that interface (**Services > DHCP Server**); the switch port of the device is untagged in the right VLAN; the trunk to the firewall carries the tag. |
| Devices get an address but reach nothing | A LAN-type interface added after the setup wizard has no rules until you add them (**Firewall > Rules**, its tab). **Diagnostics > Firewall Log** shows what is blocked. |
| Traffic arrives in the wrong network | The tags on the switch and on the firewall differ, or the switch's native VLAN on the trunk is wrong. **Diagnostics > Packet Capture** on the parent port shows the tags that arrive. |

