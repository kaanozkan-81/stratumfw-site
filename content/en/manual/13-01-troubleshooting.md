---
title: "Troubleshooting"
seo_title: "13 Troubleshooting — Stratum FW User Manual"
description: "What to check when the panel cannot be reached, the internet or DNS does not work, a change was reverted, a port forward or VPN does not connect, a site is blocked, or the firewall is slow - with the page or command for each step."
weight: 1301
chapter_num: "13"
url: "/manual/troubleshooting/"
aliases:
  - "/manual/13-01-troubleshooting/"
translationKey: "manual-13-01-troubleshooting"
---

Most problems are found in four places:

| Where | Shows |
| :--- | :--- |
| [**Dashboard**](/manual/dashboard/) | Gateways, services, interfaces, alerts - the overview |
| [**Diagnostics > Firewall Log**](/manual/diagnostics-firewall-log/) | What was blocked, and by which rule |
| [**Diagnostics > System Logs**](/manual/diagnostics-system-logs/) | What the services report; the **Configuration** tab for every Apply |
| [**Diagnostics > Ping / Traceroute / DNS**](/manual/diagnostics-tools-states-arp/) | What the firewall itself can reach |

The [**AI Assistant**](/manual/diagnostics-ai-assistant/) can look through them for you: *"Why can't 192.168.1.50 reach the internet?"*

## The web panel cannot be reached

| Check | How |
| :--- | :--- |
| Right address and port? | The console header shows the panel's addresses (`https://...`). The port is 443 unless changed on [**Admin Access**](/manual/system-settings-general-access/). |
| From the right network? | The panel answers on internal networks, from WAN only when allowed. With *Only the selected interfaces* on [**Admin Access**](/manual/system-settings-general-access/), other networks are refused. |
| Locked out after failed sign-ins? | Wait for the block time (default 15 minutes), or try from another address. |
| A change just cut you off? | Wait: unkept changes are reverted after the time limit. Or console option **7) Pending change** > **n**. |
| Panel not running? | Console option **4) Restart web panel**. |
| Settings wrong? | Console option **6) Management access** > **2)** resets port, networks, anti-lockout and SSH. |
| Password forgotten, phone with the authenticator lost? | Console option **3) Reset admin password** (also offers to switch off two-factor sign-in). |
| Browser refuses the certificate after a change? | Console **8) Shell**: `stratum-cert restore`. |

## No internet

Work from the firewall outwards:

1. **Dashboard > Gateways**: is the WAN *online*? *Down* means the firewall cannot reach its gateway - cable,
   modem, provider. PPPoE: **System Logs > PPPoE** (*authentication failed* = user name or password).
2. **Ping** `1.1.1.1` from the firewall. Works? The firewall's internet is fine.
3. **DNS lookup** of `freebsd.org`. Fails while ping works? DNS - see below.
4. From the device: does it have an address in the right network, the firewall as gateway and as DNS server
   (`ipconfig /all`, `ip a`)? No address: [**Services > DHCP Server**](/manual/services-dhcp-server/) - is it on for that network, is the range
   full?
5. [**Firewall > Rules**](/manual/firewall-rules/) of the device's network: is there a pass rule to the internet? **Firewall Log**,
   filtered by the device's address: are its packets blocked, and by which rule?
6. [**Firewall > NAT > Outbound**](/manual/firewall-nat/): automatic outbound NAT covers the interface networks and routed networks; a
   network added by hand (hybrid or manual mode) needs its own rule.

## DNS does not work

| Symptom | Check |
| :--- | :--- |
| No name resolves, from the firewall either | [**Services > Service Status**](/manual/services-service-status/): the resolver runs? [**Services > DNS Resolver**](/manual/services-dns-resolver/): upstream servers reachable (ping them)? With *Resolver* mode, the WAN must allow DNS to the internet's servers. |
| The firewall resolves, devices do not | The devices use the firewall as DNS server ([**Services > DHCP Server**](/manual/services-dhcp-server/): DNS servers empty). The interface's rules allow DNS to the firewall. Networks behind a router ([**Interfaces > Routes**](/manual/interfaces-routes/)) are not answered by Unbound yet - give them another DNS server for now. |
| One site does not resolve | [**Firewall > Blocklists**](/manual/firewall-blocklists/) and [**Firewall > Web Filter**](/manual/firewall-web-filter/) (including **Keywords** / *Your own list*) each have a check: is the name blocked, and by which list or category? If it resolves to `198.18.0.1`, an [**Application Control**](/manual/firewall-application-control/#block-page) rule is showing the block page. |
| Internal names do not resolve | [**Services > DNS Resolver**](/manual/services-dns-resolver/): host overrides, the local domain; for Active Directory a domain override to the domain controller. |
| Old answer after a change | Devices cache answers for minutes - `ipconfig /flushdns` on Windows. |

## My changes were reverted

**Keep changes** was not clicked in time - or the panel could not reach the firewall to confirm, because the change
cut the connection. That is the protection working: the previous settings are back.

- [**Diagnostics > System Logs > Configuration**](/manual/diagnostics-system-logs/) shows the apply and *automatic rollback*.
- Make the change again, and connect so that it does not cut you off - for example a new LAN address: change it,
  Apply, connect your computer to the new network, open the panel at the new address and **Keep** within the time.
- Apply failed instead? The same log says why; nothing was changed.

## A port forward does not work

1. Test **from outside** (mobile data), not from the LAN - from inside use the internal address or NAT reflection.
2. [**Firewall > NAT > Port Forward**](/manual/firewall-nat/): the rule is enabled, the **source** allows the testing address, the target
   address and port are right.
3. **Firewall Log**, filtered by the port: blocked on the WAN? Then the source of the forward does not match.
   Nothing at all? The traffic does not reach the firewall: is the provider's modem in router mode (forward the port
   there as well, or switch it to bridge mode)? Some providers block ports such as 25, 80, 443.
4. The server answers? It must have the firewall as its gateway, and its own firewall must allow the port.

## VPN does not connect

See the troubleshooting tables in [**VPN > OpenVPN**](/manual/vpn-openvpn/), [**VPN > WireGuard S2S**](/manual/vpn-wireguard-s2s/) and [**VPN > IPsec**](/manual/vpn-ipsec/). In short:

- the external address and port are reachable (and forwarded on the provider's modem);
- **System Logs > Authentication** (OpenVPN sign-ins), the **IPsec log**, WireGuard's last handshake;
- the networks reachable by VPN clients are ticked; the VPN network does not overlap the user's home network.

## A site or an app is blocked

1. [**Firewall > Web Filter**](/manual/firewall-web-filter/) (**Check a site** and the **Keywords** tab) and [**Firewall > Blocklists**](/manual/firewall-blocklists/) (**Check**): which category, keyword match (*Your own list*) or DNS list blocks the domain? Add an **Exception** (or allow-list entry), or switch off the category or list.
2. [**Firewall > Application Control > Blocked**](/manual/firewall-application-control/): is an Application Control rule cutting the application's TLS / QUIC handshake or showing the `198.18.0.1` block page? Add the device as an exception on the rule, or switch the rule to **Monitor only**. If `https://` shows a certificate warning instead of the block page, install the **Block Page CA** ([**System > Certificates**](/manual/system-certificates/#block-page-ca)).
3. [**Diagnostics > Firewall Log**](/manual/diagnostics-firewall-log/): is the connection blocked by an IP blocklist or a firewall rule?
4. [**Services > Intrusion Detection > Alerts**](/manual/services-intrusion-detection/): blocked by Suricata on a blocking interface? Disable the rule if it is a false positive.

## The firewall is slow

| Check | Where |
| :--- | :--- |
| CPU, memory, temperature | [**Dashboard**](/manual/dashboard/) |
| A full line | [**Diagnostics > Traffic Graphs**](/manual/diagnostics-traffic-graphs/) - peaks at the line's speed |
| Which device uses it | [**Diagnostics > States**](/manual/diagnostics-tools-states-arp/), [**Web Activity**](/manual/diagnostics-web-activity/) |
| Too many connections | [**System > Settings > Firewall & NAT**](/manual/system-settings-firewall-networking-tunables/): states in use against the maximum |
| Intrusion detection in Block mode | Suricata needs CPU: fewer categories, or Watch on busy interfaces |
| Throughput lower than the line | [**System > Settings > Networking**](/manual/system-settings-firewall-networking-tunables/): switch off LRO, then TSO |

## Asking for help

When you report a problem (on GitHub or to a colleague), include:

- the version (top of the dashboard) and the hardware;
- what you did, what you expected and what happened;
- the relevant lines of **System Logs** or **Firewall Log** - and the output of `stratum-config diff` if it is about
  an Apply.

**Mask public IP addresses, host names and user names before you post.** Never post passwords, keys, tokens, an
OPNsense `config.xml` or a backup file.
