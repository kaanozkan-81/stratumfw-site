---
title: "Stratum Firewall: Built-In Read-Only AI Assistant, PUB-1 IP Masking, Local Ollama, and Smarter IDS Alerts"
date: 2026-11-19
author: "Stratum Security"
translationKey: "v0-35-2-ai-assistant"
aliases:
  - /blog/stratum-firewall-v0-35-1-read-only-ai-assistant-ip-masking-local-ollama/
description: "Stratum Firewall introduces Diagnostics > AI Assistant powered by 15 read-only MCP tools, deterministic rule math, PUB-1 public IP masking, 100% air-gapped LAN Ollama inference, and reachability-aware Suricata IDS notifications."
lead: "Ask your FreeBSD 15.1 firewall natural-language diagnostic questions with zero configuration-change risk, automatic public IP masking, local LAN Ollama execution, and noise-free IDS alert notifications."
images: ["/images/ai-assistant-ui.jpg"]
tags: ["firewall", "stratum firewall", "freebsd 15.1", "ai assistant", "mcp", "ollama", "suricata"]
---

Putting an AI assistant inside a production network firewall usually raises two immediate engineering objections: **what if the model hallucinates a rule change and locks out a site**, and **why would anyone send live WAN IPs, firewall states, or secrets to an external cloud API?**

With **Stratum Firewall** on [Stratum Firewall](https://stratumfw.com/), we built **`Diagnostics > AI Assistant`** specifically to eliminate both risks at the architectural level rather than relying on prompt instructions, alongside smarter reachability-aware Suricata IDS alerting in. Explore the full design on the [Built-In AI Assistant](https://stratumfw.com/product/ai-assistant/) page and review every release note in the [Official Changelog](https://stratumfw.com/download/#changelog).

![Stratum Firewall Built-In Read-Only AI Assistant UI](/images/ai-assistant-ui.jpg)

### 1. Strictly Read-Only by Architecture: 15 Live MCP Diagnostic Tools

Unlike agentic wrappers that run shell commands or mutate configuration files, `Diagnostics > AI Assistant` has **zero write paths**. It cannot edit `config.json`, reload `pf`, flush state tables, restart `strongSwan` or `Unbound`, or toggle a single firewall rule. Any real configuration change stays exclusively under administrator control with pre-flight syntax checks and a **120-second Safe-Apply** automatic rollback from the web panel.

Instead, the assistant queries the **15 read-only Model Context Protocol (MCP) tools** exposed by Stratum Firewall's Python 3.12 / FastAPI control plane:

- **Live Firewall & NAT Inspection:** Reads active `pf` filter rules, NAT port-forwards, outbound NAT mappings, `dummynet` bandwidth limiters, and interface state counters.
- **5-Layer Security Guard Visibility:** Correlates blocked traffic across [Security Guard](https://stratumfw.com/product/security-guard/) IP blocklists (`63` feeds), `5`-RIR GeoIP rules, `93` Unbound RPZ DNSBL lists, `56` UT1 Web Filter categories, and `52` Suricata inline IPS/IDS categories.
- **Multi-WAN & Multi-Vendor VPN Health:** Checks gateway latency/loss, `strongSwan` IKEv2/IKEv1 Site-to-Site IPsec (`enc0`) security associations, and dedicated FreeBSD kernel WireGuard (`wg0`, `wg1+`) peer handshakes introduced in our [Multi-Vendor S2S VPN release](https://stratumfw.com/blog/stratum-firewall-v0-34-1-ipsec-wireguard-s2s-vlan-matrix-hitless-suricata/).
- **Transparent Tool Citations:** Every single answer lists the exact MCP tools executed right underneath the response so administrators can verify where each figure came from, and points to the exact Web UI page when a manual configuration change is needed.

### 2. Deterministic Rule Math & Reachability-Aware IDS Alerts

Real-world testing across active perimeter gateways shaped two targeted refinements across production deployments:

- **Pre-Computed Rule & Zero-Counter Math:** Asking an LLM to count enabled versus disabled rules from a JSON list can produce off-by-one errors, and models often misread `0` packet counters as a broken rule. Firewall and NAT MCP tools now return exact pre-computed `all`, `enabled`, and `disabled` integer counts in Python and annotate `0` counters explicitly as `not matched since last Apply/reload`.
- **Notifications Only for Reachable Intrusion Alerts:** Because Suricata inspects a WAN interface before `pf` filters incoming packets, internet-wide exploit scans aimed at unopened UDP/TCP ports (such as CVE-2021-35394 on UDP `9034`) previously triggered high-severity Telegram and e-mail notifications even though `pf` dropped every packet. Stratum FW resolves what is genuinely reachable on its WAN addresses (port forwards with source restrictions and port aliases, WAN pass rules including interface groups, WAN management, OpenVPN, WireGuard, and IPsec) and sends notifications only for alerts originating inside, targeting inside hosts, or hitting an open port for that source. Unopened-port hits are never hidden: they remain on the IDS page, in logs, and in MCP / AI Assistant responses clearly marked **`Port closed`** (with an opt-in toggle in `System > Settings > Notifications`).

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

When `PUB-1` masking is active (enabled by default for all cloud providers), a cloud model only sees anonymized aliases such as `PUB-1` and `PUB-2` while analyzing routing tables, WAN failover states, or blocked scanner hits. Once the response arrives, Stratum Firewall restores your real public IPs locally in memory so the answer reads naturally on screen. Keys and certificates generated via OpenSSL (the `openssl` tool and the Python `cryptography` package), passwords, `0600` root-only secret files, API tokens, and VPN configs are never reachable from the MCP layer.

### 4. Five Supported Providers Including 100% Air-Gapped Local `Ollama`

Stratum Firewall never locks you into a Stratum-hosted cloud proxy or recurring AI subscription (`$0/year`):

- **Local `Ollama` on LAN (Zero External Traffic):** Point Stratum Firewall at any `Ollama` server on your management LAN or GPU workstation running `Qwen`, `Llama`, `Mistral`, or `DeepSeek` models. Every diagnostic query stays completely inside your perimeter.
- **Bring Your Own API Key:** Connect **Anthropic Claude**, **OpenAI**, **DeepSeek**, or **Google Gemini** directly with automatic `PUB-1` IP masking and a built-in connection **Test** button.

### Practical Questions You Can Ask Right Now

Instead of jumping across five diagnostic tabs during an incident, administrators can ask direct operational questions in English or Turkish:

1. *"What was blocked in the last hour, and were any Suricata alerts aimed at an actually open WAN port?"*
2. *"Summarize all enabled vs. disabled WAN and LAN rules and highlight any rules with zero matches since the last Apply."*
3. *"Are all Site-to-Site IPsec and kernel WireGuard tunnels healthy right now, or is any peer missing recent handshakes?"*
4. *"Is WAN2 healthy, and is any inside device showing suspicious outbound blocks?"*

### Try or Upgrade Stratum Firewall

- **Existing Stratum Firewall Deployments:** Upgrade in one click via **`System > Update`** with automatic OpenZFS Boot Environment (`bectl`) snapshot protection before applying.
- **New Installations:** Grab the FreeBSD 15.1 `amd64` installer from [Download Stratum Firewall](https://stratumfw.com/download/) (with `SHA256SUMS` verification) and upgrade to the latest signed release, explore the live interface on the [Live Demo](https://demo.stratumfw.com), compare capabilities on [Compare Firewalls](https://stratumfw.com/compare/), or follow public documentation and release history on [GitHub (`stratum-panel`)](https://github.com/kaanozkan-81/stratum-panel).
