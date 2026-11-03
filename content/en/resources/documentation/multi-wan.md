---
title: "Multi-WAN"
description: "Configure failover and load balancing across multiple internet links in Stratum Gateway — gateway definitions, health checks, policy routing, and worked examples."
weight: 6
---

A single internet link is a single point of failure. Multi-WAN lets Stratum Gateway use two or more upstream connections, either for redundancy (one link backs up another) or for capacity (traffic is shared across links). This guide covers both modes, from interface roles through health monitoring and policy routing.

> **Before you start:** Each WAN link must already have an interface assigned to the WAN role. See [Network Basics](/resources/documentation/network-basics/) for interface configuration and role assignment.

## When Multi-WAN Makes Sense

Multi-WAN is the right tool when:

- **Availability matters.** A failed link should not take the business offline.
- **Different links have different strengths.** A fibre link for latency-sensitive traffic and a cheaper cable link for bulk transfers.
- **You want to survive an ISP outage.** Two links from different providers is the classic setup.

Multi-WAN is **not** a bandwidth doubler. A single TCP connection cannot be split across two links, so a single file transfer will not go faster just because you have two WANs. You get aggregate capacity for many concurrent connections, not for any one session.

## Gateway Definitions

Before Multi-WAN can do anything, each WAN interface needs a **gateway** — the next-hop IP address that traffic should be sent to for that link.

**Interfaces → WAN → Edit**

    Interface:      WAN1 (eth0)
    Role:           WAN
    IPv4 config:    DHCP
    Gateway name:   WAN1_GW

    Interface:      WAN2 (eth1)
    Role:           WAN
    IPv4 config:    Static
    IP address:     203.0.113.2/24
    Gateway:        203.0.113.1
    Gateway name:   WAN2_GW

A gateway is a named object, separate from the interface. The name (`WAN1_GW`, `WAN2_GW`) is what you reference in Multi-WAN policies. If you change the underlying interface, you do not need to rewrite the policies — you just repoint the gateway.

### Gateway Groups

A **gateway group** combines two or more gateways into a single logical target. Policies can reference the group instead of an individual gateway, which is what enables failover and load balancing.

**Multi-WAN → Gateway Groups → New**

    Name:         PRIMARY_GROUP
    Members:      WAN1_GW (tier 1)
                  WAN2_GW (tier 2)
    Trigger:      packet loss > 20% OR latency > 500ms
    Protocol:     ICMP + HTTP

The **tier** is the failover priority. Tier 1 members are used first. If all tier 1 members fail their health check, tier 2 takes over. Multiple members in the same tier are used together (load balanced).

The **trigger** defines what "failed" means. Both ICMP and HTTP checks are run against each gateway; if either exceeds the thresholds, the gateway is marked down.

## Failover Mode

Failover is the simplest Multi-WAN configuration and the one most deployments actually need.

**Policy → Routing → New**

    Name:         default-route
    Source:       any
    Destination:  any
    Gateway:      PRIMARY_GROUP
    Mode:         failover

With this single policy, all traffic uses `WAN1_GW` until it fails, at which point it switches to `WAN2_GW`. When `WAN1_GW` recovers, traffic returns to it after a short stabilization period.

### Health Check Behavior

The health check runs continuously on each gateway, every **10 seconds** by default. A gateway is marked down after **3 consecutive failures** and up after **3 consecutive successes**. These values are configurable per gateway and are the main knobs for tuning failover responsiveness:

- **Faster failover** → shorter interval, fewer failures required. More false positives.
- **Slower failover** → longer interval, more failures required. More stable, but slower to react.

For most deployments, the defaults are fine. Tune them only after you have observed how the primary link actually fails in practice.

### Session Handling

Failover is **stateful for the current session only**. When a link goes down:

- **New connections** use the backup link immediately.
- **Existing connections** on the failed link are dropped and must be re-established by the client.

There is no way around this — the remote endpoints of those connections do not know your primary link failed and cannot be told to migrate. Users will see a brief interruption (a reload, a reconnect) when a link fails, then normal operation on the backup link.


## Load Balancing Mode

Load balancing uses multiple WAN links at the same time to increase aggregate throughput. Configure it exactly like failover, but set the mode to **loadbalance**:

**Policy → Routing → New**

    Name:         default-route
    Source:       any
    Destination:  any
    Gateway:      PRIMARY_GROUP
    Mode:         loadbalance

All gateways in tier 1 are used together. If one fails, its share of traffic is redistributed across the survivors, and it rejoins the pool when it recovers.

### Distribution Method

Two algorithms are available:

| Method | Behavior |
|--------|----------|
| **Round-robin** | New connections are sent to each healthy gateway in turn |
| **Weighted** | Each gateway gets a configured share of new connections |

Round-robin gives an even split. Weighted lets you favour one link — useful when links have different capacities or costs. A 100 Mbps fibre link and a 20 Mbps LTE backup should not get equal shares; with weighting you can send 5× more traffic through the fibre link.

### Per-Gateway Weighting

**Multi-WAN → Gateway Groups → PRIMARY_GROUP → Edit**

    WAN1_GW (eth0):  weight 5
    WAN2_GW (eth1):  weight 1

This sends roughly 5/6 of new connections to `WAN1_GW` and 1/6 to `WAN2_GW`. Existing connections are not migrated when weights change — only new connections pick up the new distribution.

### What Load Balancing Does Not Do

Two limits are worth stating explicitly because they cause most load-balancing disappointments:

1. **A single session sticks to one link.** Load balancing distributes *sessions*, not packets. A large download over one TCP connection uses one WAN. Aggregate capacity only helps when there are many concurrent sessions (browsing, many users, parallel transfers).
2. **The remote server sees different source IPs.** If a client opens two sessions and they exit through different WANs, the remote server sees two different source IPs. This breaks anything that depends on session stickiness on the server side (banking portals, some SaaS login flows). **Session persistence** in the policy fixes this — it pins a given source IP to a single gateway for the duration of a configured timeout.

### Session Persistence

Enable persistence when a service is sensitive to the client's source address changing mid-session:

    Persistence:   source address
    Timeout:       300 seconds

With this enabled, a client at `192.168.1.50` will keep using the same WAN for 5 minutes, even if the load-balancer would otherwise have sent the next session to a different gateway.

If users report "random logouts" from a web service after enabling load balancing, session persistence is almost always the fix.

## Policy Routing

A routing policy overrides the default route for a specific class of traffic. Policies are matched top-down like firewall rules; the first matching policy wins.

**Policy → Routing → New**

    Name:         voip-via-fibre
    Source:       voip-subnet   (alias: 10.0.20.0/24)
    Destination:  any
    Service:      SIP, RTP
    Gateway:      WAN1_GW
    Mode:         failover

This forces all SIP and RTP traffic from the VoIP subnet through the fibre link, regardless of what the default route is doing. When `WAN1_GW` fails, the policy's failover kicks in and the same traffic moves to `WAN2_GW`.

### Common Policy Routing Scenarios

- **VoIP through the low-latency link** — SIP and RTP are sensitive to jitter, so pin them to the best upstream
- **Backups through the cheap link** — overnight backup traffic goes out the metered link to avoid consuming expensive bandwidth
- **Guest Wi-Fi through the LTE backup** — isolate guest traffic onto a link that does not affect business traffic when it is saturated
- **Specific cloud services through a specific ISP** — some SaaS providers have better peering with one upstream than another

### Policy Order Matters

Policies are evaluated top-down. A broad policy near the top will shadow every specific policy below it. The standard layout:

    #   Name                    Source        Gateway         Mode
    1   voip-via-fibre          voip-subnet   WAN1_GW         failover
    2   backup-via-cable        backup-srv    WAN2_GW         failover
    3   guest-via-lte           guest-subnet  LTE_GW          failover
    4   default-route           any           PRIMARY_GROUP   failover

Rules 1–3 are specific overrides. Rule 4 is the catch-all. If the catch-all were placed first, none of the specific policies would ever fire.


## Monitoring

Multi-WAN is only as good as its visibility. The panel exposes three views that together tell you whether your configuration is doing what you intended.

### Status Overview

**Multi-WAN → Status** shows, for each gateway:

    Gateway         Status    Latency   Loss    Since
    WAN1_GW         up        12 ms     0.0%    3d 4h
    WAN2_GW         up        48 ms     0.2%    3d 4h
    LTE_GW          down      —         —       —      (12m ago)

The **Since** column is the current up/down duration. A gateway that has been flapping (going up and down repeatedly) will show a short uptime even though the link has been present for days — that is the signal to look at the health check thresholds and the actual link quality.

### Gateway Health History

**Multi-WAN → Status → Gateway Details** shows a rolling chart of latency, packet loss, and up/down state for the last 24 hours. This is the tool to use when a user reports intermittent connectivity: the chart will show whether a specific WAN was failing at the time of the report.

The chart also shows the health-check events themselves — each failed probe is plotted. A gateway with clean up/down transitions but a scatter of failed probes is degrading but not yet failing; it is a candidate for proactive maintenance.

### Per-Policy Traffic

**Multi-WAN → Traffic** breaks down throughput by routing policy. This answers questions like:

- How much of my traffic is actually going out the backup link?
- Is the VoIP policy really pinned to the fibre link, or has it failed over?
- Are the weights I configured producing the split I expected?

If a policy's traffic is not on the gateway you expect, either the policy order is wrong (an earlier policy is matching first) or a gateway is down and the traffic has failed over. The status overview tells you which.

### WAN Monitor Log

The underlying monitor writes to the system log. From the console or Web Terminal:

    sudo journalctl -u stratum-wan-monitor -n 100 --no-pager

This shows every health-check probe, every state transition, and every failover event with timestamps. It is the ground truth when the panel's chart and your expectation disagree.

## Troubleshooting

### The Backup Link Never Gets Used

This is the most common Multi-WAN complaint, and it almost always has the same two causes.

**Cause 1 — It is working as designed.** In failover mode, the backup link is idle until the primary fails. That is correct. If you want the backup to carry traffic continuously, you want **load balancing**, not failover.

**Cause 2 — The primary link is "healthy enough".** If your primary is running at 300 ms latency and 15% packet loss but your trigger is "latency > 500 ms OR loss > 20%", the primary never trips. Tune the trigger to match what "healthy" actually means for your traffic. For VoIP, anything above 100 ms is already a problem; for bulk transfers, 500 ms is fine.

### Failover Happens Too Often

The primary link is flapping — going down, coming back up, going down again. Three causes, in order of likelihood:

1. **Health check target is unreliable.** A single remote host (say, `8.8.8.8`) that occasionally drops ICMP will cause false failures. Configure **two or more** health check targets per gateway so a single target failure does not mark the gateway down.
2. **Trigger thresholds too tight.** If normal latency on the primary is 80 ms and your trigger is "latency > 100 ms", you will see constant failovers. Set thresholds above the link's normal operating range, not at it.
3. **The link actually is flapping.** If the above are ruled out, the ISP link itself is unstable. This is a support ticket, not a configuration change.

### Traffic Goes Out the Wrong WAN

Two diagnostic steps:

1. **Check policy order.** Policies are evaluated top-down. A broad policy above a specific one shadows it. Use **Policy → Routing** and read the list in order — is the specific policy above the general one?
2. **Check with the trace tool.** **Firewall Rules → Trace** accepts a source/destination and reports which routing policy would apply. Use a source address from the affected class and a public destination. The trace output tells you which policy matched and which gateway it points to.

### Persistence Is Not Sticking

If a client's sessions are still scattered across gateways despite persistence being enabled:

- **Persistence timeout too short.** Default is often 300 seconds. If your service keeps a session alive for longer than that, the pin expires and the next session may take a different gateway. Raise the timeout above the longest expected session duration.
- **Source address is changing.** If the client is behind a NAT that varies its source address, or if the client is a mobile device roaming between networks, source-address persistence does not hold. There is no clean fix for this — the platform cannot pin what it cannot see.
- **Policy applies but the connection is short-lived.** Persistence is per-source, but each new connection re-evaluates the routing policy. If the client opens many short connections, they may spread even with persistence enabled. This is expected behavior; persistence solves mid-session continuity, not per-connection pinning.

### The Console Is Unreachable After a Policy Change

Same rule as firewall changes: **never change a routing policy on the interface you are connected through without a console session open on the appliance.** A misconfigured default route will lock you out of the panel just as effectively as a bad firewall rule, and the recovery path is the same physical console or serial connection.


## Worked Examples

Two configurations cover the majority of real deployments. Each one lists the interfaces, gateways, groups, and policies in the order you would create them.

### Example 1 — Two ISPs, Failover Only

**Goal:** Fibre primary, cable backup. All traffic uses the fibre link. If the fibre fails, everything moves to the cable link and returns when the fibre recovers. No traffic sharing.

**Interfaces:**

    WAN1 (eth0)   → fibre link, DHCP
    WAN2 (eth1)   → cable link, DHCP
    LAN  (eth2)   → internal network, static 192.168.1.1/24

**Gateways:**

    WAN1_GW       interface WAN1
    WAN2_GW       interface WAN2

**Gateway group:**

    Name:         PRIMARY_FAILOVER
    Members:      WAN1_GW (tier 1)
                  WAN2_GW (tier 2)
    Trigger:      loss > 20% OR latency > 500ms
    Protocol:     ICMP + HTTP

**Routing policy:**

    Name:         default-route
    Source:       any
    Destination:  any
    Gateway:      PRIMARY_FAILOVER
    Mode:         failover

**What this achieves:** All LAN traffic uses the fibre link. If the fibre link fails its health check, traffic moves to the cable link until the fibre recovers. When the fibre comes back, traffic returns to it after the health check passes three times in a row.

**What this does not achieve:** The cable link carries no traffic while the fibre is healthy. That is correct for failover. If you want the cable link to also be used for capacity, see Example 2.

### Example 2 — Two ISPs, Weighted Load Balancing with Persistence

**Goal:** Fibre (100 Mbps) and cable (20 Mbps) both carry traffic, weighted to match their capacities. Session persistence keeps a given client on the same link for 10 minutes to avoid breaking services that dislike source-IP changes.

**Interfaces and gateways:** Same as Example 1.

**Gateway group:**

    Name:         PRIMARY_BALANCED
    Members:      WAN1_GW (tier 1)  weight 5
                  WAN2_GW (tier 1)  weight 1
    Trigger:      loss > 20% OR latency > 500ms
    Protocol:     ICMP + HTTP

**Routing policy:**

    Name:         default-route
    Source:       any
    Destination:  any
    Gateway:      PRIMARY_BALANCED
    Mode:         loadbalance
    Persistence:  source address
    Timeout:      600 seconds

**What this achieves:** New connections are distributed across both links in roughly a 5:1 ratio, matching the capacity ratio. Existing sessions stay on their chosen link. A client's sessions all use the same WAN for 10 minutes at a time.

**What to watch:** If users report scattered logouts from a web service even with persistence enabled, the service's session timeout is longer than 600 seconds. Raise the persistence timeout above the longest session you need to preserve.

### Adding a Third WAN

The pattern extends naturally. Add the interface, add the gateway, add the gateway to a group at the appropriate tier (or with an appropriate weight), and reference the group from the routing policy. No other changes are needed.

A three-link failover setup, for example, uses three tiers:

    Tier 1:  WAN1_GW
    Tier 2:  WAN2_GW
    Tier 3:  LTE_GW

A three-link load-balanced setup uses weights instead:

    WAN1_GW   weight 5
    WAN2_GW   weight 3
    LTE_GW    weight 1

The LTE link is the lowest-weight member because metered links are usually the most expensive per gigabyte; the weights ensure it only carries a small fraction of traffic under normal conditions.

## Where to Go Next

- **Rules that decide what traffic is allowed** → [Firewall Rules](/resources/documentation/firewall-rules/)
- **Remote access over the WAN links** → [WireGuard VPN](/resources/documentation/vpn/)
- **Interface and zone concepts referenced above** → [Network Basics](/resources/documentation/network-basics/)
- **Panel navigation for the pages used here** → [Panel Tour](/resources/documentation/panel-tour/)

If a WAN link fails and you want to know why, the answer is almost always in the WAN monitor log. Start there before assuming a configuration error.

