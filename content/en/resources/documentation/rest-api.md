---
title: "REST API, MCP & SNMP"
description: "Automate Stratum Firewall via the FastAPI REST API, query live state from AI assistants over Model Context Protocol (MCP), and monitor metrics via read-only SNMP."
lead: "Automate configuration via REST API, inspect live firewall state safely with AI assistants over MCP, and integrate with LibreNMS, Zabbix, or PRTG via SNMP."
---

Stratum Firewall is built on an **API-first** Python 3.12 &amp; FastAPI architecture on FreeBSD 15.1, complemented by a read-only **Model Context Protocol (MCP)** server for AI assistants and a native FreeBSD **SNMP (`bsnmpd`)** agent.

## 1. REST API Authentication &amp; Safe-Apply

All management endpoints are served over HTTPS behind Nginx (`https://<firewall-ip>/api/`):

```bash
# 1. Sign in to obtain a session cookie
curl -k -X POST https://192.168.1.1/login \
  -d "username=admin&password=your_secure_password" \
  -c cookies.txt

# 2. Query live status (include CSRF protection header)
curl -k -X GET https://192.168.1.1/api/status \
  -b cookies.txt \
  -H "X-Requested-With: stratum"
```

* **RBAC Enforcement:** **Administrator** accounts have full CRUD access; **Operator** accounts can read state and trigger `/api/diag/*` actions; **Read-Only** accounts receive `403 Forbidden` on write methods.
* **Safe-Apply Confirmation:** Configuration writes validate syntax and return `apply_needed: true`. Trigger apply and call `/api/apply/confirm` within **60 seconds** to commit changes permanently.

## 2. AI Diagnostics via Model Context Protocol (`System → Settings → MCP`, `v0.14.0`)

Connect MCP-compatible AI clients (**Claude, ChatGPT, Gemini, Cursor, DeepSeek**) directly to your firewall with strict security boundaries:

* **Strictly Read-Only Tools (15+ Tools):** Query live system health, interfaces, WAN latency/failover, service status, firewall &amp; NAT rules (including interface groups), live firewall logs, blocked-traffic summaries, Suricata IPS alerts (`ids_alerts`), DHCP leases, WireGuard/OpenVPN sessions, and traffic graphs—**nothing can be modified via MCP**.
* **Scoped, Hashed Tokens &amp; Granular Privacy:** Create per-device access tokens (stored only as hashes, optionally restricted by source IP) and toggle individual MCP tools off if you never want specific data exposed. Passwords, private keys, and VPN profiles are never returned.

## 3. SNMP Monitoring (`System → Settings → SNMP`, `v0.19.0`)

Monitor Stratum FW from **Zabbix, LibreNMS, PRTG, Observium, or Cacti** using FreeBSD's native `bsnmpd` (`SNMP v1 / v2c`, strictly read-only):
* Exports 64-bit interface counters (`IF-MIB`), CPU/RAM/disk/process metrics (`HOST-RESOURCES-MIB`), and `pf` state/packet statistics—answering **only on selected internal interfaces** and allowed management IPs, never on WAN.
