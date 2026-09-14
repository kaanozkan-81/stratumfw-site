---
title: "Firewall Rules"
description: "How to create, order, and apply firewall rules in Stratum Gateway — the anatomy of a rule, objects and aliases, first-match ordering, and worked examples for common scenarios."
weight: 5
---

Firewall rules decide what traffic is allowed to pass and what is dropped. Stratum Gateway generates nftables rulesets from a policy layer managed in the panel, so you never edit nftables directly. Understanding the rule model is the difference between a firewall that just works and one that locks you out of your own network.

This guide covers the rule engine from the ground up: what a rule is, how rules are evaluated, how to build reusable address and port groups, and how to translate common requirements into a working ruleset.

> **Before you start:** Review [Network Basics](/resources/documentation/network-basics/) so the interface names and zone concepts are familiar. Rules reference interfaces directly, so you need to know which interface carries which traffic.

## The Default Policy

Stratum Gateway ships with a **default-deny** posture:

- **WAN inbound** — all traffic denied unless explicitly allowed
- **LAN outbound** — all traffic allowed by default
- **Inter-VLAN** — denied by default; each VLAN must be permitted explicitly

This is the standard, sane starting point. If something does not work, it is more likely because you have not permitted it yet than because a rule is blocking it.

You can tighten the LAN outbound policy later, but resist the urge to do so on day one — you will spend more time chasing self-inflicted outages than solving real problems.

## Anatomy of a Rule

Every firewall rule in Stratum is defined by the same six attributes:

    Action        — what to do when the rule matches
    Interface     — which interface the rule listens on
    Source        — who is sending the traffic
    Destination   — who is receiving the traffic
    Service       — which protocol and port
    Schedule      — when the rule is active (optional)

When all five required attributes (schedule is optional) match a packet, the rule's action is applied and evaluation stops.

### Action

Three actions are available:

| Action | Effect |
|--------|--------|
| **Pass** | Allow the packet through |
| **Block** | Silently drop the packet; no response sent |
| **Reject** | Drop the packet and return an ICMP or TCP reset to the sender |

**Block** is preferred in almost every case. A silent drop gives the sender nothing to work with, which slows down port scanners and misconfigured clients alike. Reserve **Reject** for cases where you want a client to fail fast — for example, blocking an internal service on a LAN where you want the user to see an immediate error rather than a timeout.

### Interface

The interface is the one the rule *listens on* — the incoming side. For a rule that allows LAN users to reach the internet, the interface is the LAN interface. For a rule that allows an external client to reach an internal server, the interface is the WAN interface.

    Interface: LAN

> **Common mistake:** Using the WAN interface on an outbound rule and then wondering why nothing matches. The interface is where the traffic *arrives*, not where it is going.

### Source and Destination

Source and destination can be any of:

- A single host (`192.168.1.50`)
- A network (`192.168.1.0/24`)
- An interface address (`LAN net`, `WAN address`)
- An alias — a named group of hosts, networks, or ports (see the next section)
- `any` — everything

Where possible, use aliases instead of raw addresses. Aliases make rules readable and let you change an address in one place instead of hunting through dozens of rules.

### Service

The service defines the protocol and port. Common services are predefined (HTTPS, SSH, DNS, NTP, and so on). For anything not predefined:

    Protocol: TCP / UDP / ICMP / any
    Port:     80, 443, 8000-8999 (ranges supported)

A rule with `protocol: any` and no port matches everything — useful as a fallback, dangerous as a habit.

### Schedule

Rules can be scoped to a time window. The schedule references a named time period defined under **Firewall Rules → Schedules**. Common uses:

- Allow guest Wi-Fi only during business hours
- Permit a maintenance SSH rule only on Sunday mornings
- Block social media categories during work hours

If a rule has no schedule, it is always active.

## How a Rule Is Evaluated

Stratum uses **first-match** evaluation: rules are checked in order, top to bottom, and the first rule whose criteria match a packet wins. Nothing after it is evaluated for that packet.

This means:

- **Order matters more than completeness.** A broad `Block any → any` rule near the top will shadow everything below it.
- **Specific rules go above general rules.** Put "allow SSH from the admin workstation" above "allow any LAN outbound".
- **The implicit default is deny.** If no rule matches, the packet is dropped.

When you reorder rules in the panel, the new order takes effect immediately after **Apply**. If a change breaks connectivity, you can revert from the change log — but console access is the only guaranteed recovery path, so test risky changes from a session that is not the one you are about to lock out.


## Objects and Aliases

Hardcoding IP addresses into rules is a maintenance trap. When a host moves, you edit every rule that mentions it. Objects and aliases exist to prevent that.

### What Is an Object?

An **object** is a named, reusable value. Stratum supports four object types:

| Type | Example | Used for |
|------|---------|----------|
| **Host** | `web-server-01` → 10.0.10.20 | A single IP |
| **Network** | `vlan-10-subnet` → 10.0.10.0/24 | A CIDR range |
| **Port** | `web-ports` → 80, 443, 8080-8090 | One or more ports |
| **URL** | `blocked-sites` → list of domains | Web filtering integration |

An **alias** is a named group of objects. Where an object is a single value, an alias can hold many:

    Alias: internal-web-servers
      - web-server-01   (10.0.10.20)
      - web-server-02   (10.0.10.21)
      - web-server-03   (10.0.10.22)

A rule can reference either directly. Nested aliases are supported — an alias can contain other aliases, which is how you build up "all production servers" from smaller groups.

### Creating an Object

**Firewall Rules → Objects → New**

    Name:     web-server-01
    Type:     Host
    Value:    10.0.10.20
    Comment:  Primary customer portal

Once saved, `web-server-01` is available as a source or destination in any rule.

### When to Use Aliases

Use an alias instead of raw addresses when:

- The same group of hosts appears in more than one rule
- The group is likely to change over time (new web servers, retired workstations)
- You want a rule to be readable six months from now

Do **not** wrap every address in an alias just for consistency. A one-off host that appears in one rule and will never change is fine inline. Over-aliasing makes the object list hard to navigate.

### Importing and Exporting

The object list can be exported as JSON and imported into another gateway. This is the recommended way to replicate a rule baseline across a fleet. The export covers objects and aliases only — rules themselves are exported separately under **Firewall Rules → Rules → Export**.

## Floating Rules

Most rules are attached to an interface. A **floating rule** is attached to no interface and is evaluated against *all* traffic that passes through the firewall, in the order it appears in the floating rule list, before interface-specific rules.

Floating rules are the right tool for:

- **Global blocks** — a country or reputation feed that should be blocked no matter which interface the traffic arrives on
- **Global allowlists** — management traffic from a trusted network that should always be permitted
- **Logging** — record all traffic to a specific destination without affecting its flow

Because they run before interface rules and match against all traffic, floating rules are powerful and easy to misconfigure. Two habits keep them safe:

1. **Put floating rules above interface rules in evaluation order** and keep the list short. Ten floating rules is a smell; two or three is normal.
2. **Restrict the match as tightly as possible.** A floating rule with `source: any, destination: any, action: block` will brick the entire network.

### Floating Rule Ordering

Floating rules are evaluated top-to-bottom before any interface rule runs. If a floating rule matches, evaluation stops for that packet — the interface rules never see it.

This makes the typical layout:

    [Floating]  Block known-malicious IPs          → block
    [Floating]  Allow admin access from HQ         → pass
    [Floating]  Log all traffic to 10.0.20.5       → pass + log

    [LAN]       Allow LAN to any                    → pass
    [LAN]       Block LAN to inter-VLAN             → block

    [WAN]       Block all inbound                   → block
    [WAN]       Allow HTTPS to web-server-01        → pass

The floating section handles cross-cutting policy. The interface sections handle interface-specific behavior. Keep the boundary clean and the ruleset stays readable.


## Rules and NAT

Firewall rules and NAT rules are separate in Stratum, but they work together. Understanding the relationship prevents a whole category of "why doesn't this work?" problems.

### Order of Operations

For any packet crossing the firewall, the order is:

1. **NAT (destination)** — DNAT rewrites the destination address before filtering
2. **Filter rules** — the firewall rule engine evaluates the packet
3. **NAT (source)** — SNAT rewrites the source address on the way out

This order matters. A port-forward rule translates the destination before the firewall sees it, which means filter rules must match the **translated** destination (the internal server), not the public IP that the client actually connected to.

### Port Forwarding (DNAT)

Publishing an internal service to the internet requires two steps:

**Step 1 — Create the NAT rule.** **NAT → Port Forward → New**

    Interface:   WAN
    Protocol:    TCP
    Dest. port:  443
    Redirect to: web-server-01 (10.0.10.20)
    Redirect port: 443

**Step 2 — Create the matching filter rule.** **Firewall Rules → New**

    Interface:    WAN
    Source:       any
    Destination:  web-server-01
    Service:      HTTPS
    Action:       Pass

Without the filter rule, the DNAT happens but the packet is then blocked by the default WAN inbound policy. This is the single most common port-forwarding mistake.

### Source NAT (SNAT)

SNAT is applied automatically on WAN outbound traffic — LAN clients reaching the internet are masqueraded behind the WAN address. You rarely need to create SNAT rules manually.

The cases where you do:

- **Multiple WAN links** — force a specific source to always exit through a specific WAN
- **VPN routing** — present a specific source address to a remote peer
- **Policy routing** — match traffic to a specific upstream based on source

For most setups, leave SNAT on automatic. Manual SNAT rules are a debugging tool, not a default.

### Hairpin NAT

If clients on your LAN need to reach an internal server through its public hostname, you need **hairpin NAT** (also called NAT reflection). Without it, the client sends a packet to the public IP, the router forwards it out to the internet, and the reply path does not come back through the same interface.

Enable hairpin NAT per port-forward rule with the **NAT reflection** checkbox. Test from a client behind the same firewall — the request should succeed, not time out.

## Logging

Every rule has a **Log** toggle. When enabled, matching packets are written to the firewall log and become visible under **Firewall Rules → Log** and in the Dashboard's recent activity feed.

### Logging Strategy

Logging every allowed packet floods the log and makes it useless. A practical approach:

- **Log all blocked traffic** on the WAN interface — this is your intrusion detection baseline
- **Log specific allows** that you are actively debugging or auditing
- **Do not log** broad LAN-to-any allow rules; you will drown in normal traffic

### What a Log Entry Contains

    Timestamp   2026-09-14 12:04:32
    Interface   WAN
    Action      block
    Source      185.220.101.42:54321
    Dest        10.0.0.1:443
    Rule        [Floating] Block known-malicious IPs
    Proto       TCP

If a rule fires more than you expect, this is where you find out. Sort by source to spot scanners, sort by rule to spot misfiring policy.

### Firewall Log vs. System Log

The firewall log is a filtered view — only rule-matched events. The system log (under **System Settings → Logs**) contains everything else: service startups, DHCP leases, VPN handshakes, and errors. When something is not working, check the system log first; when something is being blocked that should not be, check the firewall log.

## Troubleshooting

Three tools cover 95% of firewall problems.

### 1. The Live Rule View

**Firewall Rules → Live** shows rules as they are currently loaded, in evaluation order, with match counters. A rule with a zero counter after hours of expected traffic is a rule that is not matching — either the criteria are wrong or an earlier rule is shadowing it.

### 2. The Packet Trace

**Firewall Rules → Trace** accepts a simulated packet (source, destination, port, protocol, interface) and reports which rule would match. This is the fastest way to answer "why is this traffic being blocked?" without touching a client.

Use the trace tool whenever you are about to change a rule that affects traffic you cannot easily reproduce. It answers the question before the change, not after.

### 3. The Console

If a rule change locks you out of the panel, the only recovery is the physical console (VGA or serial). From there you can:

- Revert to the last known-good configuration via **stratum-fw config rollback**
- Edit the rule file directly if you know what you are doing
- Reboot into the previous configuration slot

The lesson: **never make a firewall rule change on the interface you are connected through without a console session open on the appliance.** This rule has saved more networks than any other single piece of advice in this guide.


## Worked Examples

Three concrete scenarios that cover most real-world needs. Each example lists the objects to create, the NAT rules (if any), and the filter rules in evaluation order.

### Example 1 — Publishing a Web Server to the Internet

**Goal:** A server on the LAN at `10.0.10.20` serves HTTPS on port 443. External clients must reach it via the WAN address. LAN clients must also reach it (via hairpin).

**Objects to create:**

    web-server-01     Host     10.0.10.20

**NAT rules (NAT → Port Forward):**

    Interface:      WAN
    Protocol:       TCP
    Dest. port:     443
    Redirect to:    web-server-01
    Redirect port:  443
    Reflection:     enabled

**Filter rules (Firewall Rules → New), in order:**

    #   Interface   Source    Destination     Service   Action
    1   WAN         any       web-server-01   HTTPS     Pass     + log
    2   LAN         any       web-server-01   HTTPS     Pass

**Why the order matters:** The WAN filter rule must exist even though the DNAT has already rewritten the destination. Without it, the packet hits the default WAN inbound deny and is dropped. Rule 2 exists only because hairpin NAT sends the packet back through the filter engine; without it, LAN clients reach the server through the WAN path and get blocked.

### Example 2 — Restricting Outbound Access from a VLAN

**Goal:** The IoT VLAN (`10.0.30.0/24`) should reach the internet only for DNS, NTP, and HTTPS to a specific update server. Everything else is blocked.

**Objects to create:**

    iot-subnet        Network   10.0.30.0/24
    iot-blocked       Alias     (empty, populated later by exception rules)
    update-server     Host      update.stratumfw.com resolves to 203.0.113.10

**Filter rules (Firewall Rules → New), in order:**

    #   Interface   Source        Destination      Service      Action
    1   LAN         iot-subnet    any              DNS          Pass
    2   LAN         iot-subnet    any              NTP          Pass
    3   LAN         iot-subnet    update-server    HTTPS        Pass
    4   LAN         iot-subnet    any              any          Block     + log

**Why the order matters:** The block rule is last. If it were first, none of the allows would ever be evaluated. First-match evaluation rewards specific-above-general every time.

**A note on DNS:** Rule 1 permits DNS to *any* destination. In a hardened deployment you would restrict this to the gateway's own resolver (which then enforces its own policies), but the simple version above is a reasonable starting point.

### Example 3 — Isolating Two VLANs from Each Other

**Goal:** The guest VLAN (`10.0.40.0/24`) must not reach the internal VLAN (`10.0.10.0/24`), but both must reach the internet. Internal clients must not be able to initiate connections into the guest VLAN either.

**Objects to create:**

    guest-subnet      Network   10.0.40.0/24
    internal-subnet   Network   10.0.10.0/24

**Filter rules (Firewall Rules → New), in order:**

    #   Interface   Source            Destination        Service   Action
    1   LAN         guest-subnet      internal-subnet    any       Block    + log
    2   LAN         internal-subnet   guest-subnet       any       Block
    3   LAN         any               any                any       Pass

**Why the order matters:** The two block rules must precede the broad allow. Put the allow on top and the VLANs become one flat network — exactly what you were trying to avoid.

**Cross-VLAN rules and the LAN interface:** Both block rules sit on the LAN interface because VLAN traffic is delivered to the firewall on a subinterface of the LAN physical port. The firewall evaluates inter-VLAN traffic as LAN-ingress traffic, and matches against the source and destination networks.

### A Pattern Worth Remembering

All three examples follow the same shape:

1. **Block the specific exceptions** (or allow them, depending on the security posture) **first**.
2. **Allow the general case last.**
3. **Log the block rules**, not the allow rules.
4. **Test with the trace tool** before applying.

Once you internalize that shape, the exact rules for any new scenario follow almost mechanically.


## Where to Go Next

- **Link redundancy and failover** → [Multi-WAN](/resources/documentation/multi-wan/)
- **Remote access for users and admins** → [WireGuard VPN](/resources/documentation/vpn/)
- **Threat detection on top of the rule engine** → [IDS (Suricata)](/resources/documentation/ids/)
- **Interface and zone concepts referenced above** → [Network Basics](/resources/documentation/network-basics/)
- **Recovering a locked-out firewall** → the console recovery section earlier in this guide

If you are new to the platform, the recommended reading order after this page is: Network Basics → Firewall Rules (this page) → Multi-WAN → VPN. That sequence builds the mental model the rest of the documentation assumes.

