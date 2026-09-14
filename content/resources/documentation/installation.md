---
title: "Installation"
lead: "Prepare hardware, write the installer to USB, and complete a fresh installation of Stratum Gateway."
---

Stratum Gateway is installed from a custom Debian 13 image. The installation is **fully automated** — once the system boots from the USB drive, the installer runs without prompts and provisions a working gateway with the panel accessible on the local network.

A typical installation completes in about 15 to 20 minutes on modern hardware.

## Before you begin

### Hardware requirements

| Component | Minimum | Recommended |
|---|---|---|
| CPU | 64-bit x86-64, 2 cores | 4 cores |
| RAM | 2 GB | 4 GB or more |
| Storage | 8 GB | 20 GB or more (SSD preferred) |
| Network interfaces | 2 (WAN + LAN) | 2 or more |

Stratum Gateway runs on bare metal, virtual machines (KVM, VMware, Proxmox, VirtualBox), and cloud instances that support custom ISO boot. Both **legacy BIOS** and **UEFI** boot are supported.

### What you'll need

- A USB flash drive, **2 GB or larger**
- The Stratum Gateway installer ISO ([download here](/download/))
- A target machine with the hardware above
- Access to the machine's BIOS/firmware setup (to configure boot order)
- A computer to write the USB drive (Windows, Linux, or macOS)

## Choose the right installer image

Every release ships with **two installer images**. They install the identical gateway — the only difference is how the console is displayed during and after installation.

| Image | Use when |
|---|---|
| `stratum-gateway-installer-vga.iso` | You have a physical monitor or VM display console (most common) |
| `stratum-gateway-installer-serial.iso` | You're installing on a headless server accessed via IPMI, iDRAC, iLO, or another serial-over-LAN console |

If you're unsure, choose the **VGA** image. You can switch to serial console output later if you migrate to a headless environment.

## Write the installer to USB

### Windows — using Rufus

[Rufus](https://rufus.ie/) is the recommended tool on Windows.

1. Download and run Rufus (no installation required).
2. Insert the USB drive.
3. Under **Device**, select your USB drive.
4. Under **Boot selection**, click **SELECT** and choose the Stratum Gateway ISO file.
5. Under **Partition scheme**, choose:
   - **MBR** for legacy BIOS systems
   - **GPT** for UEFI systems
6. Leave **File system** and **Cluster size** at their defaults.
7. Click **START**.
8. If prompted about ISO mode vs. DD mode, choose **ISO Image mode** (recommended).
9. Confirm the warning that the USB drive will be erased.
10. Wait for the write to complete — typically 2 to 5 minutes.

### Linux — using dd

~~bash
# Identify your USB device (double-check the device path!)
lsblk

# Write the ISO to the device — replace /dev/sdX with your USB device
sudo dd if=stratum-gateway-installer-vga.iso of=/dev/sdX bs=4M status=progress oflag=sync
~~~

> **Warning:** `dd` writes directly to the block device. Choosing the wrong `/dev/sdX` will destroy data. Verify carefully with `lsblk` first.

### macOS — using dd

~~bash
# Identify your USB device
diskutil list

# Unmount the drive (replace N with the correct disk number)
diskutil unmountDisk /dev/diskN

# Write the ISO (replace N with the correct disk number)
sudo dd if=stratum-gateway-installer-vga.iso of=/dev/rdiskN bs=4m
~~~

After writing, eject the USB drive safely.

## Boot from the USB drive

1. Insert the USB drive into the target machine.
2. Power on and enter the BIOS/UEFI setup (usually `Del`, `F2`, `F10`, or `F12` — depends on the manufacturer).
3. Set the boot order so the USB drive is first.
4. Save and exit the firmware setup.
5. The machine will boot from the USB drive.

Both legacy BIOS and UEFI boot are supported. If your system supports both, either works — pick whichever mode you prefer for the deployment.

## The installation runs automatically

Once the system boots from the USB drive, no further input is required. The installer is preseeded and will:

1. Partition the target disk
2. Install Debian 13 (trixie)
3. Install all Stratum Gateway packages
4. Install the panel application
5. Configure base services (nftables, DNS, DHCP, Suricata, CrowdSec, WireGuard)
6. Configure the first network interface as LAN with address `192.168.1.1`
7. Reboot into the first-boot provisioning phase

The screen will display progress messages throughout. **Do not power off the machine during this process.**

Total time: approximately 15–20 minutes.

## First boot

After the initial installation completes, the system reboots into its **first boot**. During this phase, the gateway:

- Generates the local TLS certificate and session secret
- Sets the initial credentials for the panel and system accounts
- Configures DHCP (`192.168.1.100` through `192.168.1.200`)
- Starts the DNS resolver, IDS, and supporting services
- Starts the web management panel

The first boot takes **2 to 3 minutes**. When it completes, the gateway is ready.

## Connect and log in

### Access the panel

Connect a computer to one of the gateway's LAN ports (or configure another device on the same switch to use DHCP — it will receive an address in the `192.168.1.100–200` range).

Open a browser and navigate to:

~~~
https://192.168.1.1
~~~

Your browser will show a certificate warning (the gateway uses a locally generated CA that your browser hasn't yet trusted). This is expected on first access — accept the warning to proceed.

### Initial credentials

The initial username and password for both the panel and the system account are **displayed on the installation console** at the end of first boot. They are also written to a file on the gateway:

| Account | Username | Initial password |
|---|---|---|
| Panel administrator | `admin` | Shown on the install console |
| System / SSH | `stratum` | Shown on the install console |

To retrieve the initial password from a console or SSH session:

~~~
sudo cat /root/STRATUM_INITIAL_LOGIN.txt
~~~

Once you have changed the password, delete the file:

~~~
sudo rm /root/STRATUM_INITIAL_LOGIN.txt
~~~

### Forced password change

**On first login, you are required to change the initial password.** Until this change is completed, the rest of the panel is locked — no configuration, no dashboard, no network settings are accessible.

This is enforced on both the panel and the underlying system account. There is no way to skip this step.

Once you have set a new password, the panel unlocks and you can proceed to initial configuration.

> **Note:** The gateway's management interface is only reachable from the LAN side. The WAN interface does not expose the panel. This is intentional — the panel is not intended to be reachable from the public internet.

## First steps after login

Once signed in, we recommend the following order:

1. **Change the admin password** (prompted automatically)
2. **Set hostname and timezone** — under *System → Settings*
3. **Review interfaces** — under *Interfaces*, confirm the LAN interface is up
4. **Configure the WAN interface** — assign the second network interface to the WAN role and connect your internet uplink
5. **Verify connectivity** — the dashboard should show WAN status and external reachability
6. **Enable security modules** — Web Filtering, IDS, CrowdSec, and Geo-blocking each need to be turned on
7. **Configure backups** — under *Backup*, set a rotation policy and download an initial backup

The [Initial Configuration](/resources/documentation/initial-configuration/) guide walks through steps 2 through 7 in detail.

## Troubleshooting

### The system won't boot from the USB drive

- Verify the USB drive was written correctly — a partial write can produce a non-bootable device
- Check that the BIOS/UEFI boot order places the USB drive first
- If using UEFI, ensure the USB drive was written with a GPT partition scheme
- If using legacy BIOS, ensure the USB drive was written with an MBR partition scheme
- Try a different USB port — USB 2.0 ports are sometimes more reliable for legacy boot
- On some systems, Secure Boot must be disabled to boot the installer

### The installation starts but stalls

- Check that the target disk is properly connected and has no existing partition table issues
- If installing to a virtual machine, ensure the disk size meets the minimum requirements
- Give it time — some stages (package installation) can appear idle for a few minutes

### I can't reach the panel at 192.168.1.1

- Confirm your computer is connected to a LAN port (not WAN)
- Confirm your computer received a DHCP address in the `192.168.1.x` range
- If your computer uses a static IP on a different subnet, switch it to DHCP temporarily
- Try pinging `192.168.1.1` from a terminal — if the ping succeeds but the panel does not open, check that you are using **https://** (not http)

### The browser shows a security warning

This is expected on first access. The gateway uses a locally generated certificate authority. To eliminate the warning permanently, you can install the gateway's CA certificate on your computer — this is covered in the [Initial Configuration](/resources/documentation/initial-configuration/) guide.

### I forgot the initial password

If you skipped writing down the credentials and can no longer access the panel, the initial credentials are still stored on the gateway. Access the local console (or SSH as `stratum`) and read:

~~~
sudo cat /root/STRATUM_INITIAL_LOGIN.txt
~~~

If you have already changed the password and forgotten it, you can reset it from the console using the provided recovery utility:

~~~
sudo stratum-panel-set-password
~~~

---

## Next steps

<div class="card-grid grid-2" style="margin-top:32px;">

  <div class="card">
    <h3>Initial Configuration</h3>
    <p>Set hostname, timezone, WAN, security modules, and backups.</p>
    <a href="/resources/documentation/initial-configuration/" class="card-link">Continue →</a>
  </div>

  <div class="card">
    <h3>Need help?</h3>
    <p>If something isn't working as expected, reach out.</p>
    <a href="/company/contact/" class="card-link">Contact us →</a>
  </div>

</div>
