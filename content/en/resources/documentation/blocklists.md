---
title: "Blocklists, Force DNS, Web Filter & Suricata IPS"
description: "Multi-layer threat defense in Stratum Firewall v0.30.3 — 63 IP blocklist feeds, Force DNS with Active Directory exceptions, Unbound RPZ DNSBL, 55-category Web Filter, Dual-Engine Suricata 8 IPS/IDS, and Unified Block Reports."
lead: "Stop threats across every layer — kernel pf IP feeds, unbypassable Force DNS with AD exceptions, Unbound RPZ DNS sinkholing, 55-category Web Filtering, inline Suricata 8 IPS, and pfBlockerNG-style unified reports."
---

Stratum Firewall combines four complementary defense engines—managed directly from the web UI with zero external subscriptions or license keys.

## 1. IP Blocklists (`Firewall → Blocklists → IP`, `v0.9.0`)

![IP Blocklists](/images/docs/blocklists.png)

* **63 Maintained Threat Feeds:** Derived from pfBlockerNG catalogs (*Spamhaus DROP, Abuse.ch Feodo Tracker, Emerging Threats, CINS Army, blocklist.de, PRI1–PRI5, Scanners, TOR, Proxy* and more) loaded straight into kernel `pf` tables.
* **Inbound &amp; Outbound Enforcement:** Drop inbound attacks silently while rejecting outbound botnet/C2 connections fast; custom allowlists always override blocked ranges, private/reserved CIDRs are protected from lockout, and feeds refresh hourly without requiring an Apply.

## 2. DNSBL, 55-Category Web Filter &amp; Force DNS (`v0.24.0`, `v0.29.2`)

* **DNS Blocklists (`Firewall → Blocklists → DNSBL`):** Sinkhole ads, trackers, phishing, and malware domains in **Unbound RPZ** using curated pfBlockerNG and **HaGeZi** catalogs plus custom allow/block lists.
* **Per-Network Web Filter (`Firewall → Web Filter`):** Assign distinct content policies per LAN, VLAN, or VPN network across **55 categories** (*UT1 Université Toulouse Capitole* &amp; *Block List Project*), enforce **Safe Search** on Google (including regional domains), Bing, DuckDuckGo, and YouTube Restricted Mode, and test domains with **Check a site**.
* **Force DNS Through the Firewall (`Services → DNS Resolver → Force DNS`, `v0.29.2`):** Ensure devices cannot bypass DNSBL or the Web Filter by hardcoding external resolvers (`8.8.8.8`, `1.1.1.1`):
  * All plain DNS (`UDP/TCP 53`) queries on chosen internal networks are transparently redirected to Stratum's resolver, and **DNS over TLS (`TCP 853`)** is blocked on those networks.
  * **Exception Alias:** Leave specific internal DNS servers—such as **Active Directory domain controllers**—directly reachable by client workstations without redirection.
  * **Privacy-First:** Works completely independently of *Web Activity*—**nothing is recorded** when Force DNS is enabled on its own.

## 3. Unified Blocklist Reports (`Firewall → Blocklists → Blocked`, `v0.28.2` – `v0.28.4`)

Inspect everything stopped by **DNSBL**, **Web Filter**, and **IP Blocklists** in a single pfBlockerNG-style analytics view (plus the **Blocklists** Dashboard widget):
* View hourly/daily block charts, feed attribution (showing `"No longer in a list"` if an upstream IP feed rotated after the block), top blocked domains/IPs, and top client devices across the last **1 hour, 24 hours, or 7 days** (fully compatible with Unbound 1.26 RPZ logs).

## 4. Per-Interface Suricata 8 IPS / IDS (`Services → Intrusion Detection`, `v0.15.0` – `v0.24.4`)

* **Per-Interface Watch or Block:** Set each interface independently to **Off**, **Watch** (`pcap` IDS alerts), or **Block** (inline `netmap` IPS packet dropping), supervising dedicated `netmap` and `pcap` Suricata processes in parallel when mixed.
* **Live Rule Reloads (`SIGUSR2`):** Enable ET Open threat categories, toggle categories to **Block**, or disable noisy individual SIDs live without restarting Suricata or interrupting traffic.
