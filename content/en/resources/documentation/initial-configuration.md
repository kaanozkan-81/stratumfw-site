---
title: "Initial Configuration & Two-Factor Sign-In"
description: "Set up Stratum Firewall after installation — TOTP 2FA with recovery codes, Admin Access hardening, WAN/LAN & DHCP Multi-WAN interfaces, Force DNS, Safe-Apply, and ZFS updates."
lead: "Bring your newly installed Stratum Firewall appliance online securely — TOTP two-factor sign-in, admin hardening, interfaces, DNS/DHCP, and signed ZFS updates."
---

Once you have installed Stratum Firewall from the [VGA ISO or Serial IMG](/resources/documentation/installation/) and opened `https://192.168.1.1/` in your browser, follow these steps to complete your production baseline.

## 1. General Settings, Admin Hardening &amp; TOTP 2FA (`v0.30.0`)

Navigate to **System → Settings** and **My Account**:

* **Two-Factor Sign-In (`My Account`, `v0.30.0`):** Enable **TOTP 2FA** using *Google Authenticator*, *Microsoft Authenticator*, *1Password*, *Bitwarden*, or any standard authenticator app:
  * **100% On-Box Privacy:** The setup QR code is rendered locally in your browser from a bundled library—the secret key **never leaves the firewall** and is never displayed again after setup.
  * **Replay &amp; Brute-Force Protection:** Every 6-digit code works only once (`±30s` clock tolerance), the challenge screen expires after 5 minutes, failed codes count toward **Sign-in Protection**, and enabling 2FA immediately ends your other active sessions.
  * **10 One-Time Recovery Codes:** Generated for lost-phone emergencies (using a recovery code raises a system notification). Administrators can also switch off a locked-out user's 2FA under **System → Users**, or from the physical/serial console password reset in `stratum-menu`.
* **General Tab &amp; Boot Tunes (`v0.28.5` – `v0.28.6`):** Set the **Hostname**, **Local Domain** (e.g., `lan.example.com`), **Time Zone**, optional **Sign-in Notice**, and choose a PC-speaker **Start-up &amp; Shutdown Tune** (*Stratum*, *Classic beep*, *Westminster Chime*, *Ode to Joy*, *Für Elise*, or *Arcade*) with a live **Test** button (machines without a PC speaker stay silent).
* **Admin Access &amp; Certificates:** Choose your web panel HTTPS port, restrict management to trusted internal interfaces (WAN access is closed by default), configure SSH keys/ports without risking self-lockout, and manage TLS certificates under **System → Certificates**.

## 2. Interfaces, 802.1Q VLANs &amp; Additional WANs

1. Navigate to **Interfaces → Assignments** to map physical NICs (`igb0`, `vtnet0`, etc.) or **802.1Q VLANs** (`Interfaces → VLANs`) to `WAN`, `LAN`, and `OPT1..N` roles (`OPT` ports can serve as internal VLANs or secondary/tertiary WAN links).
2. Configure your **WAN** (and any optional `WAN2` / `WAN3`) under **Interfaces**:
   * **DHCP (`v0.29.1`):** Supported on both primary `WAN` and additional WANs (such as cable, 4G/5G LTE, or Starlink modems—secondary DHCP WANs never hijack your default route or system DNS on their own).
   * **Static IPv4** or **PPPoE (`mpd5`):** Enter your ISP gateway/MTU or fiber/DSL credentials (passwords are stored in root-only `0600` files).
3. Click **Save**, then click **Apply changes** right beside Save. Stratum validates generated configs (`pfctl -n`, `nginx -t`, `unbound-checkconf`) and starts the **60-second Safe-Apply timer**—click **Keep changes** to commit permanently, or let it roll back automatically if connectivity was interrupted.

## 3. DHCP, Validating DNS &amp; Force DNS (`v0.29.2`)

* **DHCP Server (`Services → DHCP`):** Define address pools, one-click *"Make static"* MAC reservations from active leases, and per-VLAN gateway/DNS options.
* **DNS Resolver &amp; Force DNS (`Services → DNS`):** Run **Unbound** (DNSSEC-validating recursive or forwarder mode) or **dnsmasq** with automatic DHCP hostname registration. Enable **Force DNS through the firewall** (`v0.29.2`) on chosen internal networks so hardcoded client DNS (`8.8.8.8`) is transparently redirected to Stratum and DNS over TLS (`TCP 853`) is blocked—with an **Exception alias** for internal Active Directory domain controllers and zero browsing history recorded.

## 4. Check for Signed Software Updates (ZFS Boot Environments)

Navigate to **System → Update** and click **Check for updates** (or upload an offline `.sfu` update package on air-gapped networks). Before applying **v0.30.0** or any future release, Stratum automatically snapshots your root pool into a new **ZFS Boot Environment** (retaining the newest 5 environments) so you can always boot back into the previous version.
