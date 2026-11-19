---
title: "Services > Captive Portal"
seo_title: "07.6 Services > Captive Portal — Stratum FW User Manual"
description: "A sign-in page for guest networks: zones, sign-in without a password, with local users or a RADIUS server, accounting, time and idle limits, per-device bandwidth, the page itself, exceptions, signed-in devices and how it works."
weight: 706
chapter_num: "07.6"
url: "/manual/services-captive-portal/"
aliases:
  - "/manual/07-06-services-captive-portal/"
translationKey: "manual-07-06-services-captive-portal"
---

The captive portal puts a sign-in page in front of a network: a device that joins the guest Wi-Fi reaches nothing
but the page until it signs in - by accepting the terms of use, with a user name and password, or with an account
on your RADIUS server. After sign-in the network's normal firewall rules apply to it.

Typical uses: hotel and café guest Wi-Fi, staff accommodation, conference rooms, public networks that must record
who used them.

**Services > Captive Portal** has three tabs: **Zones**, **Signed-in devices** and **Users**.

![Services > Captive Portal](/images/manual/cp-zones.png)

## How it works

- A **zone** is one or more internal interfaces. Every device in those networks must sign in.
- Before sign-in a device reaches **DNS on the firewall**, **the sign-in page** and the zone's **allowed
  destinations** - nothing else. Its plain web requests (`http://`) are sent to the page; `https://` connections
  are refused.
- Phones and laptops check whether they are online by loading an `http://` address. They are sent to the page and
  open it by themselves ("Sign in to network"). On a laptop, opening any `http://` site shows the page.
- After sign-in, the device's address is let through, and the interface's own firewall rules apply - for example
  *guests may reach the internet, not the internal networks* ([**Firewall > Rules**](/manual/firewall-rules/)).
- Sessions end with the zone's time limit, after the idle time, when another device takes the address (the MAC
  address is checked), when the device signs out, or when an operator disconnects it.

The sign-in page runs as an unprivileged service on port 8002 of the firewall's address in the zone. It knows
nothing secret: sign-ins are checked by a separate service, and passwords and RADIUS secrets never reach the page.

> **Before you start:** the DHCP server of the zone's network must give the **firewall as DNS server** (leave
> **DNS servers for clients** empty on [**Services > DHCP Server**](/manual/services-dhcp-server/)). Devices that cannot resolve names before
> signing in never see the page; the panel refuses a zone where this is not so.

## Add a zone

**Zones > Add zone** (administrators). The page is long; the sections are described in order.

### Name and networks

| Field | Meaning |
| :--- | :--- |
| **Name** | For example *Guests*. 1-40 letters, digits, spaces, `.`, `_` or `-`. |
| **Description** | Optional. |
| **Zone enabled** | A disabled zone keeps its settings but controls nothing: its networks work as if there were no portal, with their firewall rules only. |
| **Interfaces** | The internal networks of the zone (with a static address). A network can be in one enabled zone only. |

### Sign-in

| Field | Meaning |
| :--- | :--- |
| **Method** | **No password - accept the terms and connect**; **Local users** (the **Users** tab); or **RADIUS server**. |
| **Devices per user** | (Local users, RADIUS.) How many devices one account may have signed in at the same time. 0 = no limit. |
| **Time limit (minutes)** | A device is signed out this long after it signed in. 0 = none; 1440 = one day. |
| **Idle time (minutes)** | A device is signed out after this long without traffic. 0 = never. |
| **Download / Upload per device (Mbit/s)** | A bandwidth limit for every signed-in device on its own. 0 = no limit. |
| **Page after sign-in** | Optional `http://` or `https://` address to show after sign-in. Empty: the page the device originally asked for. |

When the method of a zone changes, devices signed in the old way are signed out and sign in again the new way.

### RADIUS

With **RADIUS server**, the portal asks your RADIUS server (FreeRADIUS, Microsoft NPS and others) whether a user
name and password are valid - the same way a MikroTik or another hotspot would.

| Field | Meaning |
| :--- | :--- |
| **RADIUS server** | Its address or host name. |
| **Second server** | Optional; used when the first does not answer. |
| **Authentication port**, **Accounting port** | Default 1812 and 1813. |
| **Shared secret** | The same secret as this firewall's client entry on the RADIUS server; 8-128 printable characters without spaces. It is stored in a root-only file and never shown again - the field says when one is saved; leave it empty to keep it. |
| **NAS identifier** | How the firewall names itself to the server. Empty: its host name. |
| **Send accounting** | Start, periodic updates and Stop records for every session. |
| **Accounting updates (minutes)** | How often a running session is reported. 0 = only start and stop. |

**On the RADIUS server**, add the firewall as a client with its address *as the server sees it* and the shared
secret. For FreeRADIUS, an entry in `clients.conf`:

```
client stratum-fw {
    ipaddr = 192.168.1.1
    secret = <the shared secret>
}
```

**What the server can send back**, and the portal applies:

| Attribute | Effect |
| :--- | :--- |
| `Session-Timeout` | The device's time limit (wins over the zone's). |
| `Idle-Timeout` | The device's idle time (wins over the zone's). |
| `Acct-Interim-Interval` | How often accounting updates are sent (wins over the zone's). |
| `WISPr-Bandwidth-Max-Down` / `-Up` | Bandwidth limit for the device (bit/s). |
| `Mikrotik-Rate-Limit` | Bandwidth limit in MikroTik's notation (`upload/download`, for example `2M/10M`) - an existing MikroTik setup works unchanged. |
| `Class` | Sent back in the accounting records. |
| `Reply-Message` | Written to the log when the server rejects a sign-in. |

**Accounting records** carry the user name, the device's IP and MAC address (`Calling-Station-Id`), the zone
name (`Called-Station-Id`), the session time, the traffic and the reason the session ended (User-Request,
Idle-Timeout, Session-Timeout, Admin-Reset, Lost-Carrier ...). The traffic is counted from the device's connections
once a minute - a close lower bound of the real figure.

Every request carries a Message-Authenticator, and every answer is checked against the shared secret before it
is believed - an answer with a wrong secret is treated as no answer.

### The page

| Field | Meaning |
| :--- | :--- |
| **Page language** | **English** or **Türkçe** - the page's own words (*User name*, *Connect* ...). Your texts below are shown as written. |
| **Colour** | The accent colour of the page. |
| **Title** | For example *Welcome to Hotel Example*. |
| **Button text** | Empty: *Connect*. |
| **Message** | A short text under the title, for example *Free Wi-Fi for our guests.* |
| **Terms of use** | When set, the device must tick *I accept the terms of use* to connect. |
| **Logo** | PNG, JPEG or GIF, at most 300 KB. Available after the zone is saved; it changes on the page at once, without Apply. |

The page adapts to phones and to dark mode.

![The sign-in page](/images/manual/cp-page.png)

### Exceptions

| Field | Meaning |
| :--- | :--- |
| **Devices that never sign in** | One IPv4 address per line - printers, TV boxes, payment terminals. Give them a static DHCP mapping so their address does not change. |
| **Destinations allowed before sign-in** | One IPv4 address or network per line (at least /8) - for example the hotel's web site or a booking server that the page links to. |

Save the zone, then **Apply changes** and **Keep changes**.

## Users

The **Users** tab holds the accounts for zones that sign in with **Local users**. Operators can manage them too -
typically the front desk.

**Add user**:

| Field | Meaning |
| :--- | :--- |
| **User name** | Letters, digits, `.`, `_`, `@` and `-`, up to 64 characters - for example `room214`. |
| **Description** | For example *Room 214, until Friday*. |
| **Password** | 6-128 characters. **Generate** creates one - give it to the user right away; after saving it is not shown again. |
| **Expires** | Optional: the account works until the end of this day. |
| **User enabled** | Switch an account off without deleting it. |

User changes take effect at once, without Apply. Disabling, renaming or deleting a user, or changing its password,
signs its devices out. Passwords are stored as bcrypt hashes in a root-only file and are never sent to the
browser.

## Signed-in devices

The **Signed-in devices** tab lists every signed-in device: **Address**, **MAC**, **Zone**, **User** (with a
*RADIUS* badge for RADIUS sign-ins), when it **Signed in**, its **Last activity**, its **Traffic** and its
bandwidth limit. **Disconnect** signs a device out at once (operators and administrators).

Signing a device out closes its internet connections, but not its connections to the firewall itself - an
administrator whose laptop is in a zone keeps the panel open.

The [dashboard](/manual/dashboard/) panel **Captive portal** shows the devices per zone, the latest sign-ins and the state of the
service.

## Logs and records

Every sign-in, failed sign-in and sign-out is written to the system log (**Diagnostics > System Logs**, program
`stratum-captive`), with the zone, user, IP and MAC address, and for sign-outs the reason and the duration:

```
captive: sign-in zone=Guests user=room214 ip=10.20.0.57 mac=7c:4d:8f:06:9b:c8
captive: sign-out (idle) zone=Guests user=room214 ip=10.20.0.57 mac=7c:4d:8f:06:9b:c8 minutes=95
```

With a RADIUS server, its accounting records hold the same information in its own database.

**Protection against guessing:** 5 wrong passwords from one address within 5 minutes lock that address out for 5
minutes.

## Troubleshooting

| Symptom | Check |
| :--- | :--- |
| The page does not appear | The device got the firewall as DNS server; open an `http://` address such as `http://neverssl.com` (an `https://` site cannot show the page). |
| *The sign-in service is not available right now* | [**Services > Service Status**](/manual/services-service-status/): *Captive portal* must be running. With RADIUS: the server is reachable and answers (the system log says *RADIUS did not answer*). |
| RADIUS sign-ins are rejected | The log line says *sign-in rejected by RADIUS*: user name or password, or the server's own policy. If the log says *RADIUS did not answer ... the answer's authenticator is wrong*, the shared secrets on the two sides differ. |
| A device is signed out again and again | Its idle time is short and it is quiet (a phone asleep), or it uses a random MAC address that changes. |
| Signed in, but no internet | The interface's firewall rules ([**Firewall > Rules**](/manual/firewall-rules/)) must allow the traffic; the portal only decides who is signed in. |
