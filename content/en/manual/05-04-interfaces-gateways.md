---
title: "Interfaces > Gateways"
seo_title: "05.4 Interfaces > Gateways — Stratum FW User Manual"
description: "Gateways and multi-WAN: monitoring every internet connection, WAN failover with priorities and failback, replies on the right WAN, and sending chosen traffic through a chosen WAN (policy routing)."
weight: 404
chapter_num: "05.4"
url: "/manual/interfaces-gateways/"
aliases:
  - "/manual/05-04-interfaces-gateways/"
translationKey: "manual-05-04-interfaces-gateways"
---

Every WAN-type interface has a **gateway** - the router of the provider behind it. On
**Interfaces > Gateways** the firewall watches each of them, and with more than one internet connection it can
move the default route to another line when one fails.

What Stratum FW does with several WANs:

| Feature | What it does | Where |
| :--- | :--- | :--- |
| **Gateway monitoring** | Measures latency and packet loss of every gateway, every second. | This page |
| **WAN failover** | Moves the default route to the next working gateway when the active one fails, and back when it recovers. | This page |
| **Replies on the right WAN** | Answers to connections that came in on a WAN leave through the same WAN - always, with no setting. | Automatic |
| **Policy routing** | Sends the traffic of a chosen rule through a chosen WAN - for example the guest network over the second line. | **Firewall > Rules** |

Stratum FW does not spread the connections of one network over several lines at once (load balancing): each
rule's traffic uses one line at a time.

## The gateway list

![Interfaces > Gateways](/images/manual/gw-list.png)

| Column | Meaning |
| :--- | :--- |
| **Name** | Made from the interface's description: *Fibre* gives `Fibre_GW`, an interface without one `WAN_GW`. |
| **Interface** | The WAN-type interface. |
| **Gateway** | The provider's router: the one you entered for a static address, the one received by DHCP, or the PPPoE session. |
| **Monitor** | The address that is pinged: the gateway itself, or the monitor address you set. |
| **Priority** | The order for failover: lower is preferred. |
| **Status** | **online**, **warning**, **down** or **pending** (still measuring), with the reason when it is not online - for example *no link*, *packet loss 35 %*, *latency 620 ms*. |
| **Latency**, **Loss** | The averages of the last 30 seconds. |
| **Last hour** | A small latency graph of the last hour. |

## How a gateway is judged

Each gateway gets one ping per second, always through its own line - so a backup line is measured even while it
does not carry the default route. The decision is based on the last 30 seconds:

| Status | When |
| :--- | :--- |
| **down** | The interface has no link, no address or no gateway; or the packet loss reaches the **Down at packet loss** limit (default 20 %); or the average latency reaches **Down at latency** (default 500 ms). |
| **warning** | Half of either limit is reached (10 % loss or 250 ms with the defaults). Nothing is switched. |
| **pending** | Just started or the address just changed: fewer than 10 answers expected yet, none received. |
| **online** | Everything else. |

A gateway that goes down or comes back is reported in the notifications (and by Telegram or e-mail when they are
set up).

## Gateway settings

Click a gateway to change it.

![Editing a gateway](/images/manual/gw-edit.png)

| Setting | Meaning |
| :--- | :--- |
| **Description** | A note for the gateway. |
| **Monitor address** | The address to ping. Empty: the gateway itself. Many providers' routers do not answer ping; then use a public address such as `1.1.1.1` or `8.8.8.8`. Each gateway needs its own monitor address (the firewall routes each one through its own line), and it cannot be inside one of your internal networks. |
| **Priority** | 1 to 255, lower is preferred. With WAN failover on, the best gateway that is up carries the default route. By default the gateways are numbered 10, 20, 30 ... in the order of the interfaces, so the main WAN is preferred. |
| **Mark the gateway down by packet loss and latency** | On by default. Off: only a missing link or address marks it down - for a line whose quality you do not want to judge, or one that legitimately has high latency (satellite, some mobile lines). |
| **Down at packet loss (%)** | 1 to 100. Default 20. |
| **Down at latency (ms)** | 0 to 10000. Default 500. 0: latency never marks the gateway down. |

> **Tip:** Monitor something beyond the provider's router. A router that answers ping only proves the cable to it
> works; `1.1.1.1` through that line proves the internet is reachable. Use a different public address for each
> line.

## WAN failover

The **WAN failover** card below the list (shown when there are at least two WAN-type interfaces):

| Setting | Meaning |
| :--- | :--- |
| **Switch the default route to the next gateway (by priority) when the active one is down** | Switches failover on. Off by default: the default route stays on the main WAN whatever happens. |
| **Return to a preferred gateway after (s)** | A gateway with a better priority must be online this long (default 60 seconds, 0 to 3600) before the default route returns to it, so a flapping line does not switch back and forth. |
| **Close the connections of the gateway that went down** | On by default. Open connections through the failed line are closed, so devices reconnect at once through the new one instead of waiting for their own timeouts. |

These settings take effect after **Apply changes**.

**What happens on a failure:**

1. The active gateway is judged **down**.
2. The default route moves to the gateway with the best priority that is up. The change is logged and notified.
3. Connections through the failed line are closed (if set); devices reconnect through the new line.
4. When the preferred gateway has been online for the failback time, the default route returns to it.

The **Gateways** panel on the dashboard shows which gateway carries the default route and since when.

> **Note:** Failover moves the firewall's *default route*. Traffic that a rule sends through a specific WAN
> (policy routing, below) follows that rule's own setting for a down gateway.

## Replies leave through the WAN they came in on

With more than one WAN, every connection that comes in on a WAN - a port forward, a VPN, the panel - is answered
through the same WAN, whichever line carries the default route. Without this, the reply would leave through the
other provider, which drops it. Stratum FW does this automatically for static, DHCP and PPPoE WANs; there is
nothing to set.

## Policy routing: chosen traffic through a chosen WAN

To send some traffic through a particular line - the guest network over the second provider, a server's
backups over the cheaper line - set a **Gateway** on its rule:

1. **Firewall > Rules**, the tab of the internal interface (or of a group whose members are all internal).
2. Edit the *pass* rule that allows the traffic (or add one).
3. **Gateway**: choose the WAN. **Default (routing table)** means normal routing.
4. **If the gateway is down**:
   - **Use the default route** - the usual choice: the traffic takes the normal route (failover included) while
     that line is down, and returns to it when it is back.
   - **Keep using this gateway (traffic fails)** - for traffic that must never leave through another line, for
     example because the other provider must not see it.
5. Save and **Apply changes**.

Traffic of such a rule to your own networks - the internal and VPN networks and the firewall itself - is not
sent to the WAN: the firewall keeps it local automatically, so a rule with destination *any* still lets the
guests reach the firewall's DNS.

When a gateway goes down or comes back, the rules are reloaded on their own within seconds; connections that are
open stay as they are.

## Several WANs and DHCP

The main WAN's DHCP lease sets the default route and the DNS servers as usual. An additional WAN that gets its
address by DHCP never takes over the default route or the DNS servers by itself: the gateway of its lease is used
for failover, for replies and for policy routing, and follows the lease when it changes.

