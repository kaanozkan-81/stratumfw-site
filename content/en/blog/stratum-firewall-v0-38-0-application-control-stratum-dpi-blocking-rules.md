---
title: "Stratum Firewall v0.38.0: Capsicum-Sandboxed Application Control, Monitor-First Rules & Surgical TLS/QUIC Blocking"
date: 2026-11-23
author: "Stratum Security"
translationKey: "v0-38-0-application-control"
description: "Stratum Firewall v0.38.0 introduces Firewall > Application Control > Rules powered by stratum-dpi 1.2 in a FreeBSD Capsicum sandbox — with Monitor-only auditing, shared-CDN-safe TLS/QUIC blocking, VPN tunnel detection, and a 16th read-only MCP AI tool."
lead: "See and control hundreds of applications across your VLANs without putting an inline proxy in the packet path or breaking shared Cloudflare/Google CDN addresses."
images: ["/images/manual/appctl-rules.png"]
tags: ["firewall", "stratum firewall", "freebsd 15.1", "application control", "dpi", "capsicum", "quic", "mcp", "v0.38.0"]
---

Blocking unwanted applications on modern networks — from unauthorized VPN tunnels and peer-to-peer traffic on guest Wi-Fi to social media or gaming onsegmented VLANs — fails quickly when a firewall either **sits inline as a fragile single point of failure** or **blindly blocks entire Cloudflare, Akamai, or Google IP ranges** that host dozens of business services on the same address.

With **Stratum Firewall v0.38.0** on [Stratum Firewall](https://stratumfw.com/), **`Firewall > Application Control`** combines an out-of-path FreeBSD **Capsicum**-sandboxed DPI engine (`stratum-dpi 1.2`) with surgical `pf` connection cutting, **Monitor only** pre-deployment auditing, and a **16th read-only MCP tool (`Application usage`)**. Read the full [Chapter 06.8 — Firewall > Application Control](/manual/firewall-application-control/) in the User Manual and the [Official Changelog](/resources/changelog/).

![Stratum Firewall v0.38.0 Application Control and stratum-dpi 1.2 Architecture](/images/application-control-architecture.svg)

### 1. Out-of-Path `stratum-dpi 1.2` in a FreeBSD Capsicum Sandbox

- **Never in the Packet Path:** Each watched internal interface runs its own unprivileged `stratum-dpi 1.2` engine reading a passive packet copy. Even if an engine is stopped or restarted, live network forwarding is untouched.
- **Instant Confidence Classification:** Combines `nDPI` (LGPLv3 components) with Stratum FW's own TLS `ClientHello` and QUIC `Initial` parsers — naming server-identified flows immediately without waiting for packet-count thresholds, and always preferring verified SNI over port or IP guesses.
- **Privacy-Minimal Storage:** Locked inside a **FreeBSD Capsicum** capability sandbox, storing only daily per-device and per-application totals (connections, RX/TX bytes, first/last seen) with zero individual URLs written to disk.

### 2. Monitor-First Rules & Shared-CDN-Safe TLS / QUIC Blocking

![Firewall > Application Control > Rules](/images/manual/appctl-rules.png)

Every rule under **`Firewall > Application Control > Rules`** matches watched interfaces, Stratum categories (*VPN and proxies*, *Social media*, *Games*, *AI assistants*...), individual or custom domain applications, and optionally **encrypted connections without a server name** (characteristic of stealth VPN and tunnel clients), with device/subnet exceptions:

| Capability | How `v0.38.0` Protects Production Networks |
| :--- | :--- |
| **Monitor Only Default** | Every new rule starts in **Monitor only**, logging *Would block* events on the **Blocked** tab so you can verify zero collateral impact before switching to **Block** |
| **Confidence-Gated Actions** | Acts **only** on high-confidence server names or packet payloads — port/address guesses, `DNS`/`DHCP`/`NTP`, and private RFC1918 destinations are never blocked |
| **Shared-CDN-Safe HTTPS Cut** | Cuts matching TLS flows connection-by-connection during the handshake without blocking unrelated sites sharing the same Cloudflare, Google, or Akamai IP |
| **10-Minute QUIC -> TCP Fallback** | Refuses QUIC to that server for that device for 10 minutes so browsers seamlessly fall back to TCP where every connection is checked individually by server name |
| **Instant Lift & `pf` Reload Continuity** | Stopping the collector or switching a rule off lifts every active block at once; `Apply` automatically reloads active blocks after `pf` reloads |

### 3. Dashboard Panel & 16th Read-Only MCP Tool (`Application usage`)

- **Role-Safe Dashboard Widget:** The **Applications** dashboard panel displays the last hour's top applications and categories without exposing individual client names.
- **16 Read-Only AI & MCP Tools:** Both the built-in [AI Assistant](/product/ai-assistant/) and external MCP clients can now query *Application usage* — including top apps per network or device, active rules, and what they blocked — while keeping configuration 100% read-only.

### Upgrade to `v0.38.0`

Upgrade existing installations in one click under **`System > Update`** (`stratum-update run`) with automatic OpenZFS Boot Environment rollback protection, explore [Chapter 06.8 in the User Manual](/manual/firewall-application-control/), download installer media on [Download](/download/), or try the interface in the [Live Demo](https://demo.stratumfw.com).
