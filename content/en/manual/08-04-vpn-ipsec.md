---
title: "VPN > IPsec"
seo_title: "08.4 VPN > IPsec — Stratum FW User Manual"
description: "Site-to-site tunnels with IPsec (strongSwan): to FortiGate, MikroTik, Cisco, pfSense/OPNsense and cloud gateways - identities, preshared key, networks, phase 1 and phase 2, status, connecting and the IPsec log."
weight: 804
chapter_num: "08.4"
url: "/manual/vpn-ipsec/"
aliases:
  - "/manual/08-04-vpn-ipsec/"
translationKey: "manual-08-04-vpn-ipsec"
---

IPsec is the standard for site-to-site VPNs: almost every firewall and router speaks it - FortiGate, MikroTik,
Cisco, Palo Alto, pfSense, OPNsense, and the VPN gateways of the cloud providers. Use it when the other side does
not support WireGuard, or when a partner prescribes IPsec.

Stratum FW uses **strongSwan** with IKEv2 or IKEv1 and a **preshared key**. The tunnels are **policy based**: the
traffic between the local and the remote networks is encrypted; everything else is routed as before.

![VPN > IPsec](/images/manual/ipsec.png)

## Switch IPsec on

| Setting | Meaning |
| :--- | :--- |
| **Enable IPsec (tunnels to other firewalls and routers)** | Starts the IPsec service. Tunnels can be prepared while it is off. |
| **TCP MSS limit in tunnels** | Default 1350. Keeps large packets from being fragmented after encryption. 0 = off. |

Click **Save**.

The firewall rules are made for you: IKE (UDP 500 and 4500) and ESP from the other side are allowed on its WAN,
tunnel traffic is never translated by NAT, and traffic from the remote networks is allowed as each tunnel says.

## Add a tunnel

**Add tunnel**. Both sides must agree on five things: the **IKE version**, the **preshared key**, the **phase 1**
and **phase 2** settings, and the **networks**, mirrored.

### Other side

| Field | Meaning |
| :--- | :--- |
| **Name**, **Description** | For example *Partner Istanbul*. |
| **Tunnel enabled** | A disabled tunnel keeps its settings. |
| **Remote gateway** | The other side's public IPv4 address or host name. **`any`** accepts a side with a changing address: the tunnel then only waits for it and needs a **Remote ID**. |
| **Our side (WAN)** | The WAN interface the tunnel uses. |
| **Remote ID** | The identity the other side sends - FortiGate: *Local ID*; MikroTik: *my-id*. Empty: its address. Required with a host name or `any`. |
| **Local ID** | The identity this side sends. Empty: the WAN address. **Behind NAT** our own address is the private one - set an ID the other side expects. |
| **Connection** | **Connect at start and keep it up**, or **Wait for the other side (respond only)**. |

### Preshared key

The same key on both sides: at least 8 characters, 20 or more recommended. **Generate** makes a strong one;
**Show** displays what you typed, so you can copy it.

The key is stored in a root-only file and is never shown again after saving. When editing a tunnel, leave the field
empty to keep the saved key.

### Networks

| Field | Meaning |
| :--- | :--- |
| **Local networks** | Internal interfaces (with a static address) whose networks join the tunnel. |
| **Other local networks** | Networks behind a router in your local network, entered by hand. |
| **Remote networks** | The networks behind the other side. `0.0.0.0/0` is not supported. |
| **Allow traffic from the remote networks to the local networks** | On: devices on the other side can open connections to your networks. Off: only connections started from your side. |
| **One phase 2 entry per network pair** | One security association for each pair of a local and a remote network - needed for FortiGate, Cisco and other devices with one entry per subnet. Always on with IKEv1. |

At most 16 local and 16 remote networks per tunnel. Remote networks must not overlap your networks or another
tunnel's.

### Phase 1 (IKE)

| Setting | Default | Choices |
| :--- | :--- | :--- |
| **IKE version** | IKEv2 | IKEv2, or IKEv1 for older devices. Use IKEv2 when both sides support it. |
| **IKEv1 mode** | Main mode | **Aggressive mode** only if the other side requires it - weaker with a preshared key. |
| **Encryption** | AES-256-CBC | AES-256-GCM, AES-128-GCM, AES-256/192/128-CBC |
| **Integrity / PRF** | SHA-256 | SHA-256, SHA-384, SHA-512, SHA-1 (legacy) |
| **DH group** | 14 (MODP 2048) | 14, 15, 16, 19, 20, 21, 31 (Curve25519); 5 and 2 are weak, only for old devices |
| **Lifetime (seconds)** | 28800 | |

### Phase 2 (ESP)

| Setting | Default | Choices |
| :--- | :--- | :--- |
| **Encryption** | AES-256-CBC | as in phase 1 |
| **Integrity** | SHA-256 | as in phase 1; not used with GCM, which has its own |
| **PFS group** | 14 (MODP 2048) | Off, or a DH group - the same on both sides |
| **Lifetime (seconds)** | 3600 | |

**Dead peer detection (seconds)** - default 30: how often the other side is checked when there is no traffic; a
dead tunnel is then reconnected. 0 = off.

**Proposals sent** shows the exact proposal strings - handy to compare with the other side's configuration.

Click **Save**, then **Apply changes** and **Keep changes**.

## Matching the other side

The networks are **mirrored**: our local networks are their remote networks, and the other way round.

**FortiGate:** *VPN > IPsec Wizard*, template **Custom**:
- the same IKE version (the wizard's default is 1; version 2 is recommended);
- phase 1 and phase 2 proposals as here, PFS with the same DH group;
- its *Local Address* = our remote networks, its *Remote Address* = our local networks;
- turn on **One phase 2 entry per network pair** here when it has one phase 2 selector per subnet.

**MikroTik (RouterOS 7):** `/ip ipsec profile`, `proposal`, `peer` (`exchange-mode=ike2`), `identity` (the
secret) and a `policy` per network pair. Exclude the tunnel traffic from masquerade with an accept rule above it
in `/ip firewall nat` - otherwise it is translated and never matches the policy.

**pfSense / OPNsense:** a phase 1 with the same settings and a phase 2 entry per network pair.

**Behind NAT:** NAT traversal (UDP 4500) is automatic. The side behind NAT should connect; the other can wait.

## Status and control

The tunnel list shows each tunnel's **Remote gateway**, **Networks**, **Status** and the traffic **Received** and
**Sent**:

| Status | Meaning |
| :--- | :--- |
| **connected** | Phase 1 and at least one phase 2 entry are up. |
| **phase 1 only** | The two sides authenticated, but the networks (phase 2) did not agree. |
| **connecting** | Negotiation in progress. |
| **not connected** | The tunnel is down. |
| **disabled** | The tunnel is switched off. |

**Connect** and **Disconnect** start or stop a tunnel now - without changing the configuration. The [dashboard's](/manual/dashboard/) **VPN connections** panel shows the IPsec tunnels too.

## The IPsec log

**IPsec log** at the bottom of the page shows strongSwan's messages - the reason a tunnel does not come up is
almost always there:

| Message | Means |
| :--- | :--- |
| `NO_PROPOSAL_CHOSEN` / *no proposal chosen* | Phase 1 or phase 2 settings differ (encryption, integrity, DH or PFS group). |
| *authentication failed* / `AUTHENTICATION_FAILED` | Preshared key or the IDs differ. |
| `TS_UNACCEPTABLE` | The networks differ - check the mirroring and the per-pair option. |
| No answer at all | The other side is unreachable, or UDP 500/4500 is blocked on the way. |

## IPsec or WireGuard?

Between two firewalls that both speak WireGuard (Stratum FW, OPNsense, pfSense, MikroTik 7), [**WireGuard S2S**](/manual/vpn-wireguard-s2s/) is
simpler - two keys and the networks, nothing to negotiate. Use IPsec for everything else.

> **Note:** IPsec tunnels are not taken over by the OPNsense importer; set them up again here.
