---
title: "VPN > OpenVPN"
seo_title: "08.1 VPN > OpenVPN — Stratum FW User Manual"
description: "Remote access with OpenVPN: the server, which networks VPN users may reach, client DNS, advanced settings, giving users access, client profiles, fixed addresses and connected clients."
weight: 801
chapter_num: "08.1"
url: "/manual/vpn-openvpn/"
aliases:
  - "/manual/08-01-vpn-openvpn/"
translationKey: "manual-08-01-vpn-openvpn"
---

OpenVPN lets people work from outside as if they were in the office: their laptop or phone connects to the
firewall, gets an address in the VPN network, and reaches the internal networks you choose. Every user signs in
with **their own certificate and their own password** - a stolen laptop without the password, or a password
without the laptop, is not enough.

The OpenVPN client runs on Windows, macOS, Linux, iOS and Android (OpenVPN Connect, or OpenVPN GUI / Tunnelblick
/ the system's network manager).

![VPN > OpenVPN](/images/manual/ovpn.png)

## The server

Switch on **Enable the OpenVPN server (remote access)** and set:

### Connection

| Setting | Meaning |
| :--- | :--- |
| **Protocol** | **UDP (recommended)**, or **TCP** for networks that block UDP (some hotels and guest networks). |
| **Port** | Default 1194. A pass rule on the WAN for this port is added automatically. |
| **Accept connections on** | Every WAN, or one interface. |
| **External address clients connect to** | The public address or host name of the firewall, as written into the client profiles - for example `vpn.example.com`. Use a host name if your public address may change. |

### Tunnel and access

| Setting | Meaning |
| :--- | :--- |
| **VPN network** | Addresses for VPN clients, default `10.201.0.0/24`. Must not overlap a local network - nor the home networks your users connect from (avoid `192.168.0.0/24` and `192.168.1.0/24`). |
| **Maximum connections** | How many clients may be connected at once (default 64). |
| **Local networks VPN clients may reach** | Only the selected networks are routed to VPN clients and allowed by the firewall. **New interfaces and VLANs are not added automatically** - tick them here when you add them. |
| **Extra networks** | Networks behind a router in a local network ([**Interfaces > Routes**](/manual/interfaces-routes/)), up to 32. |
| **Send all internet traffic of VPN clients through the firewall** | Off: only traffic to your networks uses the VPN (split tunnel). On: everything does - for users on untrusted networks, or so the firewall's filters apply to them. |
| **VPN clients may reach each other** | Off by default. |
| **A user may connect from several devices at the same time** | Off: a new connection of the same user ends the previous one. |
| **Windows clients use only the VPN's DNS servers (block-outside-dns)** | Prevents Windows from asking its local DNS server for internal names. |
| **Require the client certificate usage "TLS client"** | Leave on. Switch it off only for certificates imported from another system that lack this mark. |

The firewall rules for VPN clients follow from **Local networks VPN clients may reach**: there is nothing to add on [**Firewall > Rules**](/manual/firewall-rules/).

### Client settings

| Setting | Meaning |
| :--- | :--- |
| **DNS servers** | Up to four, separated by commas. Empty: the firewall - so VPN users can use internal names and the [DNS blocklists](/manual/firewall-blocklists/) apply. |
| **DNS domain** | The domain for short names, for example `office.lan`. |
| **Search domains** | Further domains to try for short names. |

### Advanced settings

The defaults are right for almost every installation.

| Setting | Meaning |
| :--- | :--- |
| **Login method** | **Certificate and password (recommended)**, or **Certificate only**. |
| **Log level** | 1 errors only, 3 normal (default), 4 detailed, 5 debug - for troubleshooting a connection. |
| **Data encryption** | The ciphers offered: AES-256-GCM, AES-128-GCM, CHACHA20-POLY1305. |
| **Authentication digest** | SHA256 (default), SHA384 or SHA512. |
| **Minimum TLS version** | TLS 1.2 or TLS 1.3. |
| **Key renegotiation (seconds)** | Default 3600; 0 = never. |
| **Keepalive interval / timeout (seconds)** | Default 10 / 60: how quickly a dead connection is noticed. |
| **Tunnel MTU**, **MSS clamping** | Only for connections with packet-size problems (pages that never finish loading over the VPN). |

Click **Save**, then **Apply changes** and **Keep changes**. The certificate authority, the server certificate and
the keys are created on the firewall the first time; they never leave it.

> **Note:** Changing the protocol, port, external address, digest, ciphers or login method changes the client
> profiles: users must download and import their profile again.

## Give users access

VPN users are the accounts of **System > Users**. An account does not need access to the panel: a user without
panel access is a *VPN-only* account - the usual case for staff.

1. Create the user on **System > Users** (or use an existing one). Their password is the VPN password.
2. On **VPN > OpenVPN**, under **Users with VPN access**, switch on **VPN access** for the user. A client
   certificate is issued for them at once.
3. Download their **Profile** (`.ovpn`) and give it to the user in a safe way - it contains their certificate and
   private key. Do not send it by e-mail or a messaging app; hand it over on a USB stick or through a file share
   only they can reach.
4. The user imports the profile into the OpenVPN client and signs in with their user name and password.

Access changes take effect at once, without Apply. **Removing access revokes the certificate**: the profile stops
working immediately, even if the user kept a copy.

Disabling or deleting the user on **System > Users** revokes the certificate in the same way.

**Fixed VPN addresses** - for a user whose address appears in firewall rules - are set on **System > Users**
(**Fixed VPN address**). They come from the lower half of the VPN network (for `10.201.0.0/24`: `10.201.0.2` -
`10.201.0.127`); the upper half is the pool for everyone else. Fixed addresses cannot be used while **A user may
connect from several devices at the same time** is on.

Only administrators can download profiles.

## Connected clients

**Connected clients** lists the users connected now: **User**, **From** (their public address), **VPN address**,
**Connected** since, and the traffic **Received** and **Sent**. The [dashboard's](/manual/dashboard/) **VPN connections** panel shows the
same.

## Moving from OPNsense or pfSense

Users can keep the profiles already on their computers: the OPNsense importer (see **Migrating from OPNsense**)
takes over the certificate authority, the server certificate, the users' certificates and the TLS key, so the
existing profiles keep working when Stratum FW takes the address of the old firewall.

## Troubleshooting

| Symptom | Check |
| :--- | :--- |
| The client cannot connect at all | **External address** and **Port** in the profile are right and reachable; the provider's modem forwards the port if it is in router mode; the user's network allows UDP (else use TCP). |
| *AUTH_FAILED* | User name or password wrong, or the user has no VPN access. **Diagnostics > System Logs** shows the reason. |
| Connected, but internal servers do not answer | The server's network is ticked under **Local networks VPN clients may reach**; the server's own firewall (Windows firewall) allows the VPN network; the VPN network does not overlap the user's home network. |
| Internal names do not resolve | **DNS servers** empty (the firewall) or the internal DNS server; **DNS domain** set; on Windows, **block-outside-dns**. |
