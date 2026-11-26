---
title: "Firewall > Rules"
seo_title: "06.1 Firewall > Rules — Stratum FW User Manual"
description: "Firewall rules per interface and group: how they are evaluated, the system rules, every field of a rule, separators, hit counters, schedules, policy routing and good practice."
weight: 601
chapter_num: "06.1"
url: "/manual/firewall-rules/"
aliases:
  - "/manual/06-01-firewall-rules/"
translationKey: "manual-06-01-firewall-rules"
---

Firewall rules decide which traffic may pass. Each interface - and each interface group - has its own list of
rules on **Firewall > Rules**, one tab per interface or group.

![Firewall > Rules](/images/manual/fw-rules.png)

## How rules are evaluated

- **Rules apply to traffic arriving on an interface.** A rule on the *LAN* tab looks at packets that come in
  from the LAN - devices in the LAN opening connections. To control what the guests may reach, write rules on
  the guests' tab, not on the tab of the network they want to reach.
- **From top to bottom, first match wins.** The first rule whose conditions match decides; the rules below it
  are not checked.
- **Answers are allowed automatically.** The firewall keeps track of every connection it lets through (a
  *state*); the replies of an allowed connection pass without a rule of their own.
- **Everything else is blocked.** At the end of every list stands the *Default deny*: traffic no rule allows is
  blocked and logged.
- **Order of the lists:** system rules first, then the rules of the groups the interface belongs to, then the
  interface's own rules, then the default deny.

```
Packet arrives on GUEST (OPT2, member of group Vlans)
  1. system rules (grey)            anti-lockout, DHCP, port forwards ...
  2. rules of group Vlans           <- your shared rules
  3. rules of OPT2 (GUEST)          <- your rules for this network
  4. default deny                   blocked and logged
```

## The rule list

| Column | Meaning |
| :--- | :--- |
| (handle) | Drag a rule by its handle to move it. |
| **On** | The switch enables or disables the rule. |
| **Action** | Pass, block or reject; an icon shows that matching packets are logged. |
| **Protocol**, **Source**, **Destination**, **Port** | What the rule matches. `!` before an address means *everything except*. |
| **Description** | Your text, with badges for a schedule (green while the schedule is active) and for a gateway (*via WAN2*). |
| **Hits** | Packets and bytes that matched the rule since the rules were last loaded (every Apply loads them again). Hover for the number of open connections (states). |
| **Actions** | Move up / down, edit, delete. |

Every change on this page - adding, editing, moving, enabling or disabling a rule - is saved at once and becomes
active with **Apply changes**.

### System rules (grey)

Above your rules, grey lines show the rules Stratum FW writes from other settings. They cannot be edited here;
the description says where they come from. Depending on the interface:

| System rule | Comes from |
| :--- | :--- |
| Anti-lockout: web panel and SSH from LAN | **System > Settings > Admin Access** (LAN only) |
| Management access / No management access from this interface | **System > Settings > Admin Access** |
| Management access from WAN | **System > Settings > Admin Access** |
| DHCP server, NTP server | **Services > DHCP Server**, **Services > NTP** |
| SNMP agent | **System > Settings > SNMP** |
| Block broadcast and multicast | Noise that is never forwarded (logged or not: **System > Settings > Firewall**) |
| Block bogon (reserved) networks, Block private networks | **System > Settings > Firewall** (WAN-type interfaces) |
| Ping (echo request) | **System > Settings > Firewall** (WAN-type interfaces) |
| Port forward: ... | [**Firewall > NAT**](/manual/firewall-nat/) - every port forward gets its pass rule automatically |
| VPN and captive portal rules | The VPN pages and [**Services > Captive Portal**](/manual/services-captive-portal/) |

## Add or edit a rule

Click **Add rule** (or a rule's edit icon).

![Editing a rule](/images/manual/fw-rule-edit.png)

| Field | Meaning |
| :--- | :--- |
| **Action** | **Pass** - allow the traffic. **Block** - drop it silently. **Reject** - drop it and tell the sender (a TCP reset or an ICMP message), so connections fail at once instead of timing out. |
| **Protocol** | **Any**, **TCP**, **UDP**, **TCP/UDP** or **ICMP** (all ICMP, including ping). A destination port needs TCP, UDP or TCP/UDP. |
| **Interface** | The tab the rule belongs to. |
| **Source**, **Destination** | **Any**; **Single host or network** (`192.168.1.10`, `10.0.0.0/8`); **Alias** - a host or network alias ([**Firewall > Aliases**](/manual/firewall-aliases/)); **Interface network** - the network of an interface, for example *LAN net* (all LAN clients; it follows the interface when its address changes); **This firewall** - every address of the firewall itself, virtual IPs included; **Internet (not private networks)** *(Destination)* - anything except `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` and `100.64.0.0/10` (*WAN net* is only the WAN link's own subnet, not the internet). **Invert match** means everything except what is chosen. |
| **Destination port** | **Any**, **Port or range** (`443`, `8000:8100`) or **Port alias**. Source ports are rarely useful and are not offered. |
| **Schedule** | **Always**, or a schedule from [**Firewall > Schedules**](/manual/firewall-schedules/): the rule exists only during the schedule's times; outside them the rules below decide. |
| **Gateway** | (Pass rules on internal interfaces, with more than one WAN.) Send the traffic through a chosen WAN - see [**Interfaces > Gateways**](/manual/interfaces-gateways/). |
| **Description** | Up to 200 characters. Write *why* the rule exists - in a year nobody will remember. |
| **Log packets that match this rule** | Matching packets appear in [**Diagnostics > Firewall Log**](/manual/diagnostics-firewall-log/) and in the firewall log reports. Log what you need, not everything: a logged *pass* rule for busy traffic fills the log quickly. |
| **Rule is enabled** | A disabled rule stays in the list but does nothing. |
| **Position** | For a new rule: **At the bottom of the list** or **At the top of the list**. |

Click **Save**, then **Apply changes** and **Keep changes**.

### Block or reject?

- On **internal networks** use **Reject**: devices and users get an immediate error instead of waiting for a
  timeout.
- On the **WAN** use **Block**: a scanner gets no answer and learns nothing about the firewall.

## Starting closed and "Allow internet access"

On a new installation (or after a factory reset), **every interface starts closed**: like zone-based firewalls, neither LAN nor any VLAN can reach the internet or another internal network until a rule allows it (the web panel stays reachable from LAN via the anti-lockout rule and DHCP keeps working; existing installations keep their rules).

![Firewall > Rules — closed interface note and Allow internet access](/images/manual/fw-rules-allow-internet.png)

When you open the tab of an interface that has no custom rules yet, Stratum FW shows a note and an **Allow internet access** button that adds two rules in one click:

1. **This firewall** — allows DNS, NTP and ping to the firewall itself.
2. **Internet (not private networks)** — allows traffic to the public internet while keeping `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` and `100.64.0.0/10` (all other internal VLANs) unreachable.

## Separators

**Add separator** inserts a coloured, named line into the list - for example *Servers*, *Printers*, *Temporary*.
Separators move like rules (arrows or drag) and can be renamed, recoloured or deleted. They only group the list;
they do not change what the rules do.

## Good practice

- **Specific above general.** An exception (*allow the printer to reach the print server*) must stand above the
  general rule it is an exception to (*block the guests from the LAN*).
- **Block internal networks before allowing the internet.** A typical guest network:

  | # | Action | Source | Destination | Port | Description |
  | :--- | :--- | :--- | :--- | :--- | :--- |
  | 1 | Pass | GUEST net | This firewall | 53 (TCP/UDP) | DNS from the firewall |
  | 2 | Reject | GUEST net | RFC1918 (alias: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`) | any | No access to internal networks |
  | 3 | Pass | GUEST net | any | any | Internet |

  Rule 2 keeps the guests away from every internal network - also from networks added later - and rule 3 lets
  everything else out to the internet.
- **Keep "This firewall" in mind.** A guest rule that passes *any* also passes the firewall's own addresses -
  including the panel, unless **Admin Access** limits which networks may reach it.
- **Use aliases** for addresses and ports that appear in several rules: change the alias once instead of every
  rule.
- **Use groups** for rules that many interfaces share ([**Interfaces > Groups**](/manual/interfaces-groups/)).
- **Check with the hit counters and the log.** A rule that never gets a hit may be in the wrong place or have
  the wrong source; [**Diagnostics > Firewall Log**](/manual/diagnostics-firewall-log/) shows which rule blocked a packet.

