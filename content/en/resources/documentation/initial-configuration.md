---
title: "Initial Configuration"
description: "Set up Stratum Firewall after installation — WAN/LAN interfaces, 802.1Q VLANs, DNS/DHCP, Admin Access hardening, Safe-Apply, and ZFS updates."
lead: "Bring your newly installed Stratum Firewall appliance online securely — interfaces, DNS, DHCP, admin hardening, and signed updates."
---

Once you have installed Stratum Firewall from the [VGA ISO or Serial IMG](/resources/documentation/installation/) and opened `https://192.168.1.1/` in your browser, follow these steps to complete your production baseline.

## 1. General Settings &amp; Admin Access Hardening

Navigate to **System → Settings**:

* **General Tab:** Set the appliance **Hostname**, **Local Domain** (e.g., `lan.example.com`), **Time Zone**, and optional **Sign-in Notice**.
* **Admin Access Tab:**
  * Choose the web panel HTTPS port and restrict management access to trusted internal interfaces (WAN management access is closed by default).
  * Configure **Sign-in Protection** (automatic IP lockout after repeated failed login attempts).
  * Manage **SSH** access (enable/disable, custom port, key-only authentication, and `root` authorized public keys). Saving any setting that would lock out your current IP address is refused automatically.
* **Certificates (`System → Certificates`):** Inspect the panel TLS fingerprint, generate a fresh self-signed certificate matching your hostname/IPs, or upload a custom/CA-signed TLS certificate.

## 2. Interfaces &amp; 802.1Q VLANs

1. Navigate to **Interfaces → Assignments** to map physical NICs (`igb0`, `vtnet0`, etc.) or **802.1Q VLANs** (`Interfaces → VLANs`) to `WAN`, `LAN`, and `OPT1..N` roles (`OPT` ports can act as either internal LAN segments or additional WAN links).
2. Configure your **WAN** under **Interfaces → WAN**:
   * **DHCP:** Obtains a public IP and gateway automatically from your ISP.
   * **Static IPv4:** Set your assigned IP/prefix, upstream gateway, and custom MTU if required.
   * **PPPoE (`mpd5`):** Enter your fiber/DSL ISP credentials; passwords are stored securely in root-only files.
3. Click **Save**, then click **Apply changes** right beside the Save button. Stratum validates the generated configurations (`pfctl -n`, `nginx -t`, `unbound-checkconf`) and starts a **60-second Safe-Apply timer**—click **Keep changes** to make them permanent, or let the timer expire to roll back automatically if connectivity was lost.

## 3. DHCP &amp; Validating DNS (Unbound / dnsmasq)

* **DHCP Server (`Services → DHCP`):** Define address pools, static MAC reservations (with one-click *"Make static"* from active leases), and custom gateway/DNS options per LAN/VLAN.
* **DNS Resolver (`Services → DNS`):** Use **Unbound** (validating DNSSEC resolver in full-recursion or forwarder mode) or **dnsmasq**; DHCP client hostnames resolve automatically across both engines, and you can add local host/domain overrides.

## 4. Check for Signed Software Updates (ZFS Boot Environments)

Navigate to **System → Update** and click **Check for updates** (or upload an offline `.sfu` update package on air-gapped networks). Before applying **v0.28.3** or any future release, Stratum automatically snapshots your current system into a new **ZFS Boot Environment** (retaining the newest 5 environments) so you can always boot back into the previous version if needed.
