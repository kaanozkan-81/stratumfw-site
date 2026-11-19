---
title: "Firewall > Schedules"
seo_title: "06.4 Firewall > Schedules — Stratum FW User Manual"
description: "Time-based firewall rules: defining days and time ranges, using a schedule on pass and block rules, ranges over midnight and what happens when a schedule ends."
weight: 604
chapter_num: "06.4"
url: "/manual/firewall-schedules/"
aliases:
  - "/manual/06-04-firewall-schedules/"
translationKey: "manual-06-04-firewall-schedules"
---

A schedule limits a firewall rule to certain days and times. A rule with a schedule exists only during the
schedule's times; outside them it is as if the rule were not there.

Typical uses:

- the guest Wi-Fi only from 07:00 to 23:00;
- social media blocked during working hours;
- a supplier's remote access allowed on weekdays only.

## The list

**Firewall > Schedules** shows each schedule with its **Name**, **Times**, whether it is active **Now**, the
rules that use it (**Used by**) and its **Description**.

![Firewall > Schedules](/images/manual/fw-schedules.png)

## Add a schedule

Click **Add schedule**.

| Field | Meaning |
| :--- | :--- |
| **Name** | Letters, digits, `-` and `_`, starting with a letter, up to 31 characters - for example `Work_Hours`. Renaming keeps the rules that use it. |
| **Description** | Your note. |
| **Time ranges** | One or more ranges, each with its **days** and a **start** and **end** time (`HH:MM`). **Add time range** adds another. |

The rule exists when the time is inside **any** of the ranges.

- **End 24:00** means until midnight.
- **A range over midnight is two ranges** - for example *22:00-24:00* on Friday and *00:00-06:00* on Saturday.
- Different times on different days are separate ranges: *Mon-Fri 08:00-18:00* and *Sat 09:00-13:00*.

Click **Save**. The edit page lists the rules that use the schedule under **Used by**.

## Use a schedule on a rule

Edit the rule ([**Firewall > Rules**](/manual/firewall-rules/)) and choose the schedule under **Schedule**. In the rule list, the schedule
appears as a badge next to the description - green while it is active, grey while the rule is off.

How it works depends on the rule's action:

| Rule | During the schedule | Outside the schedule |
| :--- | :--- | :--- |
| **Pass** | The traffic is allowed. | The rule does not exist: the rules below it - or the default block - decide. Connections that the rule allowed are **closed when the schedule ends**. |
| **Block / Reject** | The traffic is blocked. | The rule does not exist: the traffic is not blocked by it. |

Example - social media blocked during working hours on the LAN:

| # | Action | Source | Destination | Schedule |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Reject | LAN net | `Social_Media` (host names alias) | `Work_Hours` |
| 2 | Pass | LAN net | any | Always |

## Timing

- Rules change within a minute of a range's start or end.
- Times are in the firewall's time zone (**System > Settings > General**). A wrong time zone or clock moves every
  schedule - check them first when a schedule seems early or late.
- Changes to schedules and to which rules use them take effect after **Apply changes**.

## Delete a schedule

A schedule that rules still use cannot be deleted: the panel says how many rules use it and which. Choose
**Always** (or another schedule) on those rules first.

