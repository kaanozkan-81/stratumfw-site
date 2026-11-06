---
title: "WireGuard VPN"
lead: "Configure high-performance, zero-dependency remote access using WireGuard."
---

Stratum Firewall includes native integration for **WireGuard**, the modern, high-performance VPN protocol. Unlike other firewalls that rely on heavy external packages, Stratum implements the X25519 cryptography internally, ensuring maximum security and a minimal attack surface.

## Why WireGuard?

- **Blazing Fast:** Operates in the kernel space, resulting in significantly higher throughput than OpenVPN.
- **Stateless:** Handles network changes (like switching from Wi-Fi to LTE on a phone) seamlessly.
- **Modern Crypto:** Uses state-of-the-art cryptography (ChaCha20, Poly1305, Curve25519, BLAKE2).

## Setting up the Server
![WireGuard Setup](/images/docs/wireguard.png)

Before creating peers, you must enable the WireGuard service.

1. Navigate to **VPN → WireGuard**.
2. Toggle **Enable WireGuard**.
3. **Listen Port:** Leave as default (UDP 51820) unless you have a specific requirement.
4. **Tunnel Network:** Define the private IP range for your VPN (e.g., `10.202.0.0/24`). The firewall will take the first address (`10.202.0.1`), and peers will get the rest.
5. **Access Networks:** Select which internal networks (e.g., LAN) the VPN users are allowed to access.
6. Click **Save** and **Apply**.

## Creating a Peer (Device)

A "Peer" is a single device (a phone, a laptop) that connects to the VPN. 

1. Under the **Peers** section, click **Add Peer**.
2. **Device Name:** Give it a recognizable name (e.g., "Kaan iPhone").
3. **Key Generation:** Leave "Generate the key pair on the firewall" selected. Stratum will securely generate the private and public keys for the device.
4. **Preshared Key (PSK):** We highly recommend enabling this. It adds an extra layer of symmetric encryption (post-quantum resistance).
5. Click **Save**.

## Connecting a Mobile Device (QR Code)

The easiest way to connect a phone is via QR code.

1. On the WireGuard page, find your newly created peer.
2. Click the **QR Code** button.
3. Open the official WireGuard app on your phone (iOS or Android).
4. Tap **"+" → "Create from QR code"** and scan the code on your screen.

Your phone is now connected. You can verify the connection by checking the "Last Handshake" column in the Stratum panel.

## Connecting a Laptop (Configuration File)

For laptops, you can download the configuration file directly.

1. Click the **Download** icon next to the peer.
2. You will receive a `.conf` file.
3. Open the WireGuard app on your laptop, click **"Import tunnel(s) from file"**, and select the `.conf` file you just downloaded.

## Security Best Practices

- **One Peer Per Device:** Never share a peer configuration between multiple people. If a device is lost, you can simply delete that specific peer without affecting others.
- **Key Management:** The private keys of the peers are stored on the firewall with strict permissions (`chmod 600`). They are never exposed in the main `config.json`.
- **Delete Unused Peers:** If an employee leaves or a device is retired, delete the peer immediately to revoke access.

## Next Steps

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>Protect your DNS</h3>
    <p>Learn how to block malicious domains with DNSBL.</p>
    <a href="/resources/documentation/blocklists/" class="card-link">Blocklists Guide →</a>
  </div>
  <div class="card">
    <h3>Automate with the API</h3>
    <p>Manage peers programmatically using our REST API.</p>
    <a href="/resources/documentation/rest-api/" class="card-link">API Reference →</a>
  </div>
</div>
