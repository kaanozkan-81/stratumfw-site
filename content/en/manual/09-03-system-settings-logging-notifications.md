---
title: "System > Settings: Logging and Notifications"
seo_title: "09.3 System > Settings: Logging and Notifications — Stratum FW User Manual"
description: "Firewall log rotation, sending logs to a syslog server for long-term or legal retention, and notifications by Telegram and e-mail: setup, events and thresholds."
weight: 903
chapter_num: "09.3"
url: "/manual/system-settings-logging-notifications/"
aliases:
  - "/manual/09-03-system-settings-logging-notifications/"
translationKey: "manual-09-03-system-settings-logging-notifications"
---

## Logging

![System > Settings > Logging](/images/manual/set-logging.png)

### Logs on this firewall

**Firewall log: start a new file at** *5* **MB and keep** *3* **old files** (compressed). Raise both if you want
more history on the firewall itself; the disk usage is shown on the [dashboard](/manual/dashboard/).

The logs are read on [**Diagnostics > System Logs**](/manual/diagnostics-system-logs/) and [**Diagnostics > Firewall Log**](/manual/diagnostics-firewall-log/).

### Send logs to a syslog server

For a NAS or a log server that keeps logs for a long time - for example **records that must be kept for legal
reasons**, such as for hotel and other public guest networks. Up to two servers.

For each server:

| Field | Meaning |
| :--- | :--- |
| **Server** | IP address or host name. |
| **Port** | Default 514 (UDP). The server must accept syslog on this port. |
| **What to send** | **System** (all services, notice and above); **Sign-ins** (panel, SSH, VPN); **Firewall log** (every logged packet); **DHCP and DNS** (leases, queries). |

**Send test message** works at once, before Apply: it proves that the server receives syslog from the firewall.
The other changes take effect with **Apply changes**.

On the server, the firewall log arrives as one readable line per packet with the tag `filterlog` (facility
`local5`); DHCP and DNS with facility `local6`. The logs on the firewall are kept as before.

> **Tip - captive portal and web activity:** for a guest network, send **Sign-ins**, **Firewall log** and **DHCP
> and DNS**. Together they record which device (IP and MAC address) had which address, who signed in on the portal,
> and which connections it made.

## Notifications

![System > Settings > Notifications](/images/manual/set-notify.png)

The firewall can report problems by **Telegram** and/or **e-mail**.

### Telegram

1. In Telegram, talk to **@BotFather**, create a bot (`/newbot`) and copy its **token**.
2. Send any message to your new bot once (a bot cannot write to you first).
3. Get your **chat ID**, for example from **@userinfobot**. For a group: add the bot to the group; group IDs start
   with `-100`. A channel can be given as `@channelname`.
4. Switch on **Send notifications with a Telegram bot**, enter **Bot token** and **Chat ID**, and click **Send test
   message**.

### E-mail

| Field | Meaning |
| :--- | :--- |
| **Mail server (SMTP)**, **Port** | For example `smtp.office365.com`, 587. |
| **Security** | **STARTTLS (587)**, **TLS (465)**, or **None (25, local relay)**. |
| **User name**, **Password** | Empty when the server needs no sign-in. |
| **Sender address**, **Recipients** | One or more recipients. |

**Send test message** checks the settings at once.

The bot token and the mail password are stored on the firewall only (readable by root) and are never shown again.
Leave the field empty to keep the saved one.

### What to report

| Event | On by default |
| :--- | :--- |
| A software update is available | yes |
| Applying the configuration failed | yes |
| Changes were reverted automatically (not kept in time) | yes |
| WAN connection down, up again, or new address | yes |
| An address was blocked after failed sign-ins | yes |
| The panel certificate expires within 21 days | yes |
| IP or DNS blocklist downloads fail | yes |
| Disk almost full - at **90** % | yes |
| CPU temperature high - at **85** °C | yes |
| High-severity intrusion detection alerts (from inside, to inside hosts, or to a port the firewall opens) | yes |
| ... also those against WAN ports the firewall does not open (scans it drops anyway) | no |
| The automatic backup to the server (SFTP or SMB) failed | yes |

The firewall checks WAN, disk, temperature, certificate, blocklists and updates every 5 minutes; failed applies,
automatic rollbacks and blocked sign-ins are reported at once. The same problem is reported again only after a
pause (disk and blocklists daily, temperature every 6 hours); WAN changes every time they happen.

**Save** takes effect at once.
