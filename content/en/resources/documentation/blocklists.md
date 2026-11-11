---
title: "Blocklists, Web Filter & Suricata IPS"
description: "Multi-layer threat defense in Stratum Firewall — 63 IP blocklist feeds, Unbound RPZ DNSBL, 55-category Web Filter, Dual-Engine Suricata 8 IPS/IDS, and Unified Block Reports."
lead: "Stop threats across every layer — kernel pf IP feeds, Unbound RPZ DNS sinkholing, 55-category Web Filtering, inline Suricata 8 IPS, and pfBlockerNG-style unified reports."
---

Stratum Firewall combines four complementary defense engines—managed directly from the web UI with zero external subscriptions or license keys.

## 1. IP Blocklists (`Firewall → Blocklists → IP`, `v0.9.0`)

![IP Blocklists](/images/docs/blocklists.png)

* **63 Maintained Threat Feeds:** Derived from pfBlockerNG catalogs (*Spamhaus DROP, Abuse.ch Feodo Tracker, Emerging Threats, PRI1–PRI5, Scanners, TOR, Proxy* and more) loaded straight into kernel `pf` tables.
* **Inbound &amp; Outbound Enforcement:** Drop inbound attacks silently while rejecting outbound botnet/C2 connections fast; custom allowlists always override blocked ranges, and private/reserved CIDRs are protected from accidental lockout.
* **Live Updates:** Feeds refresh on a configurable schedule (hourly by default) without requiring a firewall Apply.

## 2. DNSBL &amp; 55-Category Web Filter (`v0.6.13`, `v0.24.0`)

* **DNS Blocklists (`Firewall → Blocklists → DNSBL`):** Sinkhole ads, trackers, phishing, and malware domains in **Unbound RPZ** using curated pfBlockerNG and **HaGeZi** catalogs plus custom allow/block lists.
* **Per-Network Web Filter (`Firewall → Web Filter`):** Assign different content policies per LAN, VLAN, or VPN network across **55 categories** (*UT1 Université Toulouse Capitole* &amp; *Block List Project*—adult content, malware, proxies/VPN bypass, social media, gambling, etc.), enforce **Safe Search** on Google (including regional domains), Bing, DuckDuckGo, and YouTube Restricted Mode, and test domains with **Check a site**.

## 3. Unified Blocklist Reports (`Firewall → Blocklists → Blocked`, `v0.28.2` – `v0.29.0`)

Inspect everything stopped by **DNSBL**, **Web Filter**, and **IP Blocklists** in a single pfBlockerNG-style analytics view (plus the **Blocklists** Dashboard widget):
* View hourly/daily block charts, which feed matched each event, top blocked domains/IPs, and client devices with the most blocks across the last **1 hour, 24 hours, or 7 days** (fully compatible with Unbound 1.26 RPZ logs).

## 4. Per-Interface Suricata 8 IPS / IDS (`Services → Intrusion Detection`, `v0.15.0` – `v0.24.4`)

* **Per-Interface Watch or Block:** Set each interface independently to **Off**, **Watch** (`pcap` IDS alerts), or **Block** (inline `netmap` IPS packet dropping). When Watch and Block are mixed across interfaces, Stratum automatically supervises dedicated `netmap` and `pcap` Suricata processes in parallel.
* **Live Rule Reloads (`SIGUSR2`):** Enable ET Open threat categories, toggle categories to **Block**, or disable noisy individual SIDs with one click—changes reload live without restarting Suricata or interrupting traffic.
