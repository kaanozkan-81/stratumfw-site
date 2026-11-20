---
title: "Console Menu"
seo_title: "11.1 Console Menu — Stratum FW User Manual"
description: "The menu on the VGA or serial console and over SSH: the status header, assigning ports, setting addresses, resetting the admin password, management access, pending changes, the shell, reboot and halt - and the first-boot wizard."
weight: 1101
chapter_num: "11.1"
url: "/manual/console-menu/"
aliases:
  - "/manual/11-01-console-menu/"
translationKey: "manual-11-01-console-menu"
---

The console menu is the way in when the web panel cannot be reached: on a monitor and keyboard (VGA), on a serial
console (the console port of appliances such as the WatchGuard M370), or over **SSH**. It works the same on all of
them.

On the local console (VGA and serial) the menu appears by itself. Over SSH, sign in as `root` ([**System > Settings > Admin Access**](/manual/system-settings-general-access/), SSH): you get a shell - type `stratum-menu` to open the menu.

![The console menu](/images/manual/console-menu.png)

## The header

At the top the menu shows the state of the firewall:

```
Stratum FW 0.36.1  -  fw-belek  -  FreeBSD 15.1-RELEASE

 WAN   igb0     203.0.113.10/29        static  up
 LAN   igb1     192.168.1.1/24         static  up
 OPT1  igb2     10.20.0.1/24           static  down

 Web panel: https://192.168.1.1/   https://10.20.0.1/
 WAN management access: closed
```

- Each interface with its **role**, **port**, **address**, **mode** (static, dhcp, pppoe) and **link** (*up*, *no
  carrier*, or *missing* when the port does not exist on this machine).
- **Web panel**: the addresses to type into the browser (with the port when it is not 443). WAN addresses are
  listed only when management from WAN is open.
- *A configuration change is awaiting confirmation (option 7)* when an Apply has not been kept yet.

## The options

```
  1) Assign interfaces               6) Management access
  2) Set interface IP address        7) Pending change
  3) Reset admin password            8) Shell
  4) Restart web panel               9) Reboot system
  5) Ping host                      10) Halt system
                                     0) Exit
```

### 1) Assign interfaces

Decides which network port is WAN, LAN, OPT1 ... The list shows every port with its **MAC** address, **Link** and
**Media**, and what it is assigned to now.

For each role, type the port name (`igb0`, `em1` ...) - or **`a` for auto-detect**: unplug the cable of the port you
want, then plug it in within 30 seconds; the port whose link comes up is taken. Handy on appliances whose port
labels do not match the system's names.

**WAN** is required; **LAN** and further **OPT** ports are optional (Enter to skip). The new assignment is shown
and applied after **Proceed?**.

A role that already existed keeps its settings on the new port. A new LAN gets `192.168.1.1/24` with a DHCP server
(`192.168.1.100`-`.199`).

### 2) Set interface IP address

Choose an interface, then:

- **WAN**: *Configure WAN IPv4 via DHCP?* - or a static address with prefix and the **WAN gateway**;
- **internal interfaces**: the address with prefix (for example `192.168.1.1/24`), and whether to run a **DHCP
  server** with its range.

PPPoE, VLANs and everything else are set in the web panel.

### 3) Reset admin password

Sets a new password for a panel user (default `admin`), at least 10 characters. If the user has two-factor
sign-in, the menu offers to switch it off as well (lost phone).

This is the way back in when you have forgotten the password. Anybody at the console can do it - another reason to
keep the firewall in a locked room.

### 4) Restart web panel

Restarts the panel and nginx - when the panel does not answer but the firewall routes.

### 5) Ping host

Four pings to a host (default `1.1.1.1`) - the quickest test of the internet connection.

### 6) Management access

```
 1) Open or close WAN access
 2) Reset management access to the defaults
    (port 443, all internal networks, anti-lockout on, SSH on with password; ...)
```

The way back in after a mistake on [**System > Settings > Admin Access**](/manual/system-settings-general-access/). **2** restores the defaults; WAN access and
the SSH keys stay as they are.

### 7) Pending change

When an Apply is awaiting confirmation, shows it and asks *Keep the pending change?* - **y** keeps it, **n** (the
default) restores the previous settings. Useful when a change made in the panel cut you off: revert it here instead
of waiting for the timer.

### 8) Shell

A root shell (`/bin/sh`) for the commands of the [next chapter](/manual/command-line/). `exit` returns to the menu.

### 9) Reboot system, 10) Halt system

Restart or power off, after a confirmation.

### 0) Exit

Leaves the menu. On the local console a new sign-in prompt appears; over SSH you are back in the shell.

## Changes made on the console

Options 1, 2 and 6 apply their change at once, with the same protection as the panel: the new settings are active,
and **if you do not answer within 120 seconds they are reverted automatically**. Answer *Keep the new settings?*
with **y** (Enter) to keep them, **n** to go back.

The change is validated first; an invalid change is refused and nothing is touched.

## The first-boot wizard

On the first start after installation - when there is no configuration yet - every console shows *Welcome to
Stratum FW*. Press **Enter** on one of them; the others wait until it has finished.

The wizard walks through:

1. **Assign interfaces** - as option 1.
2. **Addresses** of every assigned interface - as option 2.
3. **Management access from WAN** - recommended *No*: manage the firewall from LAN. Without a LAN, a closed WAN means
   the firewall can only be managed from the console.
4. **Time zone** - type part of a city name, for example `istanbul`.
5. **Web panel administrator** - the password of `admin`, at least 10 characters.

Then the configuration is applied and the menu appears with the panel's address. The rest is done in the web panel
- see [**Initial Configuration**](/manual/initial-configuration/).

To run the wizard again later: `stratum-menu --setup` in the shell. It starts over with the assignment of the ports.
