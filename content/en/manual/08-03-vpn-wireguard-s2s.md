---
title: "VPN > WireGuard S2S"
seo_title: "08.3 VPN > WireGuard S2S — Stratum FW User Manual"
description: "Site-to-site tunnels with WireGuard: joining your networks with another office over the internet, keys, who connects, local and remote networks, setting up the other side and tunnel status."
weight: 803
chapter_num: "08.3"
url: "/manual/vpn-wireguard-s2s/"
aliases:
  - "/manual/08-03-vpn-wireguard-s2s/"
translationKey: "manual-08-03-vpn-wireguard-s2s"
---

A site-to-site tunnel joins the networks of two locations - head office and a branch, two hotels, an office and a
data centre - over the internet, so that devices on both sides reach each other as if they were in one network.
Nobody signs in: the two firewalls keep the tunnel up by themselves.

**VPN > WireGuard S2S** builds these tunnels with WireGuard. The other side can be another Stratum FW, OPNsense,
pfSense, MikroTik RouterOS 7 or Linux. For devices that do not speak WireGuard (FortiGate, Cisco, most cloud VPN
gateways) use [**VPN > IPsec**](/manual/vpn-ipsec/).

![VPN > WireGuard S2S](/images/manual/wgs2s.png)

## How it works

- Each tunnel is its own interface: `wg1`, `wg2` ... (the remote access server on [**VPN > WireGuard**](/manual/vpn-wireguard/) is `wg0`
  and separate).
- The tunnel is **route based**: the remote networks are routed into the tunnel interface, and the other side may
  send only from those networks.
- Each side has its own key pair; each side needs the other's **public key**. Private keys never leave the
  firewall.
- The firewall rules are made for you: the tunnel's UDP port is opened on the WAN (only for the other side's
  address, when it is known), and traffic from the remote networks to the local networks is allowed when you
  choose so. Large packets are adjusted to fit into the tunnel.

## Before you start

- The networks on the two sides **must not overlap**. Two offices that both use `192.168.1.0/24` cannot be joined
  - renumber one of them first.
- At least one side needs a reachable public address (or a host name), with the tunnel's UDP port reaching the
  firewall - if the provider's modem is in router mode, forward the port on it.

## Add a tunnel

**Add tunnel**. The page has three parts: **This side**, **Other side** and **Networks**.

### This side

| Field | Meaning |
| :--- | :--- |
| **Name**, **Description** | For example *Branch Antalya*. |
| **Tunnel enabled** | A disabled tunnel keeps its settings and keys. |
| **Interface** | `wg1` ... `wg99`; the next free number is proposed. |
| **Listen port (UDP)** | From 51821 upwards; each tunnel has its own port. |
| **Open the port on** | Every WAN, or one. |
| **Tunnel address (optional)** | An address inside the tunnel for this side, for example `10.203.0.1/30` - only so the tunnel itself can be pinged. The page tells you the address for the other side. |
| **This side's public key** | Made when the tunnel is saved. Give it to the other side. |

### Other side

| Field | Meaning |
| :--- | :--- |
| **Its public key** | The other side's WireGuard public key. |
| **Its address**, **Its port** | Where the other side is reachable: its public IPv4 address or host name, and port (default 51820). Leave the address empty when the other side is behind NAT or has a changing address - it then connects to you. |
| **Preshared key** | Optional, recommended: an extra secret, the same on both sides. **Generate** makes one; **Show** shows it so you can copy it to the other side. **Remove the saved preshared key** removes it. |

**The keys are not known yet?** Each side needs the other's key, so one side has to start. Save the tunnel
**disabled**, without the other side's key: this side's key is made. Give it to the other side, get theirs, come
back, enter it and enable the tunnel.

### Networks

| Field | Meaning |
| :--- | :--- |
| **Local networks** | Your internal interfaces whose networks the other side may reach. |
| **Other local networks** | Networks behind a router in your local network, entered by hand. |
| **Remote networks** | The networks behind the other side. They are routed into the tunnel, and the other side may send only from them. `0.0.0.0/0` is not supported. |
| **Allow traffic from the remote networks to the local networks** | On: devices on the other side can open connections to your networks. Off: only connections started from your side work (with their replies). |
| **Keepalive (seconds)** | Default 25: keeps the tunnel open through NAT. 0 = off. Keep it on on a side behind NAT. |
| **MTU** | Default 1420. |

The panel refuses remote networks that overlap your local networks, the VPN networks or another tunnel's
networks.

Click **Save**, then **Apply changes** and **Keep changes**.

## Set up the other side

The networks are **mirrored**: your local networks are the other side's remote networks, and the other way round.

**Another Stratum FW:** add a tunnel there with this side's public key, the same preshared key, and the networks
mirrored. Give one side the other's address (the one with the public address), or both.

**MikroTik (RouterOS 7):**

```
/interface wireguard add name=wg-office listen-port=51820 private-key="<generated by the router>"
/interface wireguard peers add interface=wg-office public-key="<this side's public key>" \
    endpoint-address=<our public address> endpoint-port=51821 \
    allowed-address=192.168.1.0/24 preshared-key="<the preshared key>" persistent-keepalive=25s
/ip route add dst-address=192.168.1.0/24 gateway=wg-office
/ip firewall filter add chain=input protocol=udp dst-port=51820 action=accept place-before=0
```

(`192.168.1.0/24` stands for your local networks.) Copy the router's public key from
`/interface wireguard print` into **Its public key**.

**OPNsense / pfSense:** **VPN > WireGuard**: an instance (its keys and port) and a peer with this side's public key,
our networks as *Allowed IPs* and our address as endpoint; then a gateway or route for our networks and a rule on
the WireGuard interface.

## Status

The tunnel list shows each tunnel with its **Interface**, **Remote endpoint**, **Networks**, **Status**, and the
traffic **Received** and **Sent**. A tunnel is up when its last handshake is recent (within about two minutes).
The [dashboard's](/manual/dashboard/) **VPN connections** panel shows the site-to-site tunnels too.

## Troubleshooting

| Symptom | Check |
| :--- | :--- |
| No handshake | The public keys are crossed correctly (each side has the *other's* key); the preshared keys are the same; the side that connects has the right address and port; the port reaches the firewall (provider's modem, the other side's firewall). |
| Handshake, but no traffic | The networks are mirrored exactly; the other side routes our networks into its tunnel and its rules allow them; **Allow traffic from the remote networks** is on here if they start the connections. |
| Some pages hang over the tunnel | Lower the **MTU** on both sides (for example 1380). |
| The tunnel drops after a while behind NAT | **Keepalive** on the side behind NAT. |

**Diagnostics > Ping** from the firewall with a local source address, or the tunnel address, helps to tell the
two cases apart.
