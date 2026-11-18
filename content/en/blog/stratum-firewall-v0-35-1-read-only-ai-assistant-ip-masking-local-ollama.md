---
title: "Stratum Firewall v0.35.1: Built-In Read-Only AI Assistant, PUB-1 IP Masking, and Local Ollama"
date: 2026-11-18
author: "Stratum Security"
translationKey: "v0-35-1-ai-assistant"
description: "Stratum Firewall v0.35.1 introduces Diagnostics > AI Assistant powered by 14 read-only MCP tools, deterministic rule math, automatic PUB-1 public IP masking for cloud LLMs, and 100% air-gapped LAN Ollama inference."
lead: "Ask your FreeBSD 15.1 firewall natural-language diagnostic questions with zero configuration-change risk, automatic public IP masking before cloud calls, or 100% on-premise LAN Ollama execution."
images: ["/images/ai-assistant-ui.jpg"]
tags: ["firewall", "stratum firewall", "freebsd 15.1", "ai assistant", "mcp", "ollama", "privacy", "v0.35.1"]
---

Putting an AI assistant inside a production network firewall usually raises two immediate engineering objections: **what if the model hallucinates a rule change and locks out a site**, and **why would anyone send live WAN IPs, firewall states, or secrets to an external cloud API?**

With **Stratum Firewall v0.35.0 and v0.35.1** on [Stratum Firewall](https://stratumfw.com/), we built **`Diagnostics > AI Assistant`** specifically to eliminate both risks at the architectural level rather than relying on prompt instructions. You can explore the full design on the [Built-In AI Assistant](https://stratumfw.com/product/ai-assistant/) page and review every release note in the [Official Changelog](https://stratumfw.com/resources/changelog/).

![Stratum Firewall Built-In Read-Only AI Assistant UI](/images/ai-assistant-ui.jpg)

### 1. Strictly Read-Only by Architecture: 14 Live MCP Diagnostic Tools

Unlike agentic wrappers that run shell commands or mutate configuration files, `Diagnostics > AI Assistant` has **zero write paths**. It cannot edit `config.json`, reload `pf`, flush state tables, restart `strongSwan` or `Unbound`, or toggle a single firewall rule.

Instead, the assistant queries the exact same **14 read-only Model Context Protocol (MCP) tools** exposed by Stratum Firewall's Python 3.12 / FastAPI control plane:

- **Live Firewall & NAT Inspection:** Reads active `pf` filter rules, NAT port-forwards, outbound NAT mappings, `dummynet` bandwidth limiters, and interface state counters.
- **5-Layer Security Guard Visibility:** Correlates blocked traffic across [Security Guard](https://stratumfw.com/product/security-guard/) IP blocklists (`63` feeds), `5`-RIR GeoIP rules, `93` Unbound RPZ DNSBL lists, `56` UT1 Web Filter categories, and `52` Suricata inline IPS categories.
- **Multi-WAN & Multi-Vendor VPN Health:** Checks gateway latency/loss, `strongSwan` IKEv2/IKEv1 Site-to-Site IPsec (`enc0`) security associations, and dedicated FreeBSD kernel WireGuard (`wg0`, `wg1+`) peer handshakes introduced in our [v0.34.1 Multi-Vendor S2S VPN release](https://stratumfw.com/blog/stratum-firewall-v0-34-1-ipsec-wireguard-s2s-vlan-matrix-hitless-suricata/).
- **Transparent Tool Citations:** Every single answer lists the exact MCP tools executed right underneath the response so administrators can verify where each figure came from, and points to the exact Web UI page when a manual configuration change is needed.

### 2. Deterministic Rule Math & Explicit Counter Semantics (`v0.35.1`)

Early testing with real firewall rulebases highlighted a classic LLM pitfall: when a firewall has dozens of VLAN, WAN, and VPN rules, asking a language model to count enabled versus disabled rules from a JSON list can produce off-by-one errors, and models often misinterpret `0` packet counters as a broken rule.

**Stratum Firewall v0.35.1** solves both deterministically in Python before the model ever writes a token:

- **Pre-Computed Rule & NAT Totals:** Rule and NAT inspection tools now return exact `all`, `enabled`, and `disabled` integer breakdowns computed directly by the control plane.
- **Explicit Zero-Counter Semantics:** Every `0` packet/byte counter is annotated with explicit operational context (`not matched since last reload/Apply`), preventing false alarms right after a Safe-Apply reload or boot environment switch. Production verification against DeepSeek confirmed exact parity between live `pf` counters and assistant answers.

### 3. `PUB-1` Public IP Masking, Secret Isolation & RAM-Only Sessions

Network administrators rightfully refuse to leak public WAN endpoints, remote IPsec gateway IPs, or credentials to third-party APIs. Stratum Firewall enforces four hard privacy boundaries documented on our [Security & Verification](https://stratumfw.com/product/security/) page:

![Stratum Firewall PUB-1 IP Masking and Local Ollama Architecture](/images/ai-assistant-privacy.jpg)

| Privacy & Security Control | Local LAN `Ollama` Mode | Cloud LLM Mode (`Claude`, `OpenAI`, `DeepSeek`, `Gemini`) |
| :--- | :--- | :--- |
| **Network Data Path** | 100% local LAN HTTP call; zero packets leave your network | Outbound HTTPS to your chosen API endpoint |
| **Public IP Protection** | Stays strictly inside your private LAN | Masked automatically as `PUB-1`, `PUB-2`... before transmit; restored locally in UI |
| **Secrets & `0600` Keys** | Never exposed to MCP tools or prompts | Never exposed to MCP tools or prompts |
| **Chat Retention** | Ephemeral RAM-only (1-hour TTL, zero disk logs) | Ephemeral RAM-only (1-hour TTL, zero disk logs) |
| **Access Scope** | `Administrators` role only | `Administrators` role only |

When `PUB-1` masking is active (enabled by default for all cloud providers), a cloud model only sees anonymized tokens such as `PUB-1` and `PUB-2` while analyzing routing tables, WAN failover states, or blocked scanner hits. Once the response arrives, Stratum Firewall maps those tokens back to your real public IPs locally in memory so the answer reads naturally on screen. Passwords, `0600` root-only key files, API tokens, TOTP seeds, and WireGuard/IPsec private keys are architecturally unreachable from the MCP layer.

### 4. Five Supported Providers Including 100% Air-Gapped Local `Ollama`

Stratum Firewall never locks you into a Stratum-hosted cloud proxy or recurring AI subscription (`$0/year`):

- **Local `Ollama` on LAN (Zero External Traffic):** Point Stratum Firewall at any `Ollama` server on your management LAN or GPU workstation running `Qwen`, `Llama`, `Mistral`, or `DeepSeek` models. Every diagnostic query stays completely inside your perimeter.
- **Bring Your Own API Key:** Prefer frontier cloud reasoning? Connect **Anthropic Claude**, **OpenAI**, **DeepSeek**, or **Google Gemini** directly with automatic `PUB-1` IP masking active out of the box.

### Practical Questions You Can Ask Right Now

Instead of jumping across five diagnostic tabs during an incident, administrators can ask direct operational questions in English or Turkish:

1. *"Why can hosts on VLAN 30 not reach port 443 on 10.20.0.15, and which rule or blocklist matches first?"*
2. *"Summarize all enabled vs. disabled WAN and LAN rules and highlight any rules with zero matches since the last reload."*
3. *"Are all Site-to-Site IPsec and kernel WireGuard tunnels healthy right now, or is any peer missing recent handshakes?"*
4. *"Which Security Guard IP feeds or Suricata IPS categories triggered the most drops in the current log window?"*

### Try or Upgrade to `v0.35.1`

- **Existing Stratum Firewall Deployments:** Upgrade in one click via **`System > Update`** with automatic OpenZFS Boot Environment (`bectl`) snapshot protection before applying.
- **New Installations:** Grab the FreeBSD 15.1 `amd64` installer from [Download Stratum Firewall](https://stratumfw.com/download/) (with `SHA256SUMS` verification) and update directly to `v0.35.1`, explore the live interface on the [Live Demo](https://demo.stratumfw.com), compare capabilities on [Compare Firewalls](https://stratumfw.com/compare/), or follow public documentation and release tracking on [GitHub (`stratum-panel`)](https://github.com/kaanozkan-81/stratum-panel).
