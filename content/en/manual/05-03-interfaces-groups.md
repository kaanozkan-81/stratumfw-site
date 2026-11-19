---
title: "Interfaces > Groups"
seo_title: "05.3 Interfaces > Groups — Stratum FW User Manual"
description: "Interface groups: one set of firewall rules shared by several interfaces, evaluated before each member's own rules."
weight: 503
chapter_num: "05.3"
url: "/manual/interfaces-groups/"
aliases:
  - "/manual/05-03-interfaces-groups/"
translationKey: "manual-05-03-interfaces-groups"
---

An interface group gathers several interfaces under one name so that they can share firewall rules. A rule
written once on the group applies to every member interface.

Typical uses:

- **Many VLANs, one policy.** Twenty guest and department VLANs must all be kept away from the management
  network: one *block* rule on a group `Vlans` instead of twenty copies.
- **Several WANs.** A rule that must apply on every internet connection - for example allowing a VPN port - on
  a group of the WAN interfaces.

## How group rules are evaluated

A group gets its own tab on **Firewall > Rules**. Its rules are evaluated **before** the members' own rules,
in the order they are listed - the same as in pfSense and OPNsense. The first rule that matches decides.

```
Packet arrives on OPT3 (a member of group Vlans)
  1. rules of group Vlans      <- checked first
  2. rules of OPT3 itself      <- only if no group rule matched
  3. default: blocked
```

So put shared **block** rules on the group (they stop the traffic before any member's own "allow" rule), and
keep the rules that differ per network on the interfaces themselves.

> **Note:** A rule on a group written as *the network of LAN* or *the address of OPT2* still means exactly those
> networks - not "the network of each member". To match each member's own network, use a source of *any* on the
> group, or an alias containing all the member networks.

## The group list

**Interfaces > Groups** lists the groups with their **Members**, **Description** and the number of **Rules** they
have.

![Interfaces > Groups](/images/manual/if-groups.png)

## Create or change a group

Click **Add group** (or **Edit** on a group).

| Setting | Meaning |
| :--- | :--- |
| **Name** | 1 to 15 letters, digits or `_`, starting with a letter - for example `Vlans` or `Guest_Nets`. It is the name of the group's tab on **Firewall > Rules**. It cannot be the name of an interface (`lan`, `opt2` ...) or of an alias. |
| **Description** | Optional note, up to 80 characters. |
| **Members** | The interfaces in the group - at least one. An interface can be in several groups. |

Save, then add the group's rules on **Firewall > Rules**, the group's tab, and **Apply changes**.

When an interface is removed, it simply drops out of the groups it was in.

## Delete a group

A group that still has rules cannot be deleted: *The group has 2 rule(s). Delete them on Firewall > Rules >
Vlans first.* This is on purpose - deleting the group would silently drop those rules, and with them, for
example, the block that keeps your guests away from the management network.

## Limits

- At most 32 groups.
- A rule that sends traffic through a chosen WAN (policy routing) can be used on a group only if every member is
  an internal (LAN-type) interface.

