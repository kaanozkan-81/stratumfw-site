---
title: "Stratum Firewall v0.39.5: Real-Time Web Filter Keywords — Catching Rotating & Numbered Domains on First DNS Query"
date: 2026-11-24
author: "Stratum Security"
translationKey: "v0-39-5-web-filter-keywords"
description: "Stratum Firewall v0.39.5 adds Firewall > Web Filter > Keywords to catch numbered and rotating betting, casino, and copycat domains (site1234.com -> site1235.com) on the first Unbound DNS lookup — with List-only one-click triage, instant auto-blocking via Your own list, and zero browsing logs."
lead: "Stop domain-hopping betting, casino, and phishing clones in seconds instead of waiting 24 hours for external blocklists to catch up."
images: ["/images/web-filter-keywords-hero.jpg"]
tags: ["firewall", "stratum firewall", "freebsd 15.1", "web filter", "dns", "unbound", "keywords", "v0.39.5"]
---

![Stratum Firewall v0.39.5 Real-Time Web Filter Domain Keyword Interception](/images/web-filter-keywords-hero.jpg)

Daily category blocklists handle millions of known domains effortlessly — until an operator deals with **numbered, domain-hopping sites** (`site1234.com`, `site1235.com`, `site1236.com` ...) that rotate to a freshly registered domain every few hours specifically to stay ahead of static feeds.

With **Stratum Firewall v0.39.5** on [Stratum Firewall](https://stratumfw.com/), **`Firewall > Web Filter > Keywords`** closes that window completely: the moment any client queries Unbound DNS for a new domain containing a watched brand or word (such as *bahis* or *casino*), Stratum catches it in real time. Full documentation is live in [Chapter 06.6 — Firewall > Web Filter](/manual/firewall-web-filter/) and the [Official Changelog](/resources/changelog/).

![Stratum Firewall v0.39.5 Web Filter Keywords Architecture](/images/web-filter-keywords-architecture.svg)

### 1. Audit First (`List them only`) or Enforce in Seconds (`Block them at once`)

![Firewall > Web Filter > Keywords Panel](/images/manual/wf-keywords.png)

Administrators can tune keyword enforcement to match their confidence in each word:

- **`List them only` (Default — Safe Triage):** Newly seen domains matching a keyword appear in the **Keywords** table with the matched word, timestamp, and first querying device IP. With **one click**, you can **Block** the domain (adding it to **Your own list**), **Ignore** it, or promote it to an **Exception**.
- **`Block them at once` (Zero-Touch Blocking):** Matching domains are added straight into **Your own list** and blocked within seconds across every Web Filter policy that enables that category — displaying your customizable block page wherever configured.

### 2. Surgical Domain-Label Matching & Zero Browsing Logs

| Design Safeguard | How `v0.39.5` Prevents False Positives & Protects Privacy |
| :--- | :--- |
| **Registered Name Only** | Matches **only** the domain's own name — subdomains (`casino.news-portal.example`) and TLDs (`.com`, `.bet`) are ignored |
| **Exceptions Always Win** | Domains listed under **Web Filter > Exceptions** (and their subdomains) are never blocked, even if a keyword matches |
| **Works Without Web Activity** | When [Web Activity](/manual/diagnostics-web-activity/) is off, the collector inspects DNS queries for keywords only and **records nothing** to disk |
| **Policy-Scoped Enforcement** | Auto-blocked sites enter **Your own list**, so guest or student VLAN policies enforce them immediately while unrestricted server VLANs stay untouched |

### Upgrade to `v0.39.5`

Upgrade any Stratum Firewall appliance in one click via **`System > Update`** (`stratum-update run`) with automatic OpenZFS Boot Environment rollback safety, explore [Chapter 06.6 in the User Manual](/manual/firewall-web-filter/), download the new `v0.39.5` installer images (`.iso` & `.img`) on [Download](/download/), or test the panel in the [Live Demo](https://demo.stratumfw.com).
