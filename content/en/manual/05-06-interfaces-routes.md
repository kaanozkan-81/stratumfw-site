---
title: "Interfaces > Routes"
seo_title: "05.6 Interfaces > Routes — Stratum FW User Manual"
description: "Static routes: reaching networks behind another router on a directly connected network, and what Stratum FW does for them automatically."
weight: 506
chapter_num: "05.6"
url: "/manual/interfaces-routes/"
aliases:
  - "/manual/05-06-interfaces-routes/"
translationKey: "manual-05-06-interfaces-routes"
---

A static route tells the firewall that a network is reached through a particular router. You need one when
there are networks the firewall is not directly connected to - for example:

- a second router on the LAN with more networks behind it (a building, a lab, a core switch that routes VLANs
  itself);
- a WAN-side network reached through a router other than the internet gateway, such as a provider's MPLS or a
  management network.

```
 Internet ── [ Stratum FW ] ── LAN 192.168.1.0/24 ── [ router 192.168.1.254 ] ── 10.50.0.0/24
                                                                                  10.60.0.0/24
 Routes:  10.50.0.0/24 via 192.168.1.254
          10.60.0.0/24 via 192.168.1.254
```

Networks reached through VPNs (OpenVPN, WireGuard, IPsec) need no static routes: the VPN settings add their own.

## The list

**Interfaces > Routes** shows each route with its **Network**, **Gateway**, the interface it goes **Via**, its
**Description** and **State**.

![Interfaces > Routes](/images/manual/routes-list.png)

## Add a route

Click **Add route**.

| Setting | Meaning |
| :--- | :--- |
| **Destination network** | The network behind the router, with its prefix: `10.50.0.0/24`. The host bits must be zero (`10.50.0.5/24` is refused). It must not overlap the network of an interface or another route. |
| **Gateway** | The router's address. It must be in the network of an interface with a **static** address, and it cannot be the firewall itself. The interface is found automatically and shown as **Via**. |
| **Route active** | Switch a route off without deleting it. |
| **Description** | Your note, for example *Building B core switch*. |

Click **Save**, **Apply changes** and **Keep changes**. The firewall sets the routes again on its own after a
reboot or an interface restart.

> **Note:** The default route (`0.0.0.0/0`) is not a static route: it comes from the WAN gateway and, with
> several WANs, from [**Interfaces > Gateways**](/manual/interfaces-gateways/).

## What happens automatically

For a network routed through a router on an **internal** interface:

- **Internet access:** its traffic is translated to the WAN address (automatic outbound NAT), like the
  interface's own network.
- **Policy routing:** rules that send traffic through a chosen WAN keep traffic *to* it local, like traffic to
  the interface networks.

What you still have to do:

- **Allow its traffic** with rules on the interface it *arrives* on - for the example above, on the **LAN** tab of
  **Firewall > Rules**, with the source `10.50.0.0/24` (or an alias of the routed networks). The interface's
  *Default allow LAN to any* rule covers only the LAN's own network.
- **Give the router a route back:** the router behind the LAN needs its default route pointing to the firewall
  (`192.168.1.1`).
- **DNS:** with Unbound as the DNS resolver, the firewall answers DNS only for its own interface and VPN networks.
  Give the devices in routed networks another DNS server (for example the router behind the LAN).

## Asymmetric routing

If devices in the LAN use the *router* as their gateway instead of the firewall, their traffic to the internet
goes LAN → router → firewall, but the answers come back firewall → LAN directly. A stateful firewall sees only
half of such a conversation and may drop it. Avoid it by making the firewall the devices' gateway and letting
the router reach the firewall over a separate *transfer* network (for example a small /30 or /29 between the two)
instead of the LAN.

There can be up to 256 static routes.

