---
title: "Installation Guide (.iso & .img)"
description: "Install Stratum Firewall on physical hardware or virtual machines using the bootable VGA ISO (.iso) or headless Serial Console USB (.img) installer."
lead: "Deploy Stratum Firewall onto bare-metal hardware or virtual infrastructure in minutes using our offline ZFS appliance installers."
aliases: ["/resources/documentation/building-from-source/"]
---

Stratum Firewall ships as a self-contained **FreeBSD 15.1-RELEASE (`amd64`)** appliance operating system on the [Download](/download/) page. Every installation configures a GPT partition layout (supporting both UEFI and legacy BIOS), a resilient **ZFS root pool**, and atomic **ZFS Boot Environments** completely offline—no internet connection is required during installation.

> **Want to explore the interface first?** Launch the [Live Browser Demo](https://demo.stratumfw.com) (`demo` / `Stratum2026`) to inspect a running FreeBSD 15.1 appliance before flashing media.

## 1. System Requirements

| Resource | Minimum | Recommended (IPS / DNSBL / Multi-WAN) |
| :--- | :--- | :--- |
| **Architecture** | 64-bit x86 (`amd64`) | 64-bit x86 (`amd64`) with AES-NI |
| **Memory (RAM)** | 2 GB | 4 GB – 8 GB (for ZFS ARC, Unbound RPZ &amp; Suricata IPS) |
| **Storage** | 16 GB SSD / NVMe / Virtual Disk | 32 GB+ SSD / NVMe (stores 5 ZFS boot environments &amp; logs) |
| **Network Ports** | 2 NICs (`WAN` + `LAN`) | 2+ Intel / VirtIO NICs (`igb`, `ix`, `em`, `vtnet`) |

## 2. Choose Your Installer Variant

Two dedicated media types are published on the [Download](/download/) page:

* **VGA ISO Installer (`stratum-fw-0.27.1-amd64.iso`):** Designed for physical servers with a monitor and keyboard attached, as well as virtual machines (**Proxmox VE**, **VMware ESXi**, **KVM / QEMU**, **Hyper-V**).
* **Serial USB Disk Image (`stratum-fw-0.27.1-amd64.img`):** Raw disk image pre-configured for headless network appliances (`115200 baud, 8N1` on `COM1` / `ttyu0`, RJ45/USB console cables, IPMI Serial-over-LAN).

The installed OS automatically preserves the console mode (`VGA` or `Serial`) of the installer you booted from.

## 3. Verify SHA-256 Checksum &amp; Write USB Media

Always verify the downloaded image hash against the value displayed on the [Download](/download/) page:

```bash
# Linux or macOS
sha256sum stratum-fw-*-amd64.*

# Windows PowerShell
Get-FileHash .\stratum-fw-*-amd64.* -Algorithm SHA256
```

To write the `.img` (or hybrid `.iso`) to a USB flash drive:

* **Windows / macOS / Linux GUI:** Use **BalenaEtcher** or **Rufus** (select *DD Image mode* if prompted) and flash the image directly to your target USB stick.
* **Linux / FreeBSD / macOS CLI:**
  ```bash
  # Replace /dev/sdX with your actual USB block device!
  sudo dd if=stratum-fw-0.27.1-amd64.img of=/dev/sdX bs=4M status=progress conv=fsync
  ```

## 4. Virtual Machine Tips (Proxmox / KVM / ESXi)

* **Disk &amp; NIC Drivers:** Use **VirtIO Block** (`vtbd0`) and **VirtIO Network** (`vtnet0`, `vtnet1`) on Proxmox/KVM, or `VMXNET3` / `E1000e` on VMware ESXi.
* **CPU Type:** Select `host` (or `x86-64-v2-AES`+) so hardware AES acceleration is exposed to OpenVPN, WireGuard, and encrypted backups.

## 5. Installing &amp; First-Boot Console Wizard

1. Boot from the installation media. For Serial `.img` appliances, connect your terminal (`screen`, `minicom`, or PuTTY) at **`115200 8N1`**.
2. Select your target storage drive in the installer prompt. The installer formats the drive with **ZFS** (`zroot/ROOT/default`) and installs the complete appliance offline.
3. Remove the installation USB/ISO when prompted and reboot.
4. On first boot, **Stratum Console Setup (`stratum-menu`)** guides you through:
   * Mapping physical network ports to `WAN`, `LAN`, and optional `OPT` interfaces,
   * Setting your `LAN` IPv4 subnet (default `192.168.1.1/24` with DHCP enabled) and `WAN` mode (`DHCP`, `Static`, or `PPPoE`),
   * Setting your time zone and initial `admin` password.

Once booted, open `https://192.168.1.1/` from a machine on your LAN and continue with [Initial Configuration](/resources/documentation/initial-configuration/)—including updating to the latest release (**v0.29.0**) from **System → Update**.
