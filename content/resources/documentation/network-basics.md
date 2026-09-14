---
title: "Network Basics"
lead: "How Stratum Gateway models your network — interfaces, roles, zones, and addressing."
---

Before configuring firewall rules, VPN, or filtering, it helps to understand how Stratum Gateway thinks about a network. This guide covers the core concepts: interfaces, roles, zones, and addressing.

## Interfaces

A **network interface** is a physical or virtual port on the gateway. On a typical deployment these are the physical Ethernet ports on the back of the machine — `enp1s0`, `enp2s0`, `eno1`, and so on — but Stratum also supports virtual interfaces:

| Type | Description |
|---|---|
| **Physical** | Physical Ethernet ports |
| **VLAN** | Virtual LANs tagged on a physical port (802.1Q) |
| **Bridge** | Software bridge combining multiple ports into one segment |
| **Bond** | Link aggregation across multiple physical ports |
| **PPPoE** | Point-to-point session used by some ISPs |

Interfaces appear under **Interfaces** in the panel. Each one shows its current state, traffic counters, and the role it's been assigned.

## Roles

Every interface in Stratum is assigned a **role**. The role tells the gateway what the interface is for and drives default behavior — routing, firewall policy, DHCP, and more.

### The three primary roles

| Role | Purpose |
|---|---|
| **WAN** | Faces the internet. Receives traffic from your ISP and forwards it inward. |
| **LAN** | Faces your internal network. Provides DHCP and DNS to clients. |
| **OPT** | Optional. Used for DMZ, guest networks, IoT, additional internal segments, or a second WAN. |

A minimal gateway has one WAN and one LAN. Most deployments add at least one OPT for a guest or IoT network.

### Role assignment

Roles are assigned in the panel under **Interfaces → [interface name]**. When you change a role, Stratum goes through its safe-apply process:

1. The change is validated
2. You're asked to confirm connectivity within a timeout
3. If you don't confirm, the change is rolled back automatically

This protects you from locking yourself out by assigning the wrong role to the interface you're connected through.

## Zones and segmentation

Each role maps to a **zone** in the firewall. By default:

- The **WAN zone** accepts almost nothing inbound and permits everything outbound
- The **LAN zone** is trusted — traffic between LAN clients is allowed, and outbound traffic is permitted
- Each **OPT zone** starts locked down and requires explicit rules

This default-deny posture means a freshly installed gateway is safe by default. You open what you need, rather than closing what you don't.

### Common zone designs

**Small office (three zones):**

- WAN — internet
- LAN — staff devices
- OPT — guest Wi-Fi

**Home office with IoT:**

- WAN — internet
- LAN — trusted devices
- OPT — IoT devices (isolated from LAN)

**Branch office with DMZ:**

- WAN — internet
- LAN — internal users
- OPT1 — DMZ (public-facing services)
- OPT2 — guest Wi-Fi

## IP addressing

Stratum Gateway uses standard IPv4 and IPv6 addressing. On a typical deployment:

| Interface | Default address |
|---|---|
| LAN | `192.168.1.1/24` |
| WAN | Assigned by ISP (DHCP, PPPoE, or static) |
| OPT | You choose (e.g. `192.168.10.1/24` for guest) |

### The LAN subnet

The LAN interface is configured with `192.168.1.1/24` on a fresh install. This gives you a management address that is stable, predictable, and cannot conflict with typical home routers (which usually use `192.168.0.1` or `192.168.1.1`).

You can change this address under **Interfaces → LAN**. If you do, plan the change carefully — clients receive their default gateway through DHCP, so a change requires DHCP lease renewal or a reboot of client devices.

### DHCP

Stratum includes a DHCP server (Kea) that hands out addresses to clients on each internal network. By default:

- **Pool:** `192.168.1.100` through `192.168.1.200`
- **Lease time:** 12 hours
- **Gateway:** the interface address (`192.168.1.1`)
- **DNS:** the interface address (where Unbound runs)

You can view and manage the DHCP configuration under **DHCP**. Static reservations — permanent IP assignments for specific MAC addresses — are useful for printers, servers, and any device that needs a stable address.

### DNS

DNS resolution is handled by **Unbound**, which runs locally on the gateway. Every client receives the gateway's own address as its DNS server.

This has two important implications:

1. **All DNS traffic passes through the gateway.** Filtering (Web Filtering via RPZ) works at this layer.
2. **DNS is fast.** Unbound caches aggressively, and once a name is resolved it stays in cache until the TTL expires.

You can see DNS activity under **DNS**, and manage filtering feeds under **Web Filter**.

## VLANs

A **VLAN** (Virtual LAN) lets you split a single physical interface into multiple logical networks. This is useful when:

- You have one switch that needs to carry multiple networks
- You want to separate departments, guest access, or IoT devices without adding physical hardware
- You want to trunk multiple networks over a single uplink to a remote site

### How Stratum handles VLANs

VLANs are configured under **Interfaces → Add VLAN**. A VLAN interface is named after its parent plus a numeric ID — for example, `enp1s0.10` is VLAN 10 on interface `enp1s0`.

Each VLAN interface gets its own role (LAN, OPT) and its own addressing. From the firewall's perspective, it's a distinct network.

### Requirements

For VLANs to work, the switch and any intermediate devices must be configured to:

- **Trunk** the port connected to the gateway (carry tagged frames)
- **Tag** traffic on the appropriate VLAN ID

If you're new to VLANs, start with a single test VLAN on a spare port before deploying VLANs across your production network.

## Bridges and bonds

### Bridges

A **bridge** combines two or more physical interfaces into one logical segment. Devices plugged into any of the bridged ports appear to be on the same network.

Bridges are useful when:

- You want multiple ports to serve as a single LAN segment
- You need to extend a network across multiple switches without VLAN configuration
- You're integrating an access point that expects to see the LAN directly

### Bonds

A **bond** combines multiple physical interfaces into one logical interface for redundancy or increased throughput. If one physical port fails, the bond stays up.

Two common modes:

- **Active-backup** — one port is active, the other is standby. Simple and reliable.
- **LACP (802.3ad)** — both ports are active, distributing traffic. Requires switch support.

For most deployments, **active-backup** is sufficient and less error-prone.

## Interface roles file

Stratum stores role assignments in a plain-text file at `/etc/nftables/interface-roles.conf`. Each line maps an interface name to a role:

~~
enp1s0|wan
enp2s0|lan
enp3s0|opt
enp1s0.10|lan
~~

This file drives the firewall's zone membership and is regenerated whenever you change a role from the panel. If you prefer, you can edit it directly — but changes made this way are only picked up after running the sync utilities:

~~
sudo sync-lan-ifaces
sudo sync-wan-ifaces
~~

For most users, changing roles through the panel is the right approach. The file is worth knowing about for troubleshooting and advanced configurations.

## Common network scenarios

### Replacing a consumer router

If you're replacing a home or small-office router:

1. Connect your ISP's modem to the WAN port
2. Connect your switch (or access point) to the LAN port
3. Log in to the panel at `https://192.168.1.1`
4. Configure the WAN role on the internet-facing interface
5. The LAN is preconfigured — clients will get addresses automatically

### Adding a guest network

To add an isolated guest network:

1. If you have a spare physical port, assign it the OPT role
2. If not, create a VLAN on the LAN port for guests
3. Give the new OPT/VLAN interface its own subnet (e.g. `192.168.10.1/24`)
4. Under **Rules**, create a rule allowing the guest zone outbound access to the internet
5. Block guest-to-LAN traffic by default (it is by default)
6. Optionally enable the **Captive Portal** on the guest interface

### Setting up a DMZ

To expose a service (web server, mail server) to the internet:

1. Assign a physical port or VLAN to OPT and label it DMZ
2. Give it a subnet such as `192.168.50.1/24`
3. Place the server behind that interface
4. Under **NAT → Port Forward**, create a port-forward rule from WAN to the server's DMZ address
5. Under **Rules**, allow only the specific ports you need from WAN to DMZ
6. DMZ-to-LAN traffic should remain blocked

## Troubleshooting

### An interface shows as down

- Check the physical connection — cable, switch port, LED activity
- Verify the interface name under **Interfaces** matches the physical port
- Try a different cable or port to isolate the problem
- On virtual machines, confirm the virtual NIC is connected to the correct virtual switch

### Clients can't get an IP address

- Confirm the DHCP server is running under **Services**
- Verify the DHCP pool under **DHCP** doesn't conflict with the interface subnet
- Check the client's lease table entry under **DHCP → Leases**
- If the pool is exhausted, expand it under **DHCP → Pool**

### No internet access from clients

- Check that the WAN interface has an address and shows as up
- Verify DNS resolution works from the gateway itself
- Check firewall rules under **Rules** — the default policy should permit LAN-to-WAN
- Confirm NAT is applied to LAN traffic under **NAT**

### VLAN traffic not passing

- Verify the switch port is configured as a trunk
- Confirm VLAN IDs match on both sides
- Check that the VLAN interface exists under **Interfaces**
- Test with a single VLAN before deploying multiple

## Next steps

<div class="card-grid grid-2" style="margin-top:32px;">

  <div class="card">
    <h3>Firewall Rules</h3>
    <p>Create, order, and apply firewall rules between zones.</p>
    <a href="/resources/documentation/firewall-rules/" class="card-link">Continue →</a>
  </div>

  <div class="card">
    <h3>Initial Configuration</h3>
    <p>Hostname, timezone, WAN setup, and security modules.</p>
    <a href="/resources/documentation/initial-configuration/" class="card-link">Review guide →</a>
  </div>

</div>
