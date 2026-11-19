---
title: "Firewall > NAT"
seo_title: "06.2 Firewall > NAT — Stratum FW User Manual"
description: "Network address translation: port forwards to internal servers, outbound NAT modes and rules, 1:1 NAT, and NAT reflection."
weight: 602
chapter_num: "06.2"
url: "/manual/firewall-nat/"
aliases:
  - "/manual/06-02-firewall-nat/"
translationKey: "manual-06-02-firewall-nat"
---

NAT (network address translation) rewrites addresses as packets pass the firewall. **Firewall > NAT** has three
tabs:

| Tab | What it does | Typical use |
| :--- | :--- | :--- |
| **Port Forward** | Sends connections that arrive on a port of the firewall to a host inside. | Reaching a web server, a camera recorder or a remote desktop from outside. |
| **Outbound** | Rewrites the source address of traffic leaving through a WAN. | Letting all internal devices share the WAN address (automatic); a server leaving through its own public address. |
| **1:1** | Maps a public address to an internal host, both ways. | A server that needs its own public address for everything. |

## Port forwards

A port forward makes an internal service reachable from outside: connections to the **external port** on the
firewall are sent to the **target** host and port.

![Firewall > NAT > Port Forward](/images/manual/nat-forward.png)

**Every port forward gets its pass rule automatically** - it appears as a grey system rule on the interface's
[**Firewall > Rules**](/manual/firewall-rules/) tab. You do not write a separate rule.

### Add a port forward

Click **Add port forward**.

| Field | Meaning |
| :--- | :--- |
| **Interface** | Where the traffic arrives - normally WAN. |
| **Destination address** | The interface address, or one of its virtual IPs ([**Interfaces > Virtual IPs**](/manual/interfaces-virtual-ips/)) - so different public addresses can forward the same port to different servers. |
| **Protocol** | TCP, UDP or TCP/UDP. |
| **External port** | **Port or range** (`8443`, `5000:5010`) or **Port alias**: the port the outside world connects to. |
| **Target IP address** | The internal host that receives the traffic. |
| **Target port** | The port on the target. Empty: the same as the external port. For a range, the range is shifted to start here. A target port cannot be combined with a port alias. |
| **Allowed source** | **Any**, a single host or network, an alias or an interface network, with **Invert match** for *everyone except*. Restrict it whenever you can: a service open to the whole internet is found by scanners within hours. |
| **Description** | Your note. |
| **Log connections that match** | Log every connection through the forward. |
| **Port forward is enabled** | Switch the forward off without deleting it. |

Example: **External port** `8443`, **Target IP address** `192.168.1.10`, **Target port** `443` - a visitor opening
`https://<your public address>:8443` reaches the web server in the LAN.

Click **Save**, **Apply changes**, **Keep changes**.

### Order and separators

The first matching forward wins; move entries with their arrows or drag them by their handle. Separators group
the list without changing anything. Grey entries at the top are redirects Stratum FW writes from other settings:
*Force DNS* - the DNS queries of networks that must use the firewall's DNS (**Services > DNS Resolver**,
**Diagnostics > Web Activity**).

### Restricting the source

A forward with **Allowed source** set to an alias - for example `Office_IPs` with the addresses of your offices
and partners - is reachable only from those addresses; everyone else is blocked as if the port were closed. For a
service that only a few known places need (remote desktop, a recorder, an SSH server), this is the single most
effective protection there is.

### NAT reflection

Devices in the LAN that open the *public* address of a forward (for example `https://cloud.example.com`, which
points to your WAN address) do not reach the server by default. Switch on **NAT reflection: local devices can use
the public address of a port forward** under **System > Settings > Firewall** to make this work. The better
alternative, when you can, is a DNS override (**Services > DNS Resolver**) that resolves the name to the server's
internal address for local devices.

## Outbound NAT

Outbound NAT rewrites the source address of traffic leaving the firewall, so that internal devices with private
addresses can share the public address of the WAN.

![Firewall > NAT > Outbound](/images/manual/nat-outbound.png)

### Mode

| Mode | Meaning |
| :--- | :--- |
| **Automatic** | The networks of all internal interfaces, and the networks routed behind them ([**Interfaces > Routes**](/manual/interfaces-routes/)), are translated to the address of each WAN they leave through. The default; right for almost every installation. |
| **Hybrid** | Your rules first, then the automatic rules. Use it to make exceptions while keeping everything else automatic. |
| **Manual** | Only your own rules are used. Anything you do not list is not translated - and does not reach the internet. If the list is empty when you switch to Manual, it is filled from the automatic rules, so nothing breaks at once. |
| **Disabled** | No outbound translation at all - for a firewall between networks with public or routed addresses. |

Choose the mode and click **Save mode**. **Default rules (automatic)** below shows what the automatic mode
generates; it is updated on its own when interfaces change.

### Outbound rules (Hybrid and Manual)

| Field | Meaning |
| :--- | :--- |
| **Interface** | The interface the traffic leaves through - normally WAN. |
| **Source**, **Destination** | Any, a single host or network, an alias or an interface network, with **Invert match**. |
| **Translate to** | **Interface address** - the current address of the interface; **Specific address** - a fixed public address, for example a virtual IP from an additional block. |
| **Description**, **Rule is enabled** | As usual. |

Rules are evaluated from top to bottom; the first match wins.

Typical use: a mail server must send from the address its DNS records name. Add a virtual IP for that address,
switch to **Hybrid**, and add a rule: source the mail server, translate to the virtual IP. All other devices keep
using the WAN address.

## 1:1 NAT

1:1 NAT maps a public address to an internal one in both directions: the internal host goes out with the public
address, and traffic to the public address reaches the host.

| Field | Meaning |
| :--- | :--- |
| **Interface** | The WAN the public address belongs to. |
| **External address** | A single address, or a network the provider routes to you (`203.0.113.16/29`). A single address must be a virtual IP on the WAN. The WAN's own address cannot be used. |
| **Internal address** | A host, or a network of the same size as the external one (`192.168.1.16/29` for `203.0.113.16/29`). |
| **Description**, **Entry is enabled** | As usual. |

The list's **Incoming** column shows how many WAN pass rules lead to the internal address, or *closed* when there are none.

> **Warning:** 1:1 NAT opens nothing by itself. Add pass rules on the WAN with the **internal** address as
> destination, for exactly the ports the host should offer.

Port forwards take precedence for their ports; for its outgoing traffic, the host uses its public address
instead of the WAN address. On a second WAN, the host's outgoing traffic uses the public address only when it
leaves through that WAN - add a gateway (policy routing) to the host's rule on its internal interface.

