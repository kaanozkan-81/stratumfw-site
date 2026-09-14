---
title: "Initial Configuration"
lead: "Set up your gateway after first login — hostname, timezone, WAN, security modules, and backups."
---

Once you have signed in to the panel for the first time and changed the initial password, the gateway is functional but not yet configured for your network. This guide walks through the essential first steps.

If you haven't installed Stratum Gateway yet, start with the [Installation guide](/resources/documentation/installation/).

## Overview

The essential steps, in order:

1. Change the admin password (prompted automatically)
2. Set hostname and timezone
3. Review interfaces
4. Configure the WAN interface
5. Verify connectivity
6. Enable security modules
7. Configure backups

Each step is covered in detail below.

## Step 1 — Change the admin password

On first login, you are automatically prompted to change the initial password. This step cannot be skipped.

Choose a strong password — at least 12 characters, ideally a passphrase. The password protects the entire gateway configuration.

After the change, delete the initial credentials file from the system console:

~~
sudo rm /root/STRATUM_INITIAL_LOGIN.txt
~~

## Step 2 — Set hostname and timezone

Navigate to **System → Settings**.

### Hostname

Set a hostname that identifies this gateway on your network. Useful conventions:

- `gateway` — simple, unambiguous
- `gw-office` — site-prefixed
- `fw-hq` — firewall at headquarters

The hostname appears in logs, notifications, and SNMP queries. Choose something meaningful.

### Timezone

Set the timezone to match your location. This affects:

- Log timestamps
- Scheduled tasks (backups, updates, feed refreshes)
- Notification delivery times

Save the settings. The gateway may briefly restart affected services.

## Step 3 — Review interfaces

Navigate to **Interfaces**.

You should see the physical network interfaces detected by the gateway. At minimum:

- The **LAN** interface — already configured with `192.168.1.1/24`
- The **WAN** interface — not yet assigned

Verify the following:

- The LAN interface shows as **up**
- The interface names match your hardware (e.g., `enp1s0`, `enp2s0`)
- No interface is in an unexpected state

If you have more than two interfaces, you'll configure additional ones later as OPT ports (for DMZ, IoT, guest networks, etc.).

## Step 4 — Configure the WAN interface

Still under **Interfaces**, assign the second physical interface to the WAN role.

### Assign the WAN role

Click the interface you want to use for WAN, then select **WAN** as its role. The gateway will prompt you to choose a connection mode.

### Choose a WAN connection mode

| Mode | Use when |
|---|---|
| **DHCP** | Your ISP assigns an address automatically (most common) |
| **Static IP** | Your ISP provided a fixed IP, gateway, and DNS |
| **PPPoE** | Your ISP requires PPPoE authentication (common with DSL/fiber) |
| **Reserved Network** | For special configurations (advanced) |

For most home and small-business setups, **DHCP** is correct. For static IPs, enter the address, subnet mask, gateway, and DNS servers provided by your ISP. For PPPoE, enter the username and password from your ISP.

### Apply and confirm

When you click **Apply**, the gateway uses its safe-apply mechanism:

1. The change is validated
2. A confirmation prompt appears with a countdown
3. The change is applied
4. You must confirm connectivity within the timeout window
5. If you don't confirm, the change is rolled back automatically

This prevents a misconfiguration from locking you out.

## Step 5 — Verify connectivity

After the WAN is configured, verify that the gateway can reach the internet.

Navigate to the **Dashboard**. You should see:

- WAN interface status: **up**
- External IP address (if DHCP or PPPoE)
- Default gateway reachable
- DNS resolution working
- Traffic counters incrementing

### Test from a LAN client

From a computer connected to the LAN, test connectivity:

~~bash
ping 1.1.1.1
ping google.com
~~

The first command tests raw IP connectivity. The second tests DNS resolution. If the first works but the second doesn't, DNS is misconfigured.

### Check DNS resolution

If DNS isn't working, verify the Unbound resolver is running:

Navigate to **Services** and confirm that **unbound** shows as running. If it isn't, start it and check the logs under **Logs → System**.

## Step 6 — Enable security modules

Stratum Gateway ships with security modules available but not enabled by default. Enable them in the order below.

### Web Filtering

Navigate to **Web Filter**. Enable the feed catalog and select the categories you want to block. Common starting points:

- Ads and trackers
- Malware and phishing
- Adult content (for business and education environments)

The feeds update automatically on a schedule you choose.

### Intrusion Detection (Suricata)

Navigate to **IDS**. Select the WAN interface for monitoring. Suricata will begin inspecting traffic for known attack signatures.

Review and tune rule sets over the first few days — the initial deployment is often noisy until you suppress signatures that fire on legitimate traffic.

### CrowdSec

Navigate to **CrowdSec**. Enable the engine and the firewall bouncer. CrowdSec watches for behavioral patterns (brute-force, scanning) and blocks malicious IPs automatically.

If you enable the community blocklist, you'll also receive protection from threats observed across the CrowdSec network.

### Geo and Threat IP Blocking

Navigate to **Geo Block**. Configure country-level blocking if your business doesn't operate globally. Enable the threat IP feed to block known-malicious sources.

Start conservatively — blocking too much can break legitimate traffic. Review the logs after enabling.

## Step 7 — Configure backups

Navigate to **Backup**.

Stratum supports module-level backups (each module can be backed up independently) and full-system backups. For routine operations, module-level is usually more useful — you can restore just the firewall rules or just the VPN config without touching anything else.

### Set a rotation policy

Configure automatic backups to run on a schedule. A reasonable starting policy:

- **Daily** module-level backups of critical modules (Rules, NAT, VPN, Interfaces)
- **Weekly** full-system backup
- Keep the last **7 daily** and **4 weekly** backups

Storage requirements are minimal — a full-system backup is typically under 1 MB compressed, since only configuration is captured (not packages or system binaries).

### Download an initial backup

After configuring rotation, download a manual full-system backup and store it off the gateway. This is your recovery point if the hardware fails.

Navigate to **Backup → Create Backup**, then download the resulting file. Store it somewhere safe — a network share, cloud storage, or a local workstation.

## Additional configuration (recommended)

The following are not strictly required for the gateway to function, but most deployments benefit from them.

### Install the CA certificate on client devices

The gateway uses a local certificate authority to issue its own TLS certificates. Browsers will show a warning until you install the CA certificate on each client device.

Download the CA bundle from **System → Settings → Certificate Authority** and install it on your devices. The exact steps vary by operating system:

- **Windows:** double-click the `.crt` file → *Install Certificate* → *Local Machine* → *Trusted Root Certification Authorities*
- **macOS:** double-click → *Keychain Access* → add to *System* keychain → set to *Always Trust*
- **Linux (Firefox):** *Settings → Privacy & Security → Certificates → View Certificates → Authorities → Import*
- **Linux (system):** copy to `/usr/local/share/ca-certificates/` and run `sudo update-ca-certificates`

Once installed, the certificate warning disappears.

### Configure notifications

Navigate to **System → Notifications**. Configure SMTP settings to receive alerts about significant events:

- Failed login attempts
- Update available
- Interface down
- Backup completed or failed

Send a test notification to verify the configuration.

### Set up SNMP monitoring (optional)

If you already run a network monitoring system (Zabbix, PRTG, LibreNMS, etc.), enable SNMP under **Services → SNMP**. The gateway exposes standard MIBs for interface status, traffic counters, system resources, and service health.

### Configure NetFlow export (optional)

If you want flow-level visibility, configure NetFlow under **NetFlow**. The gateway can export flows to a local collector or a remote analysis tool.

## Where to go from here

Your gateway is now fully operational with essential security in place. From here:

<div class="card-grid grid-2" style="margin-top:32px;">

  <div class="card">
    <h3>Explore the features</h3>
    <p>Every module in the platform, with technical detail on each.</p>
    <a href="/product/features/" class="card-link">Browse features →</a>
  </div>

  <div class="card">
    <h3>Review the FAQ</h3>
    <p>Common questions about deployment, capabilities, and support.</p>
    <a href="/resources/faq/" class="card-link">Read FAQ →</a>
  </div>

  <div class="card">
    <h3>See the changelog</h3>
    <p>Release history and what changed in each version.</p>
    <a href="/resources/changelog/" class="card-link">View changelog →</a>
  </div>

  <div class="card">
    <h3>Contact support</h3>
    <p>If something isn't working or you have a specific question.</p>
    <a href="/company/contact/" class="card-link">Get in touch →</a>
  </div>

</div>
