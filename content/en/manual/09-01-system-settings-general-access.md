---
title: "System > Settings: General and Admin Access"
seo_title: "09.1 System > Settings: General and Admin Access — Stratum FW User Manual"
description: "Host name, local domain, time zone, sign-in notice and start-up sound; the web panel port and session timeout, who may manage the firewall, anti-lockout, access from the WAN, sign-in protection and SSH."
weight: 901
chapter_num: "09.1"
url: "/manual/system-settings-general-access/"
aliases:
  - "/manual/09-01-system-settings-general-access/"
translationKey: "manual-09-01-system-settings-general-access"
---

**System > Settings** holds the firewall's own settings in nine tabs: **General**, **Admin Access**, **Firewall &
NAT**, **Logging**, **Notifications**, **Networking**, **Tunables**, **SNMP** and **MCP**. This chapter covers the
first two; the others follow in the next chapters.

Only administrators can change settings.

## General

![System > Settings > General](/images/manual/set-general.png)

| Setting | Meaning |
| :--- | :--- |
| **Hostname** | The firewall's name, shown at the top of every page and in the console menu. Give each firewall a distinct name, for example `fw-belek`. Default `stratum`. |
| **Local domain** | The domain of your internal networks (default `lan`). Devices are reachable as `name.<domain>`. The same setting as on [**Services > DNS Resolver**](/manual/services-dns-resolver/). |
| **Time zone** | Used for logs, graphs and schedules. The time servers are on [**Services > NTP**](/manual/services-ntp/). |
| **Sign-in notice** | A text shown on the sign-in page, for example a legal notice - *Authorised access only. All activity is logged.* Up to 1000 characters; empty for none. Takes effect immediately. |
| **Play a sound at start-up and shutdown** | A short tune on the firewall's PC speaker when it has started, and its closing version when it shuts down or restarts - handy in a server room to know the firewall is up. Tunes: *Stratum*, *Classic beep*, *Chime (Westminster)*, *Ode to Joy*, *Für Elise*, *Arcade*. **Test** plays it on the firewall, not in the browser. Machines without a speaker stay silent. |

Host name, local domain and time zone become active after **Apply changes**; the host name changes without a
reboot.

## Admin Access

![System > Settings > Admin Access](/images/manual/set-access.png)

This tab decides how - and from where - the firewall can be managed. A mistake here can lock you out, so the
panel protects you in three ways:

- **Saving is refused** when the new settings would no longer let you in from the address you are using now.
- Changes take effect with **Apply changes** and are **reverted automatically** unless you click **Keep changes**
  in time - if the panel becomes unreachable, wait and it comes back by itself.
- On the console, menu option **6) Management access** > **2) Reset management access to the defaults** restores
  port 443, all internal networks, anti-lockout on and SSH on with password (WAN access and SSH keys stay as they
  are). Option **1** of the same menu opens or closes WAN access.

### Web panel

| Setting | Meaning |
| :--- | :--- |
| **HTTPS port** | Default 443. When it changes, the apply bar shows the new address. Ports used by services (DNS, DHCP, NTP ...) are refused. |
| **Sign out after inactivity** | 15 or 30 minutes, 1, 4 (default) or 8 hours, or 1 day. Pages that refresh themselves (the dashboard) do not count as activity. Takes effect immediately. |
| **Answer plain HTTP (port 80) with a redirect to the panel** | `http://firewall` leads to `https://firewall`. On by default. |

The panel's certificate is on [**System > Certificates**](/manual/system-certificates/).

### Who may manage the firewall

| Setting | Meaning |
| :--- | :--- |
| **From internal networks** | **All internal networks** - the firewall rules of each interface decide (default); or **Only the selected interfaces** - the panel and SSH are refused from every other interface, whatever its rules allow. Use this to keep guests and IoT networks away from the panel. |
| **Anti-lockout: always allow the panel and SSH from LAN** | A rule before all others that keeps LAN able to manage the firewall, even if you make a mistake in the LAN rules. On by default; leave it on until you are sure of your rules. |
| **Allow the panel and SSH from WAN (the internet)** | Off by default. |
| **Only from these addresses or networks** | With WAN access on: the addresses that may connect, one per line. **Strongly recommended** - a panel open to the whole internet is scanned within minutes. Better still: leave WAN access off and use the VPN. |

The page shows the **ports in use**. VPN clients reach the panel according to the VPN settings (the networks they
may reach).

### Sign-in protection

**Block an address after** *5* **failed sign-ins within** *5* **minutes, for** *15* **minutes** (the defaults;
3-20 attempts, 1-60 and 1-1440 minutes). It applies to the web panel - and to MCP tokens - and takes effect
immediately. A blocked address can be reported by Telegram or e-mail ([**Notifications**](/manual/system-settings-logging-notifications/), event *An address was
blocked after failed sign-ins*).

Users can also protect their own account with two-factor authentication ([**System > Users**](/manual/system-users/)).

### SSH

| Setting | Meaning |
| :--- | :--- |
| **Enable SSH** | Console access over the network, user `root`. On by default; type `stratum-menu` after sign-in for the [console menu](/manual/console-menu/). |
| **Port** | Default 22. |
| **Sign in with** | **Password or key**, or **Key only (recommended)**. |
| **Authorised public keys** | One public key per line - the contents of `id_ed25519.pub` (or another `.pub` file). **Never paste a private key.** |

To use a key: on your computer run `ssh-keygen -t ed25519`, paste the contents of `~/.ssh/id_ed25519.pub` here,
save and apply, check that `ssh root@<firewall>` works with the key, and only then switch to **Key only**.

**Save** saves every section on the page.
