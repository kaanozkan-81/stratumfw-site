---
title: "VPN (WireGuard)"
description: "Set up WireGuard VPN on Stratum Gateway — server configuration, peer management, client provisioning, and site-to-site tunnels."
weight: 7
---

WireGuard is a modern, fast, and auditable VPN protocol built into the Linux kernel. Stratum Gateway uses it for two distinct purposes: remote access (letting users connect to the internal network from anywhere) and site-to-site tunnels (linking two Stratum Gateways across the internet so their LANs behave as one network).

This guide covers both use cases. The interface configuration is shared; the peer design is what differs.

> **Before you start:** You need at least one WAN interface with a reachable public IP (or a port-forward rule pointing to your gateway — see [Firewall Rules](/resources/documentation/firewall-rules/)). If your WAN is behind CGNAT without a public IP, use the site-to-site pattern with one side acting as the server and the other initiating the tunnel.

## Why WireGuard

A few properties make WireGuard a good fit for this platform:

- **Kernel-integrated.** No userspace daemon, no tun device overhead, no OpenVPN-style cipher negotiation.
- **Small attack surface.** Roughly 4,000 lines of code versus OpenVPN's ~70,000. Less code, fewer bugs.
- **Roaming-friendly.** Peers can move between networks (Wi-Fi to LTE) without reconnecting.
- **Simple configuration.** Every peer is defined by a key pair, an allowed-IPs list, and an endpoint. There is no certificate authority, no cipher suite negotiation, no options that quietly weaken security.

The trade-off: WireGuard is opinionated. It does not hide behind TLS, it does not negotiate anything, and it expects the network to handle the details. In practice, this makes it *easier* to configure correctly, not harder — as long as you understand the two concepts below.

## Key Concepts

Two ideas account for almost every WireGuard misconfiguration:

### Key Pairs

Every peer (server or client) has a **private key** and a **public key**. The private key never leaves the peer. The public key is shared with everyone who needs to talk to it.

The server holds:
- Its own private key (generated on the appliance)
- The public key of every peer that is allowed to connect

Each client holds:
- Its own private key
- The server's public key

There is no certificate authority, no expiry, no revocation list. A peer is trusted because its public key is on the server's allowed list. Removing the peer's public key from the server immediately revokes access.

### Allowed IPs

The `AllowedIPs` field is the single most misunderstood part of WireGuard. It means two different things depending on which side you are configuring:

- **On the server**, `AllowedIPs` for a peer is the list of source addresses that peer is permitted to send from. A peer configured with `AllowedIPs = 10.0.100.5/32` may only originate traffic from `10.0.100.5`. Traffic arriving from that peer with any other source address is dropped.
- **On the client**, `AllowedIPs` is the list of destination networks that should be routed *through* the tunnel. `AllowedIPs = 0.0.0.0/0` means "send all traffic through the VPN" (full tunnel). `AllowedIPs = 10.0.0.0/24` means "only send traffic destined for the LAN through the VPN" (split tunnel).

The same field name means "who am I allowed to receive from" on one side and "what am I allowed to send to" on the other. This asymmetry is not a bug — it is how WireGuard implements routing without a separate routing table.

## Server Configuration

The WireGuard server is configured once per gateway. It has a single key pair, a single listen port, and a single tunnel address range.

**VPN → WireGuard → Server → Edit**

    Enabled:          yes
    Listen port:      51820
    Interface:        WAN (or a specific WAN if you have Multi-WAN)
    Tunnel address:   10.0.100.1/24
    Private key:      (generated automatically)
    Public key:       (derived, shown for reference)

The **tunnel address** is the server's own address inside the VPN. It does not need to be reachable from the internet — it is only used between the server and its peers. Using a private range that does not conflict with any of your LAN subnets is important; `10.0.100.0/24` is a common choice.

The **listen port** must be reachable from the internet. Either the WAN interface has a public IP and the firewall allows UDP/51820, or a port-forward rule on the upstream router sends UDP/51820 to this gateway.

> **Firewall reminder:** The default WAN inbound policy blocks everything. WireGuard traffic will be dropped until you create a rule allowing UDP/51820 on the WAN interface. See the Firewall Rules guide for the exact steps.

## Peer Management

A **peer** is any device that connects to the server: a laptop, a phone, a branch office gateway. Each peer has its own key pair and its own entry on the server.

**VPN → WireGuard → Peers → New**

    Name:             laptop-kaano
    Public key:       (paste from client, or generate here)
    Allowed IPs:      10.0.100.10/32
    Persistent keepalive: 25 seconds

Fill in the peer's **Allowed IPs** with a single address from the tunnel range, one address per peer. This gives the server a routing target and gives the firewall something to reference in rules — "allow `laptop-kaano` to reach the internal DNS server" is easier to read than a raw `/32`.

### Persistent Keepalive

WireGuard is silent by default. If no traffic flows for a while, the tunnel state expires and the next packet from the server side has to wait for the client to re-handshake. On most networks this is fine. On networks with aggressive NAT timeouts (mobile carriers, some corporate firewalls), the tunnel can appear to die during idle periods.

Setting **Persistent keepalive** to 25 seconds sends a small keepalive packet in each direction periodically. It costs nothing meaningful in bandwidth and prevents the tunnel from going cold. Enable it on any peer that connects from a mobile network or from behind a NAT you do not control.

### Generating the Client Key Pair

The panel can generate a key pair for the client, so you do not need to install WireGuard tools on the client just to create a config.

**VPN → WireGuard → Peers → New → Generate key pair**

The server generates the key pair, keeps the private key on the appliance long enough to produce a client configuration, and shows the public key in the peer entry. The client configuration (including the private key) can be downloaded as a text file or shown as a QR code.

> **Security note:** The private key is only available at the moment of creation. If you lose it, you cannot recover it — you must generate a new key pair and update the peer. Download the config file before navigating away from the page.


## Client Provisioning

Once a peer exists on the server, the client needs a configuration file. The file contains everything the client needs: its own private key, the server's public key, the server's endpoint (public IP or hostname and port), and the AllowedIPs list.

### Downloading the Configuration

**VPN → WireGuard → Peers → [peer name] → Show config**

The panel offers two delivery formats:

- **Text file (`.conf`)** — download and import into any WireGuard client
- **QR code** — scan directly from the WireGuard mobile app

Both formats contain the same data. The QR code path is faster on mobile; the text file is the only practical option on desktop operating systems without a QR scanner.

### Full Tunnel vs. Split Tunnel

The AllowedIPs field on the client determines which traffic goes through the VPN. Choose deliberately.

**Full tunnel (`AllowedIPs = 0.0.0.0/0, ::/0`)**

All client traffic — including general web browsing — is routed through the VPN. The client's public IP becomes the gateway's WAN IP. The trade-offs:

- **Pro:** The client is protected by the gateway's firewall, IDS, and web filtering even when away from the office. Useful for remote workers on untrusted networks.
- **Pro:** All traffic is logged centrally, which helps with compliance.
- **Con:** All traffic flows through the WAN link, so the client's effective speed is limited by that link.
- **Con:** If the VPN goes down, the client loses internet access entirely (unless the client is configured to fall back).

**Split tunnel (`AllowedIPs = 10.0.0.0/8, 192.168.0.0/16`)**

Only traffic destined for the listed networks goes through the VPN. Everything else uses the client's normal internet connection.

- **Pro:** Fast and lightweight; only internal resources consume VPN bandwidth.
- **Pro:** If the VPN goes down, the client only loses access to internal resources, not the internet.
- **Con:** The client's traffic to the public internet is *not* filtered by the gateway. No web filtering, no IDS protection for that traffic.

For most remote-access use cases, split tunnel with the LAN subnets listed is the right default. Use full tunnel when the security posture requires filtering all client traffic, or when the client is expected to be on hostile networks.

The panel lets you choose the mode per peer at download time; the same peer can be provisioned to different clients with different modes.

## Client Setup by Platform

The WireGuard client is available for every major platform. Installation and import steps vary slightly; the essentials are the same everywhere.

### Windows

1. Download the WireGuard installer from **wireguard.com/install**
2. Install and launch the WireGuard application
3. **Add Tunnel → Import from file** and select the downloaded `.conf`
4. Click **Activate**

The tunnel status indicator turns green when the handshake succeeds. If it stays yellow, the client is trying but has not received a response — see Troubleshooting below.

### macOS

1. Install WireGuard from the Mac App Store
2. Open the app and choose **Import tunnel(s) from file**
3. Select the `.conf` and confirm
4. Click **Activate**

macOS may prompt for permission to add a VPN configuration — accept it. The tunnel appears in the system menu bar for quick toggling.

### Linux

Two options, both work:

**Command line (wg-quick):**

    sudo apt install wireguard
    sudo cp laptop-kaano.conf /etc/wireguard/wg0.conf
    sudo chmod 600 /etc/wireguard/wg0.conf
    sudo wg-quick up wg0

To bring the tunnel up automatically on boot:

    sudo systemctl enable wg-quick@wg0

**NetworkManager (desktop):**

    Settings → Network → VPN → + → Import from file

Select the `.conf`. The tunnel then appears in the system tray and can be toggled like any other connection.

### iOS

1. Install **WireGuard** from the App Store (free, by WireGuard Development Team)
2. Open the app and tap **+ → Create from QR code**
3. Scan the QR code shown in the panel
4. Name the tunnel and tap **Save**
5. Toggle the tunnel on

iOS may prompt to allow VPN configuration — accept it. The tunnel status appears in the app and in Settings → VPN.

### Android

1. Install **WireGuard** from the Play Store
2. Tap **+ → Scan from QR code**
3. Scan the QR code
4. Name the tunnel and tap **Create tunnel**
5. Toggle the tunnel on

Android's always-on VPN feature is worth enabling for corporate devices: **Settings → Network → VPN → WireGuard → Always-on VPN**. This ensures the tunnel comes back automatically after reboots or network changes.

### Generating a QR Code Manually

If you need to show a QR code from a client that has the text config but not the panel, any WireGuard QR tool works. The `qrencode` package on Linux:

    sudo apt install qrencode
    qrencode -t ansiutf8 < laptop-kaano.conf

The QR code is just the base64 encoding of the text config. There is no hidden state — what you scan is what you downloaded.


## Site-to-Site Tunnels

A site-to-site tunnel links two Stratum Gateway deployments so that devices on one LAN can reach devices on the other LAN without individual VPN clients. The two gateways form a permanent tunnel between them, and each gateway routes the other's LAN subnets through it.

This is the right pattern when:

- Two offices need to share resources (a file server, a domain controller, an internal application)
- A branch office needs access to headquarters without per-device configuration
- A cloud VPC needs to reach an on-premises network

### Topology

Site-to-site is a **peer relationship**, not a client-server relationship. Both sides are WireGuard peers, and both sides need:

- Their own WireGuard private key
- The other side's public key
- The other side's endpoint (public IP or hostname and port)
- The other side's LAN subnets in their AllowedIPs list

There is no server and no client. Both sides dial each other, and either can initiate the handshake.

### Configuring Side A (Headquarters)

**VPN → WireGuard → Server → Edit**

    Listen port:      51820
    Tunnel address:   10.0.200.1/24
    Private key:      (generated)

**VPN → WireGuard → Peers → New**

    Name:             branch-gw
    Public key:       (paste from Side B's server public key)
    Allowed IPs:      10.0.200.2/32, 192.168.20.0/24
    Endpoint:         branch.example.com:51820
    Persistent keepalive: 25 seconds

The Allowed IPs field lists two things: the branch gateway's tunnel address (`10.0.200.2/32`) and the branch LAN subnet (`192.168.20.0/24`). Both are needed — the tunnel address so the gateways can reach each other, the LAN subnet so devices behind Side B are routable through the tunnel.

### Configuring Side B (Branch)

**VPN → WireGuard → Server → Edit**

    Listen port:      51820
    Tunnel address:   10.0.200.2/24
    Private key:      (generated)

**VPN → WireGuard → Peers → New**

    Name:             hq-gw
    Public key:       (paste from Side A's server public key)
    Allowed IPs:      10.0.200.1/32, 192.168.10.0/24
    Endpoint:         hq.example.com:51820
    Persistent keepalive: 25 seconds

Symmetric configuration. Side B lists Side A's tunnel address and LAN subnet; Side A lists Side B's tunnel address and LAN subnet.

### Enabling Forwarding Between the Tunnels

By default, traffic arriving on the WireGuard interface is not forwarded to the LAN interface. You need an explicit filter rule on each side.

**On Side A (Headquarters):**

    #   Interface   Source            Destination        Service   Action
    1   WG          branch-lan        hq-lan             any       Pass

**On Side B (Branch):**

    #   Interface   Source            Destination        Service   Action
    1   WG          hq-lan            branch-lan         any       Pass

Where `branch-lan` and `hq-lan` are aliases for the respective subnets (`192.168.20.0/24` and `192.168.10.0/24`). With these rules, devices on Side A can reach devices on Side B and vice versa. Without them, the tunnel comes up but nothing crosses it.

> **Common mistake:** Creating the WireGuard peer entries and expecting traffic to flow. The peer config establishes the tunnel; the filter rule allows the traffic. Both are required.

### Routing Considerations

Site-to-site routing is handled by the AllowedIPs field on each peer. There is no separate static route to add. If Side A wants to reach `192.168.20.0/24` (Side B's LAN), it just needs `192.168.20.0/24` in Side B's peer entry on Side A. WireGuard installs the route automatically.

If you have more than two sites, or if any site has multiple LAN subnets, the Allowed IPs list grows. Keep it explicit — avoid `0.0.0.0/0` on a site-to-site peer unless you really want that side to route all internet traffic through the other, which is rarely what you mean.

## Firewall and Routing Integration

For VPN traffic to reach its destination, three things must be true:

1. **The tunnel is up.** The WireGuard handshake has succeeded on both sides.
2. **The traffic is allowed by a filter rule.** A rule with `Interface: WG` must pass the relevant source/destination/service.
3. **A route exists.** WireGuard installs the route automatically from AllowedIPs, but if you have overlapping subnets or unusual topologies, verify with the routing table.

### Common Rules for Remote Access VPN

For a typical remote-access setup (laptop peers connecting to the office), the rules that matter are:

    #   Interface   Source             Destination         Service      Action
    1   WG          vpn-clients        dns-server          DNS          Pass
    2   WG          vpn-clients        internal-subnet     any          Pass
    3   WG          vpn-clients        any                 any          Block    + log

Where `vpn-clients` is an alias containing every peer's tunnel address (`10.0.100.10/32`, `10.0.100.11/32`, and so on). The final block rule is a safety net: if the intended policy is "VPN clients may reach the internal network, nothing else", this rule enforces it.

If you want VPN clients to also reach the internet through the gateway (full tunnel mode), replace the final block rule with:

    3   WG          vpn-clients        any                 any          Pass

And make sure the outbound NAT on the WAN interface covers the VPN client subnet, or the return traffic will not find its way back. On most setups, the automatic SNAT covers it; if not, add an explicit SNAT rule for the VPN subnet on the WAN interface.

### Keeping VPN Traffic Out of the LAN

A common requirement is that VPN clients should reach some internal resources but not others. Two approaches:

**Approach 1 — Rule-based.** Add block rules for the sensitive subnets above the general allow rule. VPN clients can reach `internal-subnet` but a rule blocking `vpn-clients → finance-subnet` placed above the allow prevents access to finance specifically.

**Approach 2 — Alias-based.** Define `allowed-destinations` as an alias containing only the subnets VPN clients may reach, then make the allow rule reference it directly. Cleaner, but requires maintaining the alias when destinations change.

For most deployments, Approach 1 is easier to reason about: the allow rule stays broad, the block rules document the exceptions explicitly.


## Troubleshooting

Four failure modes account for almost every WireGuard problem. Each one has a distinct symptom, so the diagnosis is usually fast.

### Handshake Never Completes

**Symptom:** The client shows a tunnel state of "connecting" or "waiting" indefinitely. The server shows the peer as offline. No traffic flows.

**Cause 1 — UDP port is blocked.** The WAN inbound policy blocks UDP/51820. Check the firewall log for dropped UDP traffic to the gateway's WAN address on port 51820. If you see drops, add a filter rule:

    Interface:   WAN
    Source:      any
    Destination: WAN address
    Service:     WireGuard (UDP 51820)
    Action:      Pass

**Cause 2 — NAT timeout on the client's side.** The client is behind a NAT that drops UDP mappings after a short idle time. Enable **Persistent keepalive** on the peer entry (25 seconds is standard).

**Cause 3 — Wrong endpoint.** The client's config points to the wrong IP or hostname. Verify by checking what the panel shows for the server's WAN address, and compare it to the client config. Dynamic DNS services occasionally fail to update; a stale A record is a common cause.

**Cause 4 — Public IP changed.** If the WAN uses DHCP and the public IP changed, the client config still references the old IP. This is where a dynamic DNS name is useful — you re-point the DNS record instead of every client config.

### Handshake Succeeds But No Traffic Flows

**Symptom:** The client shows the tunnel as up. The server shows the peer as connected. But `ping 10.0.100.1` from the client fails, and no internal resources are reachable.

**Cause 1 — Missing filter rule.** The most common cause. The tunnel is up but the firewall rule that permits traffic from the WG interface does not exist. Add the rule described in the Firewall and Routing Integration section.

**Cause 2 — Wrong AllowedIPs.** On the client, the AllowedIPs field does not include the destination you are trying to reach. If you set `AllowedIPs = 10.0.0.0/24` but the destination is `192.168.1.50`, the client does not route the packet into the tunnel — it sends it out the normal gateway. Fix by expanding AllowedIPs to include the correct subnet.

**Cause 3 — Return path missing.** The gateway can reach the client, but the reply is not routed back. This happens when the destination network has a route to the internet but not back to the VPN subnet. For remote-access setups this is rarely an issue (the gateway is the default route for both sides); for site-to-site it means one side is missing an AllowedIPs entry.

### Handshake Works, Then Drops

**Symptom:** The tunnel comes up, works for a few minutes, then goes silent. Reconnecting restores it for another few minutes. This pattern is almost always idle-timeout related.

**Cause:** Somewhere between the two endpoints, a NAT device is dropping the UDP mapping during idle periods. Either side has a default UDP session timeout (often 30–60 seconds) that is shorter than the traffic pattern requires. **Persistent keepalive** on both sides solves it. Set it to 25 seconds on any peer that traverses a NAT not under your control — mobile networks, guest Wi-Fi, corporate firewalls.

### Slow Throughput

**Symptom:** The tunnel works, but throughput is a fraction of what the underlying WAN link should deliver. WireGuard's own overhead is minimal (~4% on modern hardware), so a big slowdown indicates something else.

**Cause 1 — MTU.** The most common cause. WireGuard adds 60–80 bytes of overhead per packet, which pushes some packets over the path MTU and forces fragmentation or drops. The fix is to lower the MTU on the WireGuard interface:

    MTU: 1420

1420 is the standard safe value for WireGuard over an Ethernet WAN with typical ISP MTU (1500). If PPPoE is involved (which has a smaller MTU), 1380 is often required. Try 1420 first; if that does not help, try 1380.

**Cause 2 — CPU on the client.** WireGuard is efficient, but on very low-powered clients (older phones, small routers), the kernel crypto can still be the bottleneck. Check the client's CPU during a transfer.

**Cause 3 — WAN bandwidth is genuinely the limit.** Verify by running a speed test directly on the gateway's WAN interface and comparing.

## Worked Examples

### Example 1 — Remote Worker Access to the Office

**Goal:** One laptop user needs to reach the office LAN (`192.168.10.0/24`) and the internal DNS server (`192.168.10.5`) from anywhere. General internet browsing should use the client's local connection, not the VPN.

**Server config:**

    Listen port:      51820
    Tunnel address:   10.0.100.1/24

**Peer entry:**

    Name:             laptop-user
    Public key:       (generated)
    Allowed IPs:      10.0.100.10/32
    Persistent keepalive: 25

**Client config (downloaded):**

    [Interface]
    PrivateKey = <generated>
    Address    = 10.0.100.10/24
    DNS        = 192.168.10.5

    [Peer]
    PublicKey  = <server public key>
    Endpoint   = vpn.example.com:51820
    AllowedIPs = 192.168.10.0/24
    PersistentKeepalive = 25

The client's AllowedIPs is `192.168.10.0/24` — split tunnel. Internet traffic does not go through the VPN.

**Filter rule (on the server):**

    #   Interface   Source         Destination       Service   Action
    1   WG          vpn-clients    internal-subnet   any       Pass

### Example 2 — Two Offices Connected Site-to-Site

**Goal:** Two offices with LANs `192.168.10.0/24` (HQ) and `192.168.20.0/24` (branch) should be able to reach each other's resources. Each office uses its own internet connection for general browsing.

**HQ server config:**

    Listen port:      51820
    Tunnel address:   10.0.200.1/24

**HQ peer entry (for branch):**

    Name:             branch-gw
    Public key:       (branch's server public key)
    Allowed IPs:      10.0.200.2/32, 192.168.20.0/24
    Endpoint:         branch.example.com:51820
    Persistent keepalive: 25

**Branch server config:**

    Listen port:      51820
    Tunnel address:   10.0.200.2/24

**Branch peer entry (for HQ):**

    Name:             hq-gw
    Public key:       (HQ's server public key)
    Allowed IPs:      10.0.200.1/32, 192.168.10.0/24
    Endpoint:         hq.example.com:51820
    Persistent keepalive: 25

**Filter rules:**

HQ:

    #   Interface   Source            Destination    Service   Action
    1   WG          branch-subnet     hq-subnet      any       Pass

Branch:

    #   Interface   Source            Destination    Service   Action
    1   WG          hq-subnet         branch-subnet  any       Pass

Where `branch-subnet` and `hq-subnet` are aliases for `192.168.20.0/24` and `192.168.10.0/24`. With these in place, a device at HQ can reach a printer at the branch by IP, and vice versa, without any client software on the endpoints.

### Example 3 — Mobile Devices with Always-On VPN

**Goal:** Corporate phones should always have the VPN up. If the phone reboots or switches from Wi-Fi to cellular, the tunnel should come back automatically.

**Server config:** Same as Example 1, with a dedicated tunnel range for mobile peers if you want to separate them from laptops (for example, `10.0.100.50/24`).

**Peer entry per phone:**

    Name:             phone-user1
    Public key:       (generated)
    Allowed IPs:      10.0.100.50/32
    Persistent keepalive: 25

**Client config (via QR code):**

    [Interface]
    PrivateKey = <generated>
    Address    = 10.0.100.50/24
    DNS        = 192.168.10.5

    [Peer]
    PublicKey  = <server public key>
    Endpoint   = vpn.example.com:51820
    AllowedIPs = 0.0.0.0/0
    PersistentKeepalive = 25

AllowedIPs is `0.0.0.0/0` — full tunnel. All phone traffic goes through the office, protected by the gateway's IDS and web filtering.

On the phone: enable **Always-on VPN** in the WireGuard app settings. From that point, the tunnel comes up automatically after reboots and network changes. The phone cannot reach the internet without the tunnel, which is the point of the always-on configuration.

## Where to Go Next

- **Rules that permit VPN traffic** → [Firewall Rules](/resources/documentation/firewall-rules/)
- **Redundant WAN links for the tunnel endpoint** → [Multi-WAN](/resources/documentation/multi-wan/)
- **Interface and zone concepts referenced above** → [Network Basics](/resources/documentation/network-basics/)
- **Panel navigation for the pages used here** → [Panel Tour](/resources/documentation/panel-tour/)

WireGuard problems almost always fall into one of four buckets: handshake, filtering, MTU, or keepalive. If you are debugging, identify which bucket first — the fix is usually obvious once the bucket is known.

