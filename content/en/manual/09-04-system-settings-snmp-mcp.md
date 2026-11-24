---
title: "System > Settings: SNMP and MCP"
seo_title: "09.4 System > Settings: SNMP and MCP — Stratum FW User Manual"
description: "Monitoring the firewall with SNMP (LibreNMS, Zabbix, PRTG ...) and letting AI assistants read its state through the MCP server: tokens, allowed addresses, tools and client setup."
weight: 904
chapter_num: "09.4"
url: "/manual/system-settings-snmp-mcp/"
aliases:
  - "/manual/09-04-system-settings-snmp-mcp/"
translationKey: "manual-09-04-system-settings-snmp-mcp"
---

## SNMP

![System > Settings > SNMP](/images/manual/set-snmp.png)

Monitoring systems - LibreNMS, Zabbix, PRTG, Observium, Cacti - read the firewall over SNMP: the interfaces with
their 64-bit traffic counters, CPU, memory, disks, processes and pf statistics.

| Setting | Meaning |
| :--- | :--- |
| **Answer SNMP v1 / v2c queries (read-only)** | Switches the agent on. |
| **Community** | 8-64 letters, digits or `. _ ~ -`. Used like a password: choose a long random one, not `public`. Stored root-only and never shown again; leave the field empty to keep it. |
| **Allowed from** | One address or network per line - your monitoring server. Empty: any device on the chosen interfaces. |
| **Answer on** | The internal interfaces SNMP answers on. **Never on a WAN.** |
| **Location**, **Contact** | Shown to the monitoring system. |

Takes effect after **Apply**. Nothing can be changed over SNMP.

Useful OIDs:

| OID | Contains |
| :--- | :--- |
| `1.3.6.1.2.1.2`, `1.3.6.1.2.1.31` | Interfaces (IF-MIB), with 64-bit counters |
| `1.3.6.1.2.1.25` | CPU, memory, disks, processes (HOST-RESOURCES) |
| `1.3.6.1.4.1.12325.1.200` | pf statistics |

Test from the monitoring server:

```
snmpwalk -v2c -c <community> <firewall LAN address> 1.3.6.1.2.1.1
```

> **Note:** a monitoring server that is not in an internal network (for example in the cloud) cannot reach SNMP.
> Connect it over a VPN ([**VPN > WireGuard S2S**](/manual/vpn-wireguard-s2s/) or [**IPsec**](/manual/vpn-ipsec/)) so it queries an internal address.

## MCP

![System > Settings > MCP](/images/manual/set-mcp.png)

The **Model Context Protocol** lets an AI assistant - Claude, ChatGPT, Gemini, DeepSeek through an MCP client -
ask the firewall questions such as *"what was blocked in the last hour?"* or *"which devices are on the guest
network?"*, and answer with the firewall's real state.

**Everything is read-only**: no tool can change anything. Passwords, keys, tokens and VPN configurations are never
returned. The built-in **AI Assistant** ([**Diagnostics > AI Assistant**](/manual/diagnostics-ai-assistant/)) uses the same tools.

### Switch it on

1. Switch on **Let AI assistants read the firewall's state**.
2. Note the **Server address**: `https://<firewall>/mcp` (transport *streamable HTTP*).
3. Under **Access tokens**, enter a **Name** (for example *Kaan - laptop*) and optionally **Allowed from** -
   addresses that may use the token. Empty: any address that can reach the panel ([**Admin Access**](/manual/system-settings-general-access/)).
4. **Create token**. **Copy it now** - it is shown only once. The page also shows ready-made client settings with
   the token filled in.

Tokens are stored as hashes only; up to 20. The list shows each token's beginning, allowed addresses, **Last
used** (time and address) and **Calls**. **Revoke** ends a token at once. Wrong tokens count as failed sign-ins
and block the address like the sign-in page does.

### Tools

Each of the 16 tools can be switched off - **everything a tool returns is sent to the AI provider**, so switch off what you
do not want to share.

| Tool | Returns |
| :--- | :--- |
| System status | Host name, versions, uptime, CPU and load, memory, disk |
| Interfaces | Every interface with role, device, link, address and current rate |
| WAN status | Each WAN: gateway, latency and packet loss |
| Services | The services and whether each is running |
| Firewall log | Latest log entries with the matching rule; filters |
| Blocked traffic summary | The last hour: totals, top sources, destinations and ports |
| Firewall rules | The rules of one interface or group, in order, with automatic rules |
| NAT rules | Port forwards and outbound NAT |
| DHCP leases | Active leases and static mappings per network |
| VPN sessions | Connected OpenVPN users and WireGuard peers |
| Alerts | Recent events: WAN, blocked sign-ins, failed applies, rollbacks, updates ... |
| Configuration changes | Recent applies, confirmations, rollbacks |
| Blocklists | IP and DNS blocklists, entries, updates, errors |
| Intrusion detection alerts | Suricata's state and latest alerts |
| Traffic statistics | Average and peak rate and data per interface (last hour or day) |
| Application usage | Top applications, categories and devices from Application Control, plus active rules and what they blocked |

**Save** saves the switch and the tools; it takes effect at once.

### Connect a client

**Claude Code:**

```
claude mcp add --transport http stratum https://<firewall>/mcp --header "Authorization: Bearer <token>"
```

**Gemini CLI** (`~/.gemini/settings.json`):

```json
{ "mcpServers": { "stratum": { "httpUrl": "https://<firewall>/mcp",
                               "headers": { "Authorization": "Bearer <token>" } } } }
```

**Other clients:** URL `https://<firewall>/mcp`, header `Authorization: Bearer <token>`.

The panel certificate is self-signed unless you changed it, so MCP clients must trust it: download the panel certificate directly on [**System > Certificates**](/manual/system-certificates/) (as `stratum.crt`) and, for clients built on Node.js (Claude Code, Gemini CLI), start them with
`NODE_EXTRA_CA_CERTS=/path/to/stratum.crt`.
