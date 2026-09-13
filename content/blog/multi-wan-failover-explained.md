---
title: "Multi-WAN Failover: What It Is and Why Your Business Needs It"
date: 2026-09-13
author: "Stratum Security Team"
description: "A dropped internet connection can cost a business thousands in lost productivity and revenue. Multi-WAN failover keeps you online by automatically switching between connections. Here's how it works and what to look for."
tags: ["multi-wan", "failover", "business-continuity", "networking"]
---

Every business that depends on the internet — which is to say, every business — has experienced the moment. The connection drops. The point-of-sale system stops processing. The video call freezes mid-sentence. The cloud app you rely on returns an error. Someone says, "Is the internet down?" and everyone looks up from their screens.

For a small business, this is inconvenient. For a business that transacts online, it's expensive. For a business with a customer on the phone, it's embarrassing.

The good news is that there's a well-established solution to this problem. It's called **multi-WAN failover**, and it's one of the most cost-effective investments a business can make in its network. This article explains what it is, how it works, and what to look for when evaluating a solution.

## What is multi-WAN failover?

"WAN" stands for Wide Area Network — in practice, it means the connection between your network and the internet. A "multi-WAN" setup means your network has more than one internet connection. "Failover" means that when one connection fails, traffic automatically shifts to another.

Put together, multi-WAN failover is the practice of using two or more internet connections and having your network automatically switch between them when one becomes unavailable.

The concept is simple. The implementation is where things get interesting.

## Why a single internet connection is a business risk

If your business has a single internet connection — a fiber line, a cable modem, or a business DSL circuit — you've accepted that your network's availability is entirely dependent on that one link. That dependency has consequences:

- **ISP outages are not rare.** Fiber cuts happen. Upstream routing issues happen. Scheduled maintenance happens.
- **Hardware fails.** Modems fail. Cables degrade. Weather affects physical infrastructure.
- **Your SLA doesn't prevent downtime.** An SLA gets you a credit. It doesn't get your point-of-sale system back online.
- **Recovery takes time.** Even a "fast" ISP response is measured in hours, not minutes. Averages are often worse.

Every minute of downtime has a cost. For some businesses, that cost is easy to quantify — a restaurant can't process cards, a law firm can't access case files, a retailer can't check inventory. For others, it's less direct but still real: productivity lost, customers frustrated, employees stuck.

## How failover actually works

A well-designed failover system has four components: detection, decision, transition, and recovery.

### 1. Detection

The system must determine whether a connection is actually working. This is harder than it sounds. An interface being "up" doesn't mean the connection is functional — the modem can be online while the ISP has a routing problem that makes destinations unreachable.

Good failover systems use multiple detection methods:

- **Physical link state:** Is the interface up?
- **ICMP (ping) checks:** Can we reach reliable remote hosts?
- **HTTP/HTTPS checks:** Can we reach a known endpoint and get a valid response?
- **DNS checks:** Can we resolve a name we know is stable?

The best systems combine these methods and adjust their sensitivity. Too aggressive, and you'll flip between connections during brief hiccups. Too relaxed, and you'll stay on a broken connection while users complain.

### 2. Decision

Once a connection is deemed unhealthy, the system needs to decide what to do. In a simple two-connection setup, the decision is easy: switch to the other one. In a setup with three or more connections, the decision involves priorities, weights, and rules.

Two common modes:

- **Active/Passive failover:** One connection is primary, the other is standby. Traffic uses the primary until it fails, then moves to the standby.
- **Load balancing (Active/Active):** Traffic is distributed across all available connections. If one fails, the remaining connections absorb the load.

Each has trade-offs. Active/passive is simpler and more predictable. Active/active uses bandwidth more efficiently but requires more careful configuration.

### 3. Transition

When the failover decision is made, how does traffic actually move?

This is the hardest part to get right. For a new connection, it's just a routing change. For existing connections — like an open video call, a database query, or a file upload — the transition is more disruptive. Most connections can't survive an IP change without breaking.

Well-designed systems handle this gracefully:

- **Stateful tracking:** The system tracks active sessions and attempts to preserve them where possible.
- **Fast switching:** The transition happens within a second or two, minimizing the impact on new connections.
- **Session recovery:** Applications that support reconnection (most modern software) recover automatically once the new path is available.

Some systems provide a **sticky backup** — a specific traffic class or user is pinned to a specific connection. Others provide **policy-based routing** — different traffic uses different connections depending on rules you define.

### 4. Recovery

When the failed connection comes back, what happens?

Two schools of thought:

- **Fail back automatically:** Move traffic back to the primary connection as soon as it's healthy. This preserves the intended design but adds another transition event.
- **Stay on the backup until manual switch:** Avoid unnecessary transitions but leave the network on its backup path indefinitely.

Most modern systems make this configurable. For some businesses, automatic failback is desirable. For others, it's better to avoid the extra transition if the backup is performing adequately.

## What to look for in a multi-WAN solution

Not all failover implementations are created equal. When evaluating a solution — whether it's a hardware appliance, a software platform, or a feature within a firewall — look for these capabilities:

**Multiple health-check methods.** ICMP alone is insufficient. Look for HTTP and DNS checks as options.

**Configurable thresholds.** You should be able to tune how many failed checks constitute a failure, and how long to wait before declaring a connection unhealthy.

**Policy-based routing.** Different traffic should be able to use different connections. For example, VoIP might prefer the lower-latency connection while bulk backups use the cheaper one.

**Load balancing modes.** In addition to failover, you should be able to distribute traffic across connections during normal operation. Look for weighted and equal-cost options.

**Session preservation.** The system should attempt to preserve active sessions during transitions. Not all sessions can survive, but the effort matters.

**Visibility.** You should be able to see which connection is currently active, its health status, and its traffic volume — in real time.

**Logs and alerts.** When a failover event happens, you should know about it. And you should be able to review the event later.

## Common deployment scenarios

Multi-WAN looks different depending on the environment. Here are three common patterns:

### Small business: primary + backup

Two connections, one primary, one backup. Typically a fiber or cable primary with an LTE or secondary fiber backup. The backup is usually cheaper and only used during outages. Simple, effective, and usually the right starting point.

### Growing business: two active connections

Two connections of similar speed and quality, both used actively. Traffic is balanced across them during normal operation, and either one can absorb the full load if the other fails. More expensive but provides both redundancy and increased capacity.

### Distributed business: per-site failover

Multiple sites, each with its own multi-WAN setup. Local decisions handle local outages. Central configuration ensures consistency. This is where cloud-managed solutions sometimes have an advantage — but self-hosted solutions can match them with the right tooling.

## The economics of failover

Multi-WAN failover costs money. You're paying for a second internet connection, and possibly additional hardware. Is it worth it?

The math depends on your business, but the framework is straightforward:

- **What is the cost of one hour of downtime?** Include lost revenue, lost productivity, customer frustration, and emergency response time.
- **How often do you experience downtime?** Even once a quarter is more than most businesses realize.
- **What does the second connection cost?** Business-grade backup connections are often less than the primary.

For most businesses with any meaningful dependence on the internet, the answer is yes — the second connection pays for itself the first time it saves an hour of downtime.

## A note on LTE and 5G backups

In the past few years, LTE and 5G have become genuinely viable as backup connections. They're not always as fast or as cheap as wired connections, but:

- They're often available immediately (no waiting for a fiber install)
- They're physically independent from wired infrastructure (a fiber cut doesn't affect them)
- Data caps are manageable for backup-only usage
- Speeds have improved dramatically with modern modems

For many small businesses, a wired primary plus an LTE backup is now the standard recommendation.

## How Stratum Gateway handles multi-WAN

Stratum Gateway includes multi-WAN failover and load balancing as a built-in capability. The implementation supports:

- **Multiple health-check methods** (ICMP, HTTP, DNS) with configurable thresholds
- **Weighted and equal-cost load balancing** across connections
- **Per-flow and per-packet distribution modes**
- **Automatic failback** when the primary connection recovers
- **Real-time visibility** into connection health, current routing, and traffic volumes

Configuration is done through the web panel. Changes are staged, confirmed, and applied safely — so a misconfigured failover rule never locks you out of the interface.

Because Stratum is self-hosted, the entire failover configuration lives on your own hardware. Nothing about your connections, your traffic, or your failover events leaves your network.

---

*If you want to see what multi-WAN management looks like in practice, the live dashboard demo runs a full Stratum Gateway instance with all modules active. [Take a look →](/try-me/)*

*Or learn more about Stratum's broader network capabilities on the [features page](/product/features/#multi-wan).*
