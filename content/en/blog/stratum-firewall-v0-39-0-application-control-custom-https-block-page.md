---
title: "Stratum Firewall: Capsicum-Sandboxed Application Control, Customizable HTTPS Block Page & Block Page CA"
date: 2026-11-23
author: "Stratum Security"
translationKey: "v0-39-0-application-control-block-page"
aliases:
  - /blog/stratum-firewall-v0-38-0-application-control-stratum-dpi-blocking-rules/
description: "Stratum Firewall introduces Application Control rules (stratum-dpi in a FreeBSD Capsicum sandbox), customizable HTTPS Block Pages with your own logo and top banner via Block Page CA (.cer GPO / .crt), shared-CDN-safe connection cutting, and a 16th read-only MCP AI tool."
lead: "Block unwanted applications across your VLANs and show users a clear, branded HTTPS block page — without placing a full-traffic MITM proxy in the packet path."
images: ["/images/manual/appctl-blockpage.png"]
tags: ["firewall", "stratum firewall", "freebsd 15.1", "application control", "block page", "dpi", "capsicum", "group policy", "mcp"]
---

When an employee or student tries to open a restricted application, a raw browser connection reset leaves helpdesks answering *"is the internet down?"* — yet traditional firewalls only show an HTTPS block page by **decrypting every innocent TLS session inline**.

With **Stratum Firewall** on [Stratum Firewall](https://stratumfw.com/), **`Firewall > Application Control`** combines out-of-path **FreeBSD Capsicum** DPI (`stratum-dpi`), **Monitor only** pre-deployment auditing, shared-CDN-safe `pf` blocking, and a **customizable HTTPS Block Page** backed by **Block Page CA** — signing certificates *only* for blocked domains without inspecting normal HTTPS traffic. Full setup details are live in [Chapter 06.8 — Firewall > Application Control](/manual/firewall-application-control/), [Chapter 09.6 — System > Certificates](/manual/system-certificates/#block-page-ca), and the [Official Changelog](/download/#changelog).

![Stratum Firewall Application Control, Custom Block Page, and stratum-dpi Architecture](/images/application-control-architecture.svg)

### 1. Customizable HTTPS Block Page & Group Policy `Block Page CA`

![Firewall > Application Control > Block page](/images/manual/appctl-blockpage.png)

A rule in **Block** mode can now display a branded block page naming the **site**, **application**, **rule**, **device**, and **time** instead of a connection error:

- **Unbound `198.18.0.1` Steering (Zero Full-Traffic MITM):** For networks covered by the rule (excluding device exceptions), Unbound answers ~1,000 nDPI application domains and your **Custom applications** with `198.18.0.1`, refuses QUIC to force clean HTTPS-over-TCP, signals Firefox not to switch to DoH, and directs the request to the unprivileged `_stratumbp` block page server.
- **`System > Certificates > Block Page CA`:** Create the CA once on the firewall and download it as **`.cer`** (for Windows / Active Directory **Group Policy**, with GPO steps right on the page) or **`.crt`** (PEM). Managed devices show the branded HTTPS block page cleanly; unmanaged devices see a certificate warning and remain blocked. You can also now download the **web panel certificate** directly from `System > Certificates`.
- **Instant Live-Preview Branding:** Upload your own **logo** and a wide **top picture / banner** (`PNG` / `JPEG` / `WebP`), customize the organisation name, title, texts, contact line, colour, and shown fields in any language — live at once on **Save** without **Apply**.
- **Privilege-Separated `_stratumbp`:** Runs as its own unprivileged user, unreachable directly from any network, and devices bypassing Unbound DNS are still blocked via `stratum-dpi` + `pf` connection cutting.

### 2. Out-of-Path `stratum-dpi`, Monitor-Only Rules & 16th MCP Tool

![Firewall > Application Control > Rules](/images/manual/appctl-rules.png)

| Capability | How Stratum Firewall Protects Production Networks |
| :--- | :--- |
| **Capsicum-Sandboxed `stratum-dpi`** | Reads a passive packet copy per VLAN (`nDPI` LGPLv3 + Stratum TLS/QUIC parser); traffic keeps flowing even if stopped |
| **Monitor Only Default** | Every new rule starts in **Monitor only**, logging *Would block* hits alongside *Page shown* and *Blocked* counts on the **Blocked** tab |
| **Shared-CDN-Safe HTTPS & VPN Blocking** | Acts only on high-confidence SNI/payloads (never port/IP guesses, DNS/DHCP/NTP, or RFC1918), cutting single TLS flows without breaking shared Cloudflare/Google IPs and blocking unnamed VPN tunnels for 10 minutes |
| **16th Read-Only MCP Tool** | The [AI Assistant](/product/ai-assistant/) and MCP clients query *Application usage*, active rules, and blocked counts 100% read-only |

### Try or Upgrade Stratum Firewall

Upgrade in one click under **`System > Update`** (`stratum-update run`) with automatic OpenZFS Boot Environment protection, read [Chapter 06.8](/manual/firewall-application-control/) and [Chapter 09.6](/manual/system-certificates/#block-page-ca), grab installer media on [Download](/download/), or explore the [Live Demo](https://demo.stratumfw.com).
