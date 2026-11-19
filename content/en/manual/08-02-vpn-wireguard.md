---
title: "VPN > WireGuard"
seo_title: "08.2 VPN > WireGuard — Stratum FW User Manual"
description: "Remote access with WireGuard: the server, which networks peers may reach, adding a peer per device with a QR code or the device's own key, preshared keys and peer status."
weight: 802
chapter_num: "08.2"
url: "/manual/vpn-wireguard/"
aliases:
  - "/manual/08-02-vpn-wireguard/"
translationKey: "manual-08-02-vpn-wireguard"
---

WireGuard is a modern, fast and simple VPN, built into the FreeBSD kernel. It is the best choice for phones: it
connects instantly, survives changes between Wi-Fi and mobile data, and uses little battery.

Each device is a **peer** with its own key pair. There are no user names or passwords: the key *is* the
identity - which is why each device gets its own peer, and a lost device is removed by deleting its peer.

The WireGuard app runs on Windows, macOS, Linux, iOS and Android.

![VPN > WireGuard](/images/manual/wg.png)

## The server

Switch on **Enable WireGuard (remote access)** and set:

### Connection

| Setting | Meaning |
| :--- | :--- |
| **Port (UDP)** | Default 51820. A pass rule on the WAN is added automatically. |
| **Accept connections on** | Every WAN, or one interface. |
| **External address peers connect to** | The public address or host name of the firewall, written into the peers' configurations. |
| **Firewall public key** | Shown for information; needed only for peers whose configuration is made on the device itself. The firewall's private key never leaves it. |

### Tunnel and access

| Setting | Meaning |
| :--- | :--- |
| **Tunnel network** | Default `10.202.0.0/24`. The firewall takes the first address, peers the others. Must not overlap a local network. |
| **MTU** | Default 1420. Lower it only for connections with packet-size problems. |
| **Local networks peers may reach** | Only the selected networks are routed to peers and allowed by the firewall. **New interfaces and VLANs are not added automatically.** |
| **Extra networks** | Networks behind a router in a local network. |
| **Send all internet traffic of peers through the firewall** | Off: only traffic to your networks uses the tunnel. On: everything does. |
| **Peers may reach each other** | Off by default. |

### Peer settings

| Setting | Meaning |
| :--- | :--- |
| **DNS servers** | Up to four. Empty: the firewall. |
| **Keepalive (seconds)** | Default 25. Keeps mobile and NAT connections open. 0 = off. |

Click **Save**, then **Apply changes** and **Keep changes**.

## Add a peer

One peer per device - phone, laptop, tablet. **Add peer**:

| Field | Meaning |
| :--- | :--- |
| **Device name** | For example *Kaan - iPhone*. |
| **Description** | Optional. |
| **Tunnel address** | The device's address in the tunnel network. The next free one is proposed. |
| **Peer enabled** | Switch a peer off without deleting it. |
| **Preshared key** | An extra secret shared by this peer and the firewall, on top of the key pair - recommended. |
| **Keys** | **Generate the key pair on the firewall** - the configuration is then shown as a QR code and a file; or **The device has its own key pair** - enter the device's **public key**; its private key never leaves the device. |

Click **Save** and **Apply changes**. Other peers stay connected.

### Set up the device

- **Phone (keys generated on the firewall):** open the WireGuard app, choose *Add tunnel* > *Scan from QR code*,
  and scan the code shown under the peer on the WireGuard page.
- **Laptop (keys generated on the firewall):** download the configuration file and import it into the WireGuard
  app.
- **Device with its own keys:** create the tunnel in the app, copy its public key into the peer on the firewall,
  and enter on the device the firewall's public key, the external address and port, the device's tunnel address
  and the networks to route.

> **Warning:** A peer's configuration (QR code or file) holds its private key. Show the QR code only to its owner,
> and do not send the file by e-mail or messaging apps. Configurations are shown to administrators only.

A peer's keys cannot be renewed in place: to give a device new keys, delete the peer and add it again.

## Peer status

The **Peers** list shows each peer with its **Tunnel address**, **Status**, **Last handshake**, and the traffic
**Received** and **Sent**. WireGuard has no "connection" in the usual sense: a peer is active when its last
handshake is recent (within about two minutes). The [dashboard's](/manual/dashboard/) **VPN connections** panel shows the active
peers.

## OpenVPN or WireGuard?

| | [OpenVPN](/manual/vpn-openvpn/) | WireGuard |
| :--- | :--- | :--- |
| Sign-in | Certificate **and** password per user | Key per device |
| Speed, battery | Good | Better |
| Roaming (Wi-Fi ⇄ mobile) | Reconnects | Seamless |
| Works through networks that block UDP | Yes, with TCP | No (UDP only) |
| Best for | Laptops of staff, where a password must be required | Phones and personal devices |

Both can run at the same time.
