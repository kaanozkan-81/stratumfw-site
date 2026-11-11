---
title: "VPN Overview (WireGuard & OpenVPN)"
description: "Compare and deploy WireGuard and OpenVPN remote access on Stratum Firewall — FreeBSD kernel if_wg, built-in OpenVPN EC CA, Multi-WAN multihome, VPN-only users, and OPNsense migration."
lead: "Connect remote teams and devices securely using FreeBSD kernel WireGuard or full-featured OpenVPN with built-in PKI and Multi-WAN support."
---

Stratum Firewall includes first-class, built-in support for both **WireGuard** (running inside the **FreeBSD 15.1 kernel** via `if_wg`) and **OpenVPN** (with an integrated elliptic-curve Certificate Authority, user authentication, and drop-in OPNsense takeover).

## 1. Choosing Between WireGuard and OpenVPN

| Capability | WireGuard (`VPN → WireGuard`) | OpenVPN (`VPN → OpenVPN`) |
| :--- | :--- | :--- |
| **Execution Layer** | FreeBSD 15.1 kernel (`if_wg`) | Userspace daemon with `tls-crypt` / `tls-auth` |
| **Authentication** | Cryptographic key pairs + optional PSK | Per-user Certificate + Password (`bcrypt` / `argon2id`) or Cert-only |
| **Client Provisioning** | Instant QR code &amp; downloadable `.conf` | Single self-contained `.ovpn` profile per user |
| **Multi-WAN Support** | Automatic WAN endpoint detection | Ordered multi-`remote` lines + `multihome` symmetric WAN replies (`v0.27.1`) |
| **Best For** | Maximum throughput, mobile roaming (Wi-Fi ↔ LTE), quick setup | User/password policies, Windows DNS leak prevention, [OPNsense migration](/resources/documentation/opnsense-migration/) |

## 2. OpenVPN Remote Access Highlights (`v0.7.0` – `v0.27.1`)

Navigate to **VPN → OpenVPN** to deploy a complete remote-access VPN server in minutes:

* **Built-In Elliptic-Curve CA:** Stratum automatically initializes an EC Certificate Authority, server certificate, Certificate Revocation List (`CRL`), and `tls-crypt` channel protection. Disabling or deleting a user revokes their certificate immediately.
* **VPN-Only Accounts &amp; Fixed Client IPs (`System → Users`):** Create **VPN-only** accounts that can authenticate to OpenVPN without having login access to the web management panel, assign static tunnel IPs per user, and download embedded `.ovpn` profiles directly from the user list.
* **Granular LAN/VLAN Access &amp; DNS Leak Protection:** Select with checkboxes which internal LANs and VLANs VPN clients may reach (changes apply immediately), push internal DNS servers, and enable **`block-outside-dns`** so Windows clients only query VPN DNS.
* **Multi-WAN Ready (`v0.27.1`):** Publish multiple WAN addresses into client `.ovpn` profiles (`remote` lines tried in order); Stratum configures `multihome` and `pf` reply routing so connections on a secondary WAN always reply from that same WAN.

For step-by-step WireGuard peer setup and QR provisioning, continue to the [WireGuard VPN Guide](/resources/documentation/wireguard-vpn/).
