---
title: "Initial Configuration"
seo_title: "02. Initial Configuration — Stratum FW User Manual"
description: "The first sign-in, securing your account with two-factor sign-in, what the setup wizard already configured, the internet connection (static, DHCP, PPPoE), DNS and the first checks."
weight: 2
chapter_num: "02"
url: "/manual/initial-configuration/"
aliases:
  - "/manual/02-initial-configuration/"
translationKey: "manual-02-initial-configuration"
---

The setup wizard leaves you with a working firewall: the LAN reaches the internet and nothing reaches the LAN
from outside. This chapter walks through the first visit to the panel - signing in, securing your account,
the basic system settings, the internet connection and DNS - and ends with a checklist of the things worth
setting before the firewall goes into service.

## Sign in

From a computer in the LAN, open the address shown on the console as **Web panel**, for example
`https://192.168.1.1/`.

**The certificate warning.** On a new installation the panel uses a certificate the firewall created for
itself, so the browser shows a warning such as *Your connection is not private*. This is expected: choose
*Advanced* and continue to the address. The warning goes away when you install a certificate of your own under
**System > Certificates** (or tell your browser to trust this one).

Sign in with the user `admin` and the password you set in the setup wizard.

![The sign-in page](/images/manual/initial-signin.png)

- **Invalid username or password.** The user name or the password is wrong. User names are case-sensitive.
- **Too many failed attempts from this address. Try again later.** Sign-in protection has blocked your address:
  by default 5 failed attempts within 5 minutes block it for 15 minutes. Wait, or unblock the address from
  another computer under **System > Settings > Admin Access**. Blocks can also be sent as notifications.
- If two-factor sign-in is on for the user, a second page asks for the **Code** from the authenticator app (see
  below).

A text set under **System > Settings > General > Sign-in notice** - for example a legal notice - is shown on
the sign-in page.

## Secure your account

Click your user name at the top right to open **Your account**.

![The Account page](/images/manual/initial-account.png)

### Change your password

Enter the **Current password**, then the **New password** twice. Passwords must be at least 10 characters.
Changing the password keeps you signed in on this browser and signs you out on every other device.

### Two-factor sign-in

With two-factor sign-in, a stolen password alone is not enough: after the password, the sign-in asks for a
6-digit code from an authenticator app on your phone - Google or Microsoft Authenticator, 1Password, Bitwarden
or any other app for time-based codes (TOTP). We strongly recommend it for every administrator.

1. Under **Two-factor sign-in**, enter **Your password** and click **Set up two-factor sign-in**.
2. Scan the QR code with the authenticator app - or type the **Key** shown beside it. The QR code is drawn by
   the firewall itself; the key never leaves it.
3. Enter the 6-digit **Code from the app** and click **Switch on**.
4. The panel shows **10 recovery codes** (such as `3f9a-1c07-b2e4`). Save them now - they are shown only once.
   Each code signs in one time when the phone is not at hand. Click **I have saved them**.

Switching two-factor sign-in on signs you out on every other device.

When it is on, the card shows how many recovery codes are left. With **Your password** and a **Current code**
you can get **New recovery codes** (the old ones stop working) or **Switch off** two-factor sign-in.

> **Lost phone:** sign in with a recovery code. Without one, another administrator can switch two-factor sign-in
> off for you on **System > Users**, or - with access to the console - menu option **3) Reset admin password**
> sets a new password and offers to switch it off.

## What is already configured

The setup wizard and the built-in defaults leave the firewall in this state:

| Area | State after the setup wizard | Where it is changed |
| :--- | :--- | :--- |
| Interfaces | WAN, LAN and any OPT interfaces you assigned, with the addresses you entered | **Interfaces > Assignments** |
| Firewall rules | Every interface assigned in the wizard has one rule: **Default allow ... to any** - its network may reach anywhere. Interfaces added later start without rules: everything from them is blocked until you add rules. | **Firewall > Rules** |
| From the internet | Everything coming in on the WAN is blocked, except replies to connections from inside. The firewall answers ping on its WAN address. Traffic from bogon addresses (ranges that cannot appear on the internet) is dropped. | **Firewall > Rules**, **System > Settings > Firewall** |
| NAT | Automatic outbound NAT: every internal network reaches the internet through the WAN address. | **Firewall > NAT** |
| Panel access | HTTPS on port 443; port 80 redirects to HTTPS. From the internal networks, their firewall rules decide; an *anti-lockout* rule always lets the LAN reach the panel, whatever the other rules say. Closed from the WAN unless you opened it in the wizard. | **System > Settings > Admin Access** |
| SSH | On, port 22, `root` with the password set in the installer; reachable like the panel | **System > Settings > Admin Access** |
| DHCP | On for the LAN (and for any OPT where you switched it on), with the range you entered, leases of 12 hours | **Services > DHCP Server** |
| DNS | The firewall answers DNS for its networks (dnsmasq) and forwards queries to `1.1.1.1` and `8.8.8.8` | **Services > DNS Resolver** |
| Names | Host name `stratum`, local domain `lan` | **System > Settings > General** |
| Time | The time zone you chose; the clock is set from the internet (`pool.ntp.org`) | **System > Settings > General**, **Services > NTP** |

## Basic system settings

Open **System > Settings**, tab **General**.

![System > Settings > General](/images/manual/initial-general.png)

| Setting | What it does |
| :--- | :--- |
| **Hostname** | The firewall's name: lower-case letters, digits and `-`, at most 63 characters. It is shown in the panel, on the console, in notifications and in logs sent to other servers. Give each firewall a name of its own, such as `fw-office`. |
| **Local domain** | The domain of your internal networks (default `lan`). Devices that get an address from the DHCP server can be reached by name as `name.<domain>`, and queries for this domain never leave the firewall. If you have an internal domain (for example an Active Directory domain), see the DNS chapter before you change this. |
| **Time zone** | Used for logs, graphs, schedules and reports. |
| **Sign-in notice** | Text shown on the sign-in page, for example a legal notice. Takes effect immediately; leave it empty for none. |
| **Play a sound at start-up and shutdown** | A short tune on the PC speaker of the firewall when it has started and when it shuts down - handy for appliances without a screen. **Test** plays it on the firewall, not in the browser. |

Click **Save**, then **Apply changes** in the bar at the top, and **Keep changes**.

## The internet connection

The wizard sets the WAN to DHCP or to a fixed address. To change it, or to set up PPPoE, open
**Interfaces > Assignments** and edit the **WAN**.

![Editing the WAN interface](/images/manual/initial-wan.png)

If you are connected to the panel through the interface you are editing, the page reminds you that a change
which breaks the connection is reverted 120 seconds after **Apply changes**.

**Description** is the name shown for the interface everywhere in the panel (for example *Fibre*). The WAN is
always enabled.

Choose the **IPv4 configuration**:

| Type | Use it when | Settings |
| :--- | :--- | :--- |
| **Static** | The provider gave you a fixed address. | **IPv4 address / prefix** - the firewall's own address with its prefix, for example `203.0.113.10/29`; **Gateway** - the provider's router, which must be inside that network. |
| **DHCP** | The address comes automatically: a modem or router in front of the firewall, a company or data-centre network. | None. |
| **PPPoE** | The firewall itself logs in to the provider - common for fibre and DSL when the provider's modem is in bridge mode. | **Username** and **Password** from the provider; **Service name** only if the provider asks for one (usually empty). |
| **None** | The port carries no IPv4 address of its own (for example it only carries VLANs). | None. |

**MTU** - leave it empty for the default: 1500, or 1492 for PPPoE. Change it only when the provider asks you to.

**About PPPoE.** The physical port carries the PPPoE session; the connection itself appears as a separate
`pppoe` interface, and firewall rules and NAT follow it automatically. The password is kept in a root-only file
and is never shown again; leave the field empty to keep it when you edit the interface later.

Click **Save**, **Apply changes** and - once the connection works - **Keep changes**. The WAN line on
**Interfaces > Assignments** and on the dashboard shows the address it received.

> **Tip:** A provider's modem that also routes (not in bridge mode) usually hands out an address by DHCP on its
> LAN side. Choose **DHCP** for the WAN, and make sure the firewall's LAN uses a different network from the
> modem's - for example `10.10.10.0/24` when the modem uses `192.168.1.0/24`.

A second internet line is added as another interface of type WAN; failover and load sharing between lines are
described in the chapter on **Interfaces > Gateways**.

## DNS

Devices in your networks ask the firewall for names; the firewall asks the internet. Open
**Services > DNS Resolver**.

![Services > DNS Resolver](/images/manual/initial-dns.png)

| Setting | What it does |
| :--- | :--- |
| **Local domain** | The same setting as on **System > Settings > General**. |
| **Upstream DNS servers** | The servers the firewall asks: up to four addresses, separated by commas. Default `1.1.1.1, 8.8.8.8`. |
| **Use only these upstream servers** | Ignore DNS servers learned in other ways (for example from a DHCP WAN). On by default. |
| **DNS resolver** | **dnsmasq** (DNS and DHCP in one service - the default) or **Unbound** (a validating resolver; dnsmasq keeps doing DHCP). |
| **Unbound mode** | **Forwarder** - ask the upstream servers above; **Resolver** - ask the internet's root servers directly, without a third-party DNS provider. |
| **Validate DNSSEC signatures** | With Unbound: refuse answers whose DNSSEC signature is wrong. |

> **Choose Unbound** if you want the DNS blocklists, the Web Filter or Web Activity - they all work inside
> Unbound. dnsmasq is the lighter choice for a firewall that only routes.

The same page can force the DNS queries of chosen networks to the firewall, so devices cannot bypass the
blocklists by using another DNS server. This and the other DNS settings (host and domain overrides) are covered
in the chapter on **Services > DNS Resolver**.

## Check that everything works

1. **From a computer in the LAN:** it should receive an address from the DHCP range, with the firewall as
   gateway and DNS server. Open a few web sites.
2. **On the dashboard:** the WAN shows an address, a gateway and a latency value.
3. **From the firewall:** **Diagnostics > Ping / Traceroute / DNS** - ping `1.1.1.1` (the connection) and look up
   a name such as `stratumfw.com` (DNS).
4. **The time:** the times in **Diagnostics > System Logs** match your clock. A wrong clock breaks certificates,
   two-factor codes and logs.

If the LAN has no internet: check the WAN address on **Interfaces > Assignments**, then
**Diagnostics > Firewall Log** for blocked packets, then the DNS lookup on the diagnostics page. The chapter
**Troubleshooting** goes through this step by step.

## Before the firewall goes into service

A short checklist - each item has its own chapter:

| | Item | Where |
| :--- | :--- | :--- |
| ☐ | Two-factor sign-in for every administrator | **Your account** |
| ☐ | A personal account for every person who manages the firewall - no shared `admin` password | **System > Users** |
| ☐ | Install the latest version | **System > Update** |
| ☐ | Download a backup, and set up automatic encrypted backups to your own server | **System > Backup** |
| ☐ | Notifications by Telegram or e-mail (WAN down, failed backups, blocked sign-ins ...) | **System > Settings > Notifications** |
| ☐ | Restrict who may reach the panel and SSH; SSH with keys only | **System > Settings > Admin Access** |
| ☐ | Rules for each internal network: what it may reach, instead of "allow to any" | **Firewall > Rules** |
| ☐ | A certificate of your own for the panel (optional) | **System > Certificates** |

