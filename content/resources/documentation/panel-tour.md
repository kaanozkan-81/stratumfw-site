---
title: "Panel Tour"
lead: "Navigate the Stratum interface — understand the layout, modules, and the Safe-Apply workflow."
---

The Stratum management panel is designed to be fast, responsive, and free of the clutter found in legacy firewall UIs. It is a thin client built on top of a REST API, meaning every action you take is powered by the same API you can use for automation.

## The Layout

The interface is divided into three main sections:

1. **Sidebar Navigation:** Grouped by function (Overview, Interfaces, Firewall, Services, VPN, System, Diagnostics).
2. **Top Bar:** Shows the current breadcrumb, your active user role, and system status indicators.
3. **Main Content Area:** Where the active module's configuration or dashboard lives.

## The Dashboard

The default view when you log in. The dashboard is highly customizable and provides an at-a-glance view of your network's health.

- **System Vitals:** CPU, RAM, and Disk usage of the FreeBSD host.
- **Interface Traffic:** Real-time RX/TX graphs for WAN and LAN.
- **Active States:** The number of active connections currently tracked by `pf`.
- **Security Alerts:** Recent blocks from IP Blocklists or DNSBL.

## Role-Based Views

What you see depends on your role:
- **Admins** see every menu item.
- **Operators** see diagnostic tools, states, and logs, but configuration pages (like firewall rules) are read-only or hidden.
- **Read-Only** users see only the dashboard, metrics, and logs.

## The Safe-Apply Indicator

At the top of the screen, you will occasionally see a green or yellow bar appear. This is the **Apply Bar**. 

Because Stratum uses a declarative configuration engine, changes are staged in memory until you click "Apply". This allows you to make multiple adjustments and commit them all at once. When you click Apply, the Safe-Apply workflow validates the rules, applies them to `pf`, and gives you a timeout window to confirm connectivity.

## Key Modules

- **Interfaces:** Assign physical NICs to WAN, LAN, or OPT roles.
- **Firewall / Rules:** Manage the `pf` ruleset. Drag-and-drop ordering.
- **VPN / WireGuard:** Generate keys, manage peers, and scan QR codes.
- **Security / Blocklists:** Manage threat feeds and DNSBL categories.
- **Diagnostics:** Live `pf` states, packet captures, and system logs.

## Next Steps

Now that you understand the layout, it's time to configure your firewall.

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>Initial Configuration</h3>
    <p>Set up WAN/LAN interfaces and administrator access.</p>
    <a href="/resources/documentation/initial-configuration/" class="card-link">Read guide →</a>
  </div>
  <div class="card">
    <h3>pf &amp; Firewall Rules</h3>
    <p>Learn how to write, order, and apply firewall rules safely.</p>
    <a href="/resources/documentation/pf-rules/" class="card-link">Read guide →</a>
  </div>
</div>
