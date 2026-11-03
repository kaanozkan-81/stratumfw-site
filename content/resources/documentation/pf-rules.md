---
title: "pf &amp; Firewall Rules"
lead: "Master the pf packet filter — learn how rules are evaluated, ordered, and applied safely."
---

Stratum Gateway uses **pf (Packet Filter)**, the industry-standard firewall engine built into FreeBSD. Unlike legacy firewalls that use complex, proprietary rule syntax, pf uses a clean, readable configuration language.

## How pf Evaluates Rules

The most critical concept to understand is that **pf evaluates rules from top to bottom, and the first matching rule wins.**

If a packet matches a rule, the action (pass, block, or reject) is taken immediately, and no further rules are evaluated. 

- **Pass:** Allows the packet through.
- **Block:** Drops the packet silently. The sender receives no response (good for security).
- **Reject:** Drops the packet and sends a TCP RST or ICMP unreachable back to the sender (good for debugging).

Stratum enforces a **Default Deny** policy. If a packet doesn't match any rule, it is blocked.

## Anatomy of a Rule

When you create a rule in the Stratum panel (under **Firewall → Rules**), you are defining the following parameters:

- **Interface:** Which interface does this traffic enter/leave? (e.g., WAN, LAN).
- **Direction:** `In` (traffic entering the firewall) or `Out` (traffic leaving the firewall).
- **Action:** Pass, Block, or Reject.
- **Protocol:** TCP, UDP, ICMP, or any.
- **Source / Destination:** IP addresses, networks, or Aliases.
- **Port:** The destination port (e.g., 80 for HTTP, 22 for SSH).

## Aliases and Macros

To keep your ruleset clean, avoid writing raw IP addresses or ports. Instead, use **Aliases** (under **Firewall → Aliases**).

- **Host Alias:** A list of single IP addresses (e.g., `Web_Servers`).
- **Network Alias:** A list of subnets (e.g., `Internal_Subnets`).
- **Port Alias:** A list of ports (e.g., `Web_Ports` = 80, 443).

When you update an Alias, `pf` automatically updates all rules that reference it, without requiring a full firewall reload.

## The Safe-Apply Workflow in Practice

Writing firewall rules carries a risk: you might accidentally block your own management access. Stratum solves this with **Safe-Apply**.

When you click **Apply** in the rule editor:
1. The system runs a syntax check using `pfctl -nf`.
2. It applies the new ruleset to the kernel.
3. A **confirmation banner** appears at the top of the screen with a 60-second countdown.
4. You must click **"Confirm"** to keep the changes. If you lose connectivity, the system automatically rolls back to the previous ruleset after the timeout.

## Best Practices

1. **Order Matters:** Place more specific rules above general rules.
2. **Use Anti-Lockout Rules:** Stratum automatically adds an anti-lockout rule for the LAN interface by default. Do not delete it unless you know exactly what you are doing.
3. **Log Everything:** Enable logging on block rules to see what traffic is being dropped in the **Firewall Log**.
4. **Test with Diagnostics:** Before applying a strict rule, use the **Diagnostics → Packet Capture** tool to confirm the traffic patterns.

## Next Steps

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>Set up WireGuard VPN</h3>
    <p>Allow secure remote access to your protected network.</p>
    <a href="/resources/documentation/wireguard-vpn/" class="card-link">WireGuard Guide →</a>
  </div>
  <div class="card">
    <h3>Enable Threat Intel</h3>
    <p>Automatically block malicious IPs using pf tables.</p>
    <a href="/resources/documentation/blocklists/" class="card-link">Blocklists Guide →</a>
  </div>
</div>
