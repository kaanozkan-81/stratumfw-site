---
title: "Why We Rewrote Stratum Gateway on FreeBSD (And Never Looked Back)"
date: 2026-11-03
author: "Stratum Security Team"
description: "A deep dive into our engineering journey from a fragmented Linux prototype to a modern, FreeBSD-based, API-driven NGFW with declarative configuration."
tags: ["freebsd", "architecture", "ngfw", "declarative-config"]
---

If you've been following the Stratum Gateway project since the beginning, you might have noticed a massive architectural shift. We went from a heavy, Debian-based Linux appliance with a dozen different external dependencies (Suricata, CrowdSec, Stalwart) to a sleek, cohesive **FreeBSD-based Network Security Operating System**.

We didn't do this because Linux is bad. We did it because we wanted to build a firewall that we, as network engineers, would actually want to use. 

Here is the story of why we pushed the reset button, and why our FreeBSD rewrite is the best decision we've ever made.

## The Problem with the "Collection of Tools" Approach

Our first prototype worked, but it was fragile. We were relying on `iptables` (and later `nftables`) glued together with Python scripts, an external IDS, and heavy database dependencies. 

The problem was **state**. If an update broke a package, the firewall broke. If a configuration script failed halfway through, you could lose connectivity. We were building a house of cards on top of an operating system that wasn't designed specifically to be a firewall.

## The Pivot: Why FreeBSD?

We needed an operating system that was designed from the ground up to route packets and filter traffic. We chose FreeBSD for three uncompromising reasons:

### 1. The `pf` Packet Filter
Originating from OpenBSD, `pf` is universally recognized as the gold standard for stateful packet inspection. It is incredibly fast, readable, and battle-tested. By using FreeBSD, we can interact directly with the kernel's native filtering engine instead of translating our rules through a userspace proxy.

### 2. Cohesive OS vs. A Bag of Packages
FreeBSD is a complete operating system. The kernel, the networking tools (`ifconfig`, `pfctl`), and the userland are developed together. We no longer worry about an upstream Linux distribution update breaking our networking stack overnight. It is predictable, stable, and secure.

### 3. In-Kernel Performance
From WireGuard running natively in the kernel to `ALTQ` traffic shaping, FreeBSD handles network workloads at wire speed, even on modest hardware.

## The Power of Declarative Configuration

Perhaps the most significant change isn't the OS, but how we handle configuration. 

In the old system, you had to manually edit `iptables` or `pf.conf` files. In Stratum FW, we introduced a **Declarative Configuration Engine**.

You configure your intent in the web UI. We save that intent to a single, human-readable `config.json` file. Then, our Python engine translates that JSON into native FreeBSD configuration files (`pf.conf`, `wg0.conf`, etc.).

**Why is this a game-changer?**

- **Zero Lockouts (Safe-Apply):** Before any configuration is applied to the kernel, our engine tests it using `pfctl -nf`. If you make a typo that would cut your connection, the system prevents you from applying it. If you accidentally block your WAN IP, a countdown gives you 60 seconds to confirm connectivity before it automatically rolls back.
- **Infrastructure-as-Code:** Because everything is driven by a single JSON file and a REST API, you can version-control your firewall. You can deploy a new branch office in seconds using Ansible or Terraform.
- **Absolute Transparency:** We don't hide our configurations in an SQLite database or a proprietary binary blob. You can SSH into the box, open `/etc/pf.conf`, and see exactly what the kernel is executing.

## Zero-Dependency WireGuard

To prove our commitment to a minimal attack surface, we didn't just wrap an external VPN library. We implemented the X25519 cryptography ourselves (following RFC 7748). This means WireGuard in Stratum FW has zero external dependencies, reducing the attack surface and ensuring it runs with maximum efficiency.

## The Road Ahead

We are currently at **v0.16.0 (Developer Preview)**. The core engine is running, the API is stable, and the FreeBSD foundation is rock solid. 

Over the coming months, we will release our pre-built ISO installer, finalize the IP/DNS Blocklists (for layer 3 and layer 7 threat protection), and introduce Multi-WAN failover logic.

We are building the firewall we always wanted. If you are a network engineer tired of bloated legacy appliances or cloud-managed systems that hold your data hostage, we invite you to join us.

---

*Want to see the architecture for yourself? Read our [Architecture Overview](/product/architecture/) or check out the source code on [GitHub](https://github.com/kaanozkan-81/stratum-panel).*
