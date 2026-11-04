---
title: "Beyond the Blinking Lights: True Network Visibility with Stratum FW"
date: 2026-11-04
author: "Stratum Security Team"
description: "Traditional firewalls just blink when something goes wrong. With the new v0.13.1 dashboard update, Stratum FW gives you the context you actually need."
tags: ["dashboard", "monitoring", "wan", "vpn", "update"]
---

If you've ever managed a traditional enterprise firewall, you know the feeling. You walk into the server room, and there's a wall of blinking LEDs. One of them is blinking green, another is blinking amber. But you have no idea *why*.

Is the WAN link down? Is it just experiencing high packet loss? Is the VPN tunnel up, but nobody is actually connected to it? 

We built **Stratum FW** because we believe that network security shouldn't be a guessing game. And with our latest updates (**v0.13.0** and **v0.13.1**), we are introducing a completely redesigned dashboard that replaces blinking lights with actionable intelligence.

## The New Dashboard (v0.13.0)

When you log into Stratum FW now, you aren't just greeted with CPU and memory usage. The new dashboard is a mission control for your network.

- **Status Strip:** Instantly see your hostname, version, update state, and every WAN connection with its live download/upload rates.
- **Key Figures:** CPU, memory, disk, and temperature — with 5-minute curves and colors that change when you hit warning levels.
- **Protection Metrics:** See exactly how many packets were blocked by your IP blocklists in the last hour, and which rules are doing the heavy lifting.
- **Top Blocked Sources:** A live list of the IPs and ports that are trying to hit your network the most.

## The v0.13.1 Update: WAN Health & VPN Context

The v0.13.0 dashboard was a huge leap. But we immediately followed it up with **v0.13.1**, which adds the deep context that sysadmins crave.

### WAN Health Panel
Is your ISP actually providing the speed you pay for? The new WAN panel measures **latency and packet loss to your gateway every 30 seconds**, complete with a live curve. You can also see how long the connection has been up, today's total download/upload, and the live rate. 

### VPN Visibility
Setting up WireGuard or OpenVPN is one thing; knowing who is actually using it is another. The dashboard now shows every connected OpenVPN user and active WireGuard peer:
- The public IP address they are connecting from.
- Their assigned VPN address.
- When they connected (or their last handshake).
- Total data received and sent.

### Automatic Update Checks
We also added a quality-of-life feature: simply opening the dashboard now checks for software updates in the background (at most every 5 minutes), just like pfSense. You'll see a badge if a new release is available.


## The Elephant in the Server Room: Resource Efficiency

We need to talk about the hidden cost of legacy firewalls. 

I recently installed Stratum FW on a machine with an **Intel Atom C2558** (a modest 4-thread CPU from 2014). Under Stratum FW, this hardware idles at **1% CPU** and **13% RAM**, while running IP blocklists, DNS filtering, and WireGuard.

Before this, the exact same hardware was running pfSense. Even without advanced features enabled, pfSense would frequently max out the CPU, crash, and generate panic reports just trying to do basic firewall tasks. 

Why is there such a massive gap in resource consumption?

**1. The PHP/XML Tax vs. FastAPI/JSON**
pfSense uses a legacy architecture based on PHP and a massive `config.xml` file. When you change a setting or update a blocklist, it rewrites the entire XML, parses it, and triggers a full `filter reload` of the kernel. On a weak CPU, this is devastating.
Stratum FW uses Python/FastAPI and a clean `config.json`. It only processes the changes and applies them natively.

**2. Blocklist Updates Done Right**
pfBlockerNG (pfSense) uses Python scripts to parse lists, writes them to SQLite, and reloads the firewall. 
Stratum FW takes a much lighter approach: it downloads the lists and atomically replaces the `pf` kernel table using `pfctl -T replace`. No firewall reload, no XML parsing, zero CPU spikes. That's why you can see 225,000+ blocked domains on the dashboard without the system breaking a sweat.

**3. No Background Bloat**
Stratum runs only what it needs. No unnecessary daemons, no constant config-checking loops. 

**Will it get heavier?**
Yes, adding deep packet inspection (like Suricata) will always consume CPU on any platform. But because of our declarative architecture, Stratum FW will remain a fraction of the weight of traditional firewalls.

## Under the Hood: Fixing the FreeBSD 15 Log Bug
This update wasn't just about new features. We also fixed a critical bug in **v0.13.1**. On FreeBSD 15, `pf` changed the way it writes rule numbers in its logs (e.g., "3..1/0"). This caused our firewall logs to show the wrong rule names. We completely rewrote our log parser to handle this. The Firewall Log, the dashboard, and remote syslog now show the exact right rule again.

## Ready to see it for yourself?
We are building the firewall we always wanted. The one that tells us exactly what is happening on the wire, without hiding behind proprietary CLIs or cloud dashboards.

Download the latest v0.13.1 ISO and spin it up in a VM. Or simply explore the architecture on our website.

---

* [Download Stratum FW](/download/)
* [Read the Architecture](/product/architecture/)
