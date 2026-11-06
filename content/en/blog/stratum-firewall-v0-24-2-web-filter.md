---
title: "Stratum Firewall v0.24.2: Next-Gen Web Filtering and IPS Stability"
date: 2026-11-06
description: "Discover the latest features in Stratum Firewall v0.24.2, including the new DNS-based Web Filter (Unbound RPZ) with 55 categories, Safe Search, and critical IPS blocking mode fixes."
tags: ["Web Filter", "IPS", "Suricata", "Firewall", "Stratum FW"]
categories: ["Security", "Updates"]
author: "Stratum Security"
---

We are thrilled to announce the release of **Stratum Firewall v0.24.2**! This update is packed with powerful new capabilities, headlined by the introduction of a full-featured **Web Filter** and crucial fixes for our Intrusion Prevention System (IPS).

Here is everything you need to know about the latest updates.

## 1. Introducing Web Filter (Firewall > Web Filter)
Stratum Firewall now includes a highly granular, DNS-based Web Filter powered by Unbound RPZ. It gives you full control over what users can access on your network without the need for client-side agents.

*   **Granular Policies:** Assign different policies to different networks (e.g., strict filtering for Guest Wi-Fi, relaxed for Staff LAN). VPN clients can also be assigned policies.
*   **55 Categories:** From adult content and security threats to social networks and shopping, sourced from UT1 blacklists and the Block List Project (updated daily).
*   **Safe Search Enforcement:** Force safe results on Google, Bing, DuckDuckGo, and restricted mode on YouTube.
*   **Exceptions & Diagnostics:** Easily whitelist specific domains, and use the "Check a site" tool to see exactly which category it falls into.
*   **Detailed Reporting:** See exactly which categories, sites, and devices are being blocked over the last 7 or 30 days.

## 2. Intrusion Detection (IPS) Stability Fixes
With the release of inline blocking (IPS) in v0.21.0, we noted a critical issue in **v0.24.2**: Suricata was dropping connections it had not seen start (e.g., long-lived push channels on phones), causing devices to disconnect from Wi-Fi.

*   **Midstream Pickup:** Suricata now correctly picks up connections that were open before it started or that sat idle past the flow timeout.
*   **Fail-Open on Exceptions:** We implemented `exception-policy: ignore`. Now, if Suricata encounters an exception, it allows the traffic to pass instead of dropping the flow entirely. Only explicit blocking rules will drop packets, ensuring your network stays up even under heavy inspection.

## 3. Other Fixes (v0.24.1)
*   **Web Filter Lists:** Fixed an issue where category lists were not downloaded after the first Apply. They now download immediately based on the saved configuration.

## Conclusion
Stratum Firewall v0.24.2 is a massive leap forward in both network privacy and enterprise-grade security. 

[Download the latest version today](/download/) and review the full technical details in our [Changelog](/resources/changelog/).
