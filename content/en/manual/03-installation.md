---
title: "Installation"
seo_title: "03. Installation — Stratum FW User Manual"
description: "Hardware and virtual machine requirements, downloading and verifying the installer, installing to disk, the first-boot setup wizard and the console menu."
weight: 3
chapter_num: "03"
url: "/manual/installation/"
aliases:
  - "/manual/01-installation/"
  - "/manual/03-installation/"
translationKey: "manual-01-installation"
---

This chapter takes you from an empty machine to a running firewall whose web panel you can open. It covers
the requirements, the two installer images, the installer itself, the setup wizard that runs on the first boot,
and the console menu you will see on the screen from then on.

## Requirements

Stratum FW runs on 64-bit x86 hardware (`amd64`): small network appliances, PCs, rack servers and virtual
machines.

| Resource | Minimum | Recommended |
| :--- | :--- | :--- |
| Processor | 64-bit x86 | A CPU with AES-NI (faster VPN and encrypted backups) |
| Memory | 2 GB | 4 GB with DNS blocklists and the Web Filter; 8 GB or more with intrusion detection (Suricata) |
| Disk | 8 GB (the installer refuses smaller disks) | 32 GB or more on an SSD - room for logs, reports and several boot environments |
| Network ports | 1 (the WAN) | 2 or more: WAN, LAN, and more for further networks or a second internet line |

The memory figures are guidelines: Stratum FW does not refuse to run with less, but intrusion detection and
large blocklists load a lot into memory.

**Network cards.** The cards supported by FreeBSD 15.1 work, which includes most Intel (`igb`, `ix`, `em`),
Realtek, Broadcom and Chelsio cards and the virtual cards of the common hypervisors. Intel cards are the safest
choice for a firewall.

**Virtual machines.** Stratum FW runs on Proxmox VE, KVM/QEMU, VMware ESXi, Hyper-V and VirtualBox.

- Give the VM at least two network cards and connect them to the right virtual switches *before* the
  installation - you assign them in the setup wizard.
- On Proxmox and KVM use **VirtIO** for disk and network (`vtbd`, `vtnet`); on VMware use **VMXNET3**.
- Set the CPU type to `host` (or another type that passes AES-NI through) so the VPNs can use hardware
  encryption.

> **Note:** Plan which physical port will be the WAN (towards the internet or the upstream router) and which the
> LAN (towards your network) before you start. The setup wizard can find a port for you when you plug a cable
> in, but it helps to know the names: on most Intel appliances the ports are `igb0`, `igb1` ... in the order
> printed on the case.

## Download and verify

The installers are published on the stratumfw.com [**Download**](/download/) page, together with a checksum file
(`SHA256SUMS-<version>`):

| File | Use it for |
| :--- | :--- |
| `stratum-fw-<version>-amd64.iso` | Virtual machines, and physical machines that boot from a CD/DVD or an ISO mounted by the server's management card (IPMI, iDRAC, iLO). |
| `stratum-fw-<version>-amd64.img` | A USB stick, for physical machines. |

Both images contain the same system and both work with a screen and keyboard **and** with a serial console
at the same time (115200 baud, 8N1, the first serial port). Appliances without a screen - most network
appliances - are installed over their console port with a serial or USB console cable. The installed firewall
keeps the same consoles.

Always check that the download is complete and unmodified. Compare the result with the line for your file in
`SHA256SUMS-<version>` (and on the Download page):

```bash
# Linux
sha256sum stratum-fw-0.36.1-amd64.img

# macOS / FreeBSD
shasum -a 256 stratum-fw-0.36.1-amd64.img

# Windows (PowerShell)
Get-FileHash .\stratum-fw-0.36.1-amd64.img -Algorithm SHA256
```

If the values differ, download the file again. Never install from an image whose checksum does not match.

## Write the USB stick

Everything on the stick is erased.

- **Windows, macOS, Linux:** use **balenaEtcher** or **Rufus**. In Rufus, choose *DD image* mode when it asks.
- **Linux command line** - replace `/dev/sdX` with your stick (check with `lsblk`; choosing the wrong device
  erases that disk):

```bash
sudo dd if=stratum-fw-0.36.1-amd64.img of=/dev/sdX bs=4M status=progress conv=fsync
```

- **macOS command line** - find the stick with `diskutil list`, then:

```bash
diskutil unmountDisk /dev/diskN
sudo dd if=stratum-fw-0.36.1-amd64.img of=/dev/rdiskN bs=4m
```

## Connect to the console

- **Screen and keyboard:** connect them and boot from the stick or the ISO (the boot menu key is usually
  <kbd>F11</kbd>, <kbd>F12</kbd> or <kbd>Esc</kbd>; on servers, the management card can mount the ISO).
- **Serial console:** connect the console cable and open a terminal at **115200 baud, 8 data bits, no parity,
  1 stop bit, no flow control**:

```bash
# Linux / macOS (the device name depends on the cable)
screen /dev/ttyUSB0 115200
```

  On Windows use PuTTY: *Serial*, the cable's COM port, speed 115200.

## The installer

The machine boots into the **Welcome to Stratum FW** boot menu, which continues by itself after a few seconds,
and then into the installer:

```
==============================================================
  Stratum FW 0.36.1 - Installer
==============================================================

  1) Install Stratum FW
  2) Shell
  3) Reboot
  4) Power off
```

![The installer's main menu](/images/manual/install-menu.png)

Choose **1) Install Stratum FW**.

### 1. Choose the disk

The installer lists the disks it found, with their size and model. The stick or CD you booted from is not
listed.

```
Available disks:
  1) ada0       128 GB  KINGSTON SA400S37128G
  2) da1         32 GB  SanDisk Ultra

Select target disk [1]:
```

Type the number of the disk and press <kbd>Enter</kbd>. Disks smaller than 8 GB are refused.

### 2. Confirm

```
  !!! ALL DATA ON ada0 (128 GB) WILL BE ERASED !!!
  Type 'yes' to continue:
```

Type `yes` in full. Anything else returns to the menu without touching the disk.

### 3. Set the root password

```
Root password (console and SSH, min. 8 characters):
Repeat password:
```

This is the password of the system's `root` account, used to sign in over SSH. It is **not** the web panel
password - that is set in the setup
wizard on the first boot. Use a long, unique password and keep it safe.

### 4. Installation

The installer now works on its own; nothing is downloaded, everything comes from the installation media:

```
Installing Stratum FW 0.36.1 to ada0 (log: /tmp/stratum-install.log)

  Partitioning ada0                              ok
  Creating ZFS pool                              ok
  Installing FreeBSD base (offline)              ok
  Configuring system                             ok
  Installing Stratum FW packages                 ok
  Stratum system integration                     ok
  Setting root password                          ok
  Installing boot loaders (BIOS + UEFI)          ok
  Finalizing                                     ok

Installation complete. Remove the install media before rebooting.
Reboot now? [Y/n]:
```

It takes a few minutes. Remove the stick (or eject the ISO), then press <kbd>Enter</kbd> to reboot.

If a step fails, the installer shows **Installation FAILED at:** with the step and the last lines of its log,
and returns to the menu. **2) Shell** lets you read the full log (`/tmp/stratum-install.log`) before you try
again. The most common causes are a failing disk and a disk that is still in use by another system's RAID or
ZFS pool.

### What the installer creates

| Partition | Size | Purpose |
| :--- | :--- | :--- |
| `freebsd-boot` | 512 KB | Boot code for BIOS (legacy) machines |
| `efi` | 260 MB | Boot loader for UEFI machines |
| `freebsd-swap` | 2 GB | Swap |
| `freebsd-zfs` | the rest | The ZFS pool `zroot` (compressed) |

The disk boots on both BIOS and UEFI machines. The system lives in the boot environment
`zroot/ROOT/default`; every later update adds a new boot environment beside it (see the chapter on
**System > Update**).

## First boot: the setup wizard

On the first start, every console shows:

```
Welcome to Stratum FW 0.36.1

This wizard assigns network ports, sets IP addresses, WAN access
and the web panel administrator password.

Press Enter to start the setup wizard...
```

Press <kbd>Enter</kbd> on the console you are using. If the machine has both a screen and a serial console,
the wizard runs on the one where <kbd>Enter</kbd> was pressed first; the other one waits until it has finished.

### Step 1 - Assign interfaces

The wizard lists the network ports with their MAC address, link state and speed:

```
 Port     MAC                Link         Media                   Assigned
 igb0     00:90:0b:aa:bb:01  active       Ethernet autoselect     
 igb1     00:90:0b:aa:bb:02  no carrier   Ethernet autoselect     
 igb2     00:90:0b:aa:bb:03  no carrier   Ethernet autoselect     

WAN interface (name or 'a' for auto-detect):
```

For each role, type the port name - or `a` to let the wizard find it: unplug the cable of that port if it is
plugged in, then plug it in within 30 seconds; the port whose link comes up is chosen.

| Role | Required | What it is |
| :--- | :--- | :--- |
| **WAN** | yes | The internet side: towards the modem, the provider's router or the upstream network. |
| **LAN** | no (press <kbd>Enter</kbd> to skip) | Your internal network. The web panel is reached from here. |
| **OPT1**, **OPT2** ... | no | Further networks (guests, servers, cameras ...) or a second internet line. The wizard keeps asking until you press <kbd>Enter</kbd> without a name. |

> **Warning:** Without a LAN, the firewall can only be managed from the console until you open management
> access from the WAN. Assign a LAN unless you have a reason not to.

The wizard shows the assignment and asks **Proceed?** - answer `y` (or press <kbd>Enter</kbd>).

You can change all of this later: more ports and VLANs are added in the panel under **Interfaces > Assignments**
and **Interfaces > VLANs**.

### Step 2 - IP addresses

The wizard then asks for each assigned interface in turn.

**WAN**

```
Configure WAN IPv4 via DHCP? [Y/n]:
```

- **Yes** (the default) when the WAN receives its address automatically - a modem in router mode, the
  provider's router or a company network.
- **No** for a fixed address: enter it with its prefix (for example `203.0.113.10/29`), then the gateway, which
  must be inside that network.

A PPPoE connection (fibre or DSL straight from the provider's modem in bridge mode) is not set here: choose DHCP
for now and set PPPoE in the panel afterwards (**Interfaces > Assignments**, the WAN interface).

**LAN and OPT interfaces**

```
LAN IPv4 address with prefix (e.g. 192.168.1.1/24) [192.168.1.1/24]:
Enable DHCP server on LAN? [Y/n]:
DHCP range start [192.168.1.100]:
DHCP range end [192.168.1.199]:
```

- The address is the firewall's own address in that network, and the gateway of the devices in it. The LAN
  suggests `192.168.1.1/24`; if that network is already used upstream (for example by the provider's modem on
  the WAN side), choose another one, such as `10.10.10.1/24`.
- The DHCP server hands out addresses to the devices in the network. It is suggested for the LAN and off for
  OPT interfaces. The range must be inside the network and must not contain the firewall's own address.

### Step 3 - Management access from the WAN

```
Management access from WAN
Allows the web panel (HTTPS) and SSH to be reached from the WAN side.
Recommended: No - manage the firewall from LAN.
Allow web panel and SSH access from WAN? [y/N]:
```

Answer **No** unless the firewall is in a lab or behind another firewall and has no LAN. You can open it later -
limited to the addresses you choose - under **System > Settings > Admin Access**, or from the console menu.

### Step 4 - Time zone

```
Timezone or part of a name [Europe/Istanbul]:
```

Type the zone, or part of a city name (`berlin`, `new_york`, `istanbul`); the wizard lists the matches when
there are several. Logs, schedules and reports use this time zone.

### Step 5 - Web panel administrator

```
Web panel administrator
New password for 'admin' (min. 10 characters):
Repeat password:
```

This is the password of the panel user `admin`, with the **Administrator** role. More users are added later in
the panel (**System > Users**).

### Step 6 - Apply and keep

The wizard applies the settings in the same way as the panel (see **How a change reaches the system** in the
Introduction): the configuration is checked, activated, and then you have 120 seconds to keep it:

```
New settings are active. If you do not answer within 120 seconds they are reverted automatically.
Keep the new settings? (n restores the previous settings) [Y/n]:
```

Press <kbd>Enter</kbd> to keep them. If the check fails, nothing is changed and the wizard lists the reasons;
correct them with the console menu (below).

## The console menu

After the wizard - and from then on, every time the console is used - the screen shows the firewall's state
and the console menu:

```
Stratum FW 0.36.1  -  stratum  -  FreeBSD 15.1-RELEASE

 WAN   igb0     203.0.113.10/29        static  up
 LAN   igb1     192.168.1.1/24         static  up

 Web panel: https://192.168.1.1/
 WAN management access: closed

  1) Assign interfaces               6) Management access
  2) Set interface IP address        7) Pending change
  3) Reset admin password            8) Shell
  4) Restart web panel               9) Reboot system
  5) Ping host                      10) Halt system
  0) Exit

Enter an option:
```

![The console menu](/images/manual/install-console-menu.png)

The top lines show each interface with its port, address, mode and link state, and the address of the web
panel.

| Option | What it does |
| :--- | :--- |
| **1) Assign interfaces** | Assign the ports to WAN, LAN and OPT roles again, as in the wizard. |
| **2) Set interface IP address** | Change the address of one interface (and its DHCP range). |
| **3) Reset admin password** | Set a new password for a panel user (`admin` by default). It also offers to switch off the user's two-factor sign-in - for a lost phone. |
| **4) Restart web panel** | Restart the panel and its web server, for example when the page does not load. |
| **5) Ping host** | Check the internet connection (`1.1.1.1` by default) or a host in your network. |
| **6) Management access** | Show how the panel and SSH can be reached; open or close access from the WAN; or reset management access to the defaults (port 443, all internal networks, anti-lockout on, SSH on) when a setting has locked you out. |
| **7) Pending change** | Keep or revert a change that is waiting for confirmation - for example one applied from the panel shortly before you lost access to it. |
| **8) Shell** | A command shell as `root`. Type `exit` to return to the menu. |
| **9) Reboot system** / **10) Halt system** | Restart or power off, after a confirmation. |

Changes made from the console menu are checked, applied and confirmed in the same way as in the wizard.

> **Warning:** The console logs in as `root` automatically and opens this menu, so that a firewall can always
> be repaired on site. Anyone with physical access to the console has full control of the firewall: keep the
> machine in a locked room or rack, and treat its serial console cable like a key.

## Next steps

Open the address shown as **Web panel** from a computer in the LAN - for example `https://192.168.1.1/` - and
sign in as `admin`. The browser warns once about the certificate, because it was created by the firewall itself;
this is expected on a new installation.

Continue with [**02 Initial Configuration**](/manual/initial-configuration/): the first sign-in, the internet connection, DNS, and the first rules.

