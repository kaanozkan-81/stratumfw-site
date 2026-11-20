---
title: "Services > Service Status"
seo_title: "07.1 Services > Service Status — Stratum FW User Manual"
description: "The state of every service on the firewall - running, stopped or switched off - with uptime and memory, and starting, stopping and restarting them."
weight: 701
chapter_num: "07.1"
url: "/manual/services-service-status/"
aliases:
  - "/manual/07-01-services-service-status/"
translationKey: "manual-07-01-services-service-status"
---

**Services > Service Status** lists the programs that make up the firewall, whether each one runs, and lets an
administrator start, stop or restart them.

![Services > Service Status](/images/manual/svc-status.png)

## The list

| Column | Meaning |
| :--- | :--- |
| **Service** | The service and a short description; a link opens its settings page. |
| **Status** | **Running**; **Stopped** - with *should run* when its settings say it should (counted on the [dashboard's](/manual/dashboard/) **Services** panel); or **Switched off** - not used by the current settings. A running service that its settings switch off is marked *switched off in settings*. |
| **Running for** | How long it has been running. |
| **Memory** | The memory it uses. |
| **PID** | The process number, for the command line. |
| **Actions** | **Start**, **Stop**, **Restart** - as the service allows. |

Services that are switched off in their settings are hidden; **Show switched-off services** shows them too.
**Refresh** reads the states again.

## The services

| Service | What it is | Actions |
| :--- | :--- | :--- |
| **Packet filter** | Firewall rules and NAT (pf). Restart reloads the rules. | Restart |
| **Firewall logging** | pflogd - writes the packets that rules log. | Start, stop, restart |
| **DHCP server** | dnsmasq - DHCP, and DNS when Unbound is not used. | Start, stop, restart |
| **DNS resolver** | Unbound - DNS, DNS blocklists, Web Filter. | Start, stop, restart |
| **Time server** | ntpd. | Start, stop, restart |
| **Intrusion detection** | Suricata (one or two engines). | Start, stop, restart |
| **OpenVPN** | The OpenVPN server. | Start, stop, restart |
| **WireGuard** | The WireGuard remote-access interface `wg0`. | Start, stop, restart |
| **IPsec** | strongSwan (charon) - site-to-site tunnels. | Start, stop, restart |
| **Captive portal** | The sign-in page and its sessions. Signed-in devices stay signed in across a restart. | Start, stop, restart |
| **PPPoE** | mpd5. Stopping it disconnects the PPPoE WAN. | Start, stop, restart |
| **Gateway monitor** | Gateway status, failover and policy routing ([**Interfaces > Gateways**](/manual/interfaces-gateways/)). | Start, stop, restart |
| **Firewall log collector** | Feeds [**Diagnostics > Firewall Log**](/manual/diagnostics-firewall-log/). | Start, stop, restart |
| **Traffic collector** | Feeds [**Diagnostics > Traffic Graphs**](/manual/diagnostics-traffic-graphs/). | Start, stop, restart |
| **Web Activity collector** | Feeds [**Diagnostics > Web Activity**](/manual/diagnostics-web-activity/). | Start, stop, restart |
| **SNMP** | The SNMP agent (bsnmpd). | Start, stop, restart |
| **SSH** | Open sessions stay connected on a restart. | Restart |
| **Web server** | nginx. The page reloads after a restart. | Restart |
| **Web panel** | This panel. It is back within a few seconds. | Restart |
| **System log** | syslogd. | Restart |
| **Scheduler** | cron - updates, backups, alias and list refreshes. | Restart |

Services that the current settings do not use - IPsec without tunnels, PPPoE without a PPPoE WAN - are **Switched off** and hidden unless **Show switched-off services** is ticked.

## Start, stop, restart

- **Restart** is the usual cure for a service that misbehaves - for example a DNS resolver that stopped
  answering. Restarting is harmless for most services; the descriptions above say where it is not (PPPoE
  disconnects, the panel is gone for a few seconds).
- **Stop** stops a service until it is started again, until the next **Apply** that changes it, or until a
  reboot. Whether a service runs at start-up is not decided here but by its settings page - to switch a service
  off for good, switch it off there.
- Some services can only be restarted, because the firewall cannot work without them (packet filter, SSH, web
  server, panel, system log, scheduler).

Only administrators can start, stop and restart services; every action is logged.

> **Tip:** A service shown as **stopped** that does not start again usually says why in
> [**Diagnostics > System Logs**](/manual/diagnostics-system-logs/). A configuration problem is normally caught before that: Apply checks the
> configuration of the services first and refuses a change they would not accept.
