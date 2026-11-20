---
title: "Diagnostics > System Logs"
seo_title: "10.2 Diagnostics > System Logs — Stratum FW User Manual"
description: "The logs of the system and the services: system, DHCP and DNS, PPPoE, NTP, authentication, web panel and configuration, with search and highlighted errors."
weight: 1002
chapter_num: "10.2"
url: "/manual/diagnostics-system-logs/"
aliases:
  - "/manual/10-02-diagnostics-system-logs/"
translationKey: "manual-10-02-diagnostics-system-logs"
---

[**Diagnostics > System Logs**](/manual/diagnostics-system-logs/) shows the logs of the firewall's services - the first place to look when something
does not work as expected.

![Diagnostics > System Logs](/images/manual/logs.png)

## The logs

| Tab | File | Contains |
| :--- | :--- | :--- |
| **System** | `/var/log/messages` | Everything the system and the services report: interfaces, gateways, VPNs, IPsec, the captive portal, intrusion detection, blocklists, updates ... |
| **DHCP / DNS** | `/var/log/dnsmasq.log` | DHCP leases (which device got which address) and DNS messages. |
| **PPPoE** | `/var/log/messages` (mpd5 only) | PPPoE connections: sign-in, address received, disconnects. |
| **NTP** | `/var/log/messages` (ntpd only) | Time synchronisation. |
| **Authentication** | `/var/log/auth.log` | Sign-ins to the panel, SSH and the console; OpenVPN sign-ins; failed attempts and blocked addresses. |
| **Web panel** | `/var/log/stratum.log` | The panel itself: who changed what, from which address. |
| **Configuration** | `/var/log/stratum-apply.log` | Every Apply: what was written, which services were reloaded, errors, Keep changes and automatic rollbacks. |

Each log shows the newest lines first, up to 500, refreshed every 5 seconds. **Search in this log** filters the
lines; **Pause** stops the refresh.

Lines with errors (*error*, *failed*, *denied*, *refused* ...) are highlighted red, warnings (*timeout*, *retry*,
*rollback*, *not responding* ...) yellow.

## Where to look

| Question | Log |
| :--- | :--- |
| Why did Apply fail, or why were my changes reverted? | **Configuration** |
| Who changed this, and when? | **Web panel** |
| Who signed in, and who failed? | **Authentication** |
| Which device had address 192.168.1.57 yesterday? | **DHCP / DNS** |
| Why does PPPoE not connect? | **PPPoE** - *authentication failed* means user name or password |
| Why is a WAN marked down? | **System** - lines of the gateway monitor |
| Why does a VPN user not connect? | **Authentication** (OpenVPN sign-in) and **System** (OpenVPN) |
| Captive portal sign-ins | **System**, program `stratum-captive` |

The AI Assistant can read these logs and explain them ([**Diagnostics > AI Assistant**](/manual/diagnostics-ai-assistant/)).

For long-term records, send the logs to a syslog server ([**System > Settings > Logging**](/manual/system-settings-logging-notifications/)).
