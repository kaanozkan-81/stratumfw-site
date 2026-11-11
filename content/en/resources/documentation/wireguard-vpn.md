---
title: "WireGuard VPN Setup"
description: "Configure FreeBSD kernel WireGuard (if_wg) on Stratum Firewall — zero-dependency X25519 key generation, QR codes, per-VLAN access control, and zero-downtime peer updates."
lead: "Deploy wire-speed, kernel-resident WireGuard remote access in seconds with on-box X25519 cryptography and instant QR provisioning."
---

Stratum Firewall integrates directly with FreeBSD 15.1's native kernel **WireGuard (`if_wg`)** driver and implements **RFC 7748 X25519** key generation internally with zero external package bloat.

## 1. Enable the WireGuard Server

![WireGuard Setup](/images/docs/wireguard.png)

1. Open **VPN → WireGuard** and toggle **Enable WireGuard**.
2. **Listen Port &amp; WAN:** Keep the default `UDP 51820` (or choose a custom port) and select the listening WAN interface; Stratum detects your external endpoint address automatically (or accepts a custom hostname/Virtual IP).
3. **Tunnel Network:** Specify a dedicated private CIDR (e.g., `10.202.0.0/24`). Stratum assigns `.1` to the firewall's `wg0` interface and allocates unique `/32` addresses to peers.
4. **Access Networks &amp; Tunnel Mode:** Choose exactly which internal LANs and VLANs peers may reach, optionally enable **Full Tunnel** (`0.0.0.0/0` internet routing through the firewall) or **Peer-to-Peer** communication.
5. Click **Save** and **Apply changes**—Stratum automatically creates the required WAN UDP pass rule, tunnel filter rules, outbound NAT, and local DNS access.

## 2. Provision Client Peers (Zero Downtime)

1. Under **Peers**, click **Add Peer** and enter a descriptive name (e.g., `Engineering-MacBook` or `Phone`).
2. Keep **Generate key pair on the firewall** enabled (or paste a public key generated on the client device) and enable a **Preshared Key (PSK)** for post-quantum symmetric protection.
3. Save the peer:
   * **Mobile Devices (iOS / Android):** Scan the displayed **QR Code** directly inside the official WireGuard app.
   * **Laptops / Workstations:** Download the `.conf` file and import it into the WireGuard client.

Adding or removing a peer updates `wg0` live **without disconnecting active peers**, and all private keys remain stored in root-only files outside `config.json`. Live connection status, endpoint IP, last handshake, and RX/TX counters appear both on the WireGuard page and in the **VPN Connections** Dashboard widget.
