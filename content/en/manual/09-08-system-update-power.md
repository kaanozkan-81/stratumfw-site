---
title: "System > Update and Power"
seo_title: "09.8 System > Update and Power — Stratum FW User Manual"
description: "Online and offline updates from the signed repository, what an update does, boot environments and going back to an earlier version; restarting, powering off and resetting to factory defaults."
weight: 908
chapter_num: "09.8"
url: "/manual/system-update-power/"
aliases:
  - "/manual/09-08-system-update-power/"
translationKey: "manual-09-08-system-update-power"
---

## Update

![System > Update](/images/manual/update.png)

Stratum FW is updated from its own **signed repository** at stratumfw.com. Every update is signed, and the
firewall refuses any package whose signature or checksum does not match - an update cannot be tampered with on
the way.

### Checking

The firewall checks for updates **every night** by itself. **Software update** shows the **Installed** and
**Available** version, the **Status** and when it **Last checked**. **Check now** checks at once.

When an update is available, its **Release notes** are shown, and a notification can be sent ([**System > Settings > Notifications**](/manual/system-settings-logging-notifications/), event *A software update is available*).

Updates are never installed automatically: you decide when.

### Installing

Click **Update to ...**. The panel asks once more - *A configuration backup and a boot environment are created
first. Install now?* - then shows each step:

1. **Configuration backup** - a copy of the configuration.
2. **Boot environment (system snapshot)** - a snapshot of the whole system, the way back if anything goes wrong.
3. **Install** - fetching, checking integrity, upgrading, extracting.
4. **Restart the web panel**.
5. **Verify the new version**.

The page follows the progress by itself - you may close it and come back. **Show the full log** shows every line.

**While an update runs, do not restart or power off the firewall.** Routing and filtering keep running during an
update; the panel and the Stratum services restart for a moment. Suricata is restarted only when it changed,
because a restart briefly interrupts the traffic of blocking ports. When a new version writes some generated service files differently, the update notes *Apply to activate the update's changes* in the log, on the dashboard and as a notification.

> **Development deployment:** a system where Stratum FW was copied on rather than installed as a package says
> so on this page; updates are disabled there, checking still works.

### Update from a file

For firewalls without internet access: download the update file (`.sfu`) from stratumfw.com on another computer,
then **Choose file** and **Upload and verify**. Its signature and every package checksum are verified exactly like
an online update; the install then runs the same steps.

### Boot environments - going back

A **boot environment** is a snapshot of the whole system taken before every update. The table shows each with its
**Name**, **State**, **Next boot**, **Created** and **Space**.

To go back to the version before an update: **Boot this** on its boot environment, then confirm *Reboot into this boot
environment now?*. The firewall restarts with that version **and its configuration** as they were at that moment
- changes made after the update are not there.

A boot environment can also be chosen from the **boot menu** on the console, if the panel itself does not start.

The newest five are kept; older ones are removed after an update.

## Power

![System > Power](/images/manual/power.png)

Only administrators can use this page.

### Restart or power off

**Restart** takes about a minute; the network is interrupted meanwhile. After **Power off**, the firewall has to
be switched on at the device. Both ask for a second click to confirm.

The console menu has the same: **9) Reboot system** and **10) Halt system**.

### Reset to factory defaults

Returns the firewall to the settings of a new installation - for a firewall that is given a new task, or when a
configuration cannot be repaired. **Download a backup first** if the configuration may be needed again.

| Option | Meaning |
| :--- | :--- |
| **Keep the panel users** | On: all accounts stay. Off: all users except `admin` are removed, and you set a **new password for "admin"** (at least 12 characters). |
| **Erase certificates, keys and secrets** | OpenVPN, WireGuard, IPsec, RADIUS, AI Assistant, Block Page CA, notifications, SNMP, MCP tokens, backup credentials and SSH key, panel certificate, configuration history, SSH keys. |
| **Erase logs and statistics** | Web Activity, Application Control usage and blocks, intrusion alerts, downloaded lists, traffic counters, firewall and DNS logs. |
| **Type the host name to confirm** | The firewall's host name, to make sure you are on the right firewall. |

**Reset and restart**.

What the factory defaults are - chosen so that **a firewall far away comes back reachable**:

- the reset uses the **ports in use now**: the WAN gets its address by DHCP; the LAN is `192.168.1.1/24` with a DHCP
  server (`.100`-`.199`) and may reach the internet;
- other interfaces, VLANs, rules, NAT, VPNs and services are removed;
- the panel listens on port 443 on the internal networks only; SSH uses its default settings;
- the time zone, the installed version and the update settings stay.

The reset is applied without the usual time limit (nobody could confirm it from the old address), and the
firewall restarts.
