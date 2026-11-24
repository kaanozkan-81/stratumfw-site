---
title: "Dashboard"
seo_title: "04. Dashboard — Stratum FW User Manual"
description: "The dashboard's panels, arranging and sizing them, adding and removing panels, the update check and the notifications at the top right."
weight: 4
chapter_num: "04"
url: "/manual/dashboard/"
aliases:
  - "/manual/04-dashboard/"
translationKey: "manual-04-dashboard"
---

The dashboard (**Overview > Dashboard**, the first page after sign-in) shows the state of the firewall at a
glance. It is made of panels on a grid; every user arranges, sizes, adds and removes panels for themselves, and
the layout is kept for their account on every browser.

![The dashboard](/images/manual/dashboard.png)

## The panels

Panels are refreshed on their own while the page is open, from every 5 seconds (state) to every 30 seconds
(reports). Most panels link to the page where the subject is managed: a click on a line or on the links in the
panel's title bar opens it.

| Panel | What it shows |
| :--- | :--- |
| **System** | Host name, Stratum FW version, uptime, the latency to the internet (`1.1.1.1`), the last configuration change and the packet filter's state. |
| **WAN** | Every internet connection: address, gateway, latency, how long it has been connected, today's volume and the current rate. |
| **CPU** | Processor use with the last 5 minutes, and the load average. |
| **Memory** | Memory in use. |
| **Disk** | Disk space in use. |
| **Temperature** | The hottest processor core or sensor (on hardware that reports it). |
| **Connections** | Firewall states in use, against the limit. |
| **Blocked** | Packets blocked and logged in the last hour. |
| **Traffic** | The traffic graph of the last hour for an interface you choose in the panel's title bar - the same data as [**Diagnostics > Traffic Graphs**](/manual/diagnostics-traffic-graphs/). |
| **Protection** | Blocklists, blocked sign-ins, intrusion detection and VPN at a glance. |
| **Interfaces** | Every interface: state, address, current rate in and out. |
| **Services** | Running and stopped services; the count in the title shows how many need attention. |
| **DHCP** | Devices and pool use per network. |
| **VPN connections** | OpenVPN and WireGuard users connected now (from where, with which VPN address, since when, how much traffic), WireGuard site-to-site and IPsec tunnels. |
| **Intrusion detection** | Alerts of the last 24 hours, blocked packets and the rules that fired most. |
| **Top blocked sources** | The addresses blocked most in the last hour. |
| **Top blocked ports** | The ports blocked most in the last hour. |
| **Recent changes** | The latest applied, kept and reverted configurations. |
| **Gateways** | The gateway monitor: latency and loss of every gateway, and which one carries the default route when failover is set up. |
| **Bandwidth limits** | The download and upload limits per network (Firewall > Shaper) with the current rate. |
| **Blocklists** | DNS and IP blocks of the last 24 hours, per list; administrators also see the most blocked domains and addresses. |
| **Applications** | The top applications and categories of the last hour ([**Firewall > Application Control**](/manual/firewall-application-control/)). |
| **Captive portal** | Devices signed in per zone, the latest sign-ins and the service state; a warning when the RADIUS server does not confirm accounting. |
| **Backup** | The last automatic SFTP or SMB backup to your server. *Administrators only.* |
| **Web Activity** | The most visited sites and the active devices of today. *Administrators only.* |

A new account starts with the default layout: System, WAN, the six small gauges (CPU, Memory, Disk,
Temperature, Connections, Blocked), Traffic, Protection, Interfaces, Services, DHCP, VPN connections,
Intrusion detection, Top blocked sources, Top blocked ports and Recent changes. The other panels can be added
when you need them.

A panel for a feature that is not in use says so, with a link to set it up - for example *No bandwidth limit.
Add one*.

## Arrange the dashboard

Click **Customize** above the panels. The dashboard switches to editing:

- **Move** a panel by dragging its title bar.
- **Resize** it by dragging its edges or its lower corners. Each panel has a minimum size.
- **Remove** a panel with **×** in its title bar.
- **Add panels** opens the list of all panels you may use; **Add** places a panel in the first free place,
  **Remove** takes it off. Then go back to the dashboard to move and size it.
- **Reset layout** returns to the default layout.

Click **Done** when you have finished. Every change is saved at once for your account; other users keep their
own layouts.

![Customizing the dashboard](/images/manual/dashboard-customize.png)

> **Note:** On a phone or a narrow window the panels are shown one below the other in your layout's order, and
> the layout cannot be edited there (*Arrange the panels on a wider screen*). Nothing you see on a phone changes
> your layout.

## Checking for updates

The round-arrow button at the top of the dashboard asks the update server whether a newer version exists and
shows the answer - *Stratum FW is up to date* or *Version ... is available.* Updates themselves are installed on
**System > Update**. The firewall also checks once a day on its own and reports each new version once, as a notification.

## Notifications

The bell at the top right of every page collects the firewall's events. A number on the bell shows how many
you have not read yet.

![The notifications drop-down](/images/manual/dashboard-notifications.png)

The events are the same ones that can be sent by Telegram or e-mail (**System > Settings > Notifications**):
a WAN connection going down and coming back, an address blocked after failed sign-ins, a configuration that
failed to apply or was reverted automatically, an available update, failed blocklist downloads, a failed
backup, a full disk, a high temperature, the panel certificate expiring within 21 days, and intrusion detection alerts. Problems are
marked in red, recoveries in green.

- Click the bell to open the list of the latest events.
- **✓** marks one event as read; **×** removes it from your list.
- **Mark all read** and **Clear** act on the whole list.
- **Settings** opens **System > Settings > Notifications**.

Read marks and removals are per user: clearing your list does not clear anyone else's. When a new event
arrives while a page is open, it also appears for a few seconds as a pop-up in the corner of the screen.

Notifications in the panel are always on; Telegram and e-mail are switched on, and the events chosen, on the
notification settings page.

