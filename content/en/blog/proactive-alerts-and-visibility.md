---
title: "Beyond Basic Blocking: Proactive Alerts and Deep Visibility in v0.13.2"
date: 2026-11-04
author: "Stratum Security Team"
description: "A firewall shouldn't just block packets; it should tell you what's happening. Discover the new WAN health, VPN visibility, and proactive alert features in the latest Stratum FW update."
tags: ["dashboard", "alerts", "monitoring", "vpn", "dhcp"]
---

For decades, traditional firewalls have been reactive. They sit quietly at the edge of your network, blocking bad traffic, and only making their presence known when the internet goes down or a rule breaks something.

If your ISP was providing terrible latency, or if a WireGuard peer was silently disconnected for days, you wouldn't know until a user complained.

With the latest updates (**v0.13.1** and **v0.13.2**), we are changing that. We are turning the Stratum FW dashboard into a proactive network command center.

## 1. WAN Health: Is Your ISP Actually Delivering?
You pay for a specific internet speed and reliability. But how do you know if your ISP is holding up their end of the bargain?

The new **WAN Panel** in v0.13.1 measures **latency and packet loss to your gateway every 30 seconds**, visualized with a live curve. You can also see exactly how long the connection has been up, today's total download/upload, and the live rate. If your primary fiber line starts degrading with 10% packet loss, you will see it instantly on the dashboard.

## 2. VPN Visibility: Who Is Actually Connected?
Setting up WireGuard and OpenVPN is easy. But managing them when you have dozens of remote workers is a different story.

The updated dashboard now lists **every connected OpenVPN user and active WireGuard peer**. You can instantly see:
- The public IP they are connecting from.
- Their assigned internal VPN address.
- When they connected (or their last handshake).
- Total data received and sent.

No more guessing if a user is connected, or if a compromised device is silently downloading data in the background.

## 3. Proactive Alerts: Tell Me Before It Breaks (v0.13.2)
The most important feature of v0.13.2 is the new **Dashboard Alerts** system.

You can now configure Stratum FW to send alerts via **Telegram or E-mail** (SMTP) for critical events:
- WAN connection down or up again (and new WAN IP addresses).
- Blocked sign-ins (brute-force attempts).
- Failed applies or automatic rollbacks.
- Disk almost full, CPU temperature high.
- IP/DNS blocklist download failures.

Events are recorded in a list on the dashboard (the last 100 are kept), showing the time and whether they were sent via e-mail or Telegram. If the internet goes down, Stratum will try to alert you the moment the backup WAN comes online.

## 4. DHCP & IP Management
Also in v0.13.2, the dashboard now includes a DHCP panel. You can monitor:
- Active leases per network.
- How much of the address pool is in use (turns amber at 75% and red at 90%).
- The number of static mappings.

This prevents the classic "I can't get an IP address" panic when a busy office suddenly runs out of DHCP leases.

## The Philosophy: Lean, Mean, and Always Watching
You don't need a massive cloud subscription or an enterprise support contract to get deep, actionable network visibility. Stratum FW remains lightweight (running on a 10-year-old Intel Atom!), but it now watches your network with the eyes of an enterprise NGFW.

Ready to see it in action? Download the latest ISO or spin it up in a VM.

---

* [Download Stratum FW](/download/)
* [Read the Architecture](/product/architecture/)
