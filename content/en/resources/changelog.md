---
title: "Changelog"
description: "Release history of Stratum FW - new features, changes and fixes in every version."
---

All notable changes to Stratum FW are listed here, newest first. Stratum FW updates itself from the signed update server (System > Update) or from an offline update file (`.sfu`); every update creates a ZFS boot environment, so the previous version can be booted again at any time.

## 0.12.2 - 2026-11-03

### Added
- **System > Settings > Firewall & NAT**: Blocked connections dropped silently or rejected; logging of the default block and of broadcast noise. Bogon and private network blocking and ping on the WAN address. State table size with a suggestion for the installed memory and the current usage, idle-connection timeouts, SYN flood protection. Packet normalisation: random IP ID, clearing the don't-fragment bit. **NAT reflection**: local devices reach a port forward through the public address. Static source ports for automatic outbound NAT. The defaults keep the generated pf configuration of earlier versions unchanged.

### Changed
- Settings the panel uses on its own (session timeout, sign-in notice, sign-in protection) no longer show the "Apply changes" bar, and Discard keeps them.

## 0.12.1 - 2026-11-03

### Fixed
- Clicking Apply could start the same apply twice: the apply bar's periodic refresh redrew an active Apply button while the first apply was still running, and the second one ran again right after Keep, asking to keep the same changes a second time. The bar now stays busy until the action has finished, and only one apply, confirm or rollback can run at a time (panel, console and rollback timer).
- A WAN port forward on a management port (for example 443) no longer blocks saving any configuration. The forwarded port is left out of the WAN management rule (pf sends it to the forwarded host anyway) and Admin Access shows a notice.

## 0.12.0 - 2026-11-03

### Added
- **System > Settings** with tabs, replacing System > General. More tabs (Firewall & NAT, Logging, Notifications, Networking, Tunables) follow in the next releases.
- **General**: hostname, local domain, time zone, and a sign-in notice shown on the sign-in page.
- **Admin Access**: Web panel port and the HTTP-to-HTTPS redirect, session timeout. Who may manage the firewall: all internal networks or only selected interfaces; anti-lockout rule on LAN. Access from WAN, with a list of allowed source addresses. Sign-in protection with adjustable attempts, period and block time, a list of blocked addresses and an Unblock button. SSH: on or off, port, password or key-only sign-in, root's authorised public keys. Saving is refused when the new settings would lock out the address you are connected from. nginx and sshd check the new settings before Apply.
- Console menu 6) **Management access**: shows the current settings, opens or closes WAN access, and resets management access to the defaults.

### Changed
- The anti-lockout and WAN management rules follow the configured panel and SSH ports.
- Existing SSH keys of root are taken over by the panel on the first save after the update.

## 0.11.0 - 2026-11-03

### Added
- **Certificates** (System > Certificates): manage the TLS certificate of the web panel. See the active certificate: names, issuer, expiry date with days left, key type and the SHA-256 fingerprint. Install your own certificate with intermediate certificates. Create a new self-signed certificate. The previous certificate is kept and can be restored from the page or with `stratum-cert restore`.
- Alert for administrators when a CA-issued panel certificate expires within 21 days.
- `stratum-cert` command: `show`, `selfsigned`, `install`, `restore`.

## 0.10.0 - 2026-11-03

### Security
- **IPv6 safety baseline**: IPv6 is not supported yet, and now it is guaranteed to stay off — every IPv6 packet is dropped before any pass rule, IPv6 stays disabled on all interfaces and is never forwarded.

### Added
- IPv6 design and release plan (`docs/ipv6.md`).

## 0.9.1 - 2026-11-03

### Changed
- When an Apply fails, the apply bar now says why: each failed check with its message, with a link to the full output under System Logs > Configuration. Failures are also written to the system log.
- `stratum-update run` on an up-to-date system reports "nothing to install" instead of an error.

## 0.9.0 - 2026-11-03

### Added
- **IP blocklists** (Firewall > Blocklists > IP): block known-bad addresses with pf.
  - Catalogue of 63 maintained IPv4 feeds derived from pfBlockerNG; Spamhaus DROP, Abuse.ch Feodo Tracker and Emerging Threats are recommended and preselected.
  - Direction: incoming from the internet, outgoing from local networks, or both.
  - Own allow and block lists and custom list URLs.
  - Hourly updates without Apply, live progress bar and update log, blocked packet counters, and "Check an address".
  - Safety: private and reserved ranges and networks larger than /8 are never blocked; the firewall's DNS, update and package servers are always allowed.
- **Installation media in two variants**: `stratum-fw-v0.9.0-vga.iso` and `stratum-fw-v0.9.0-serial.iso`.

## 0.8.0 - 2026-11-03

### Added
- **WireGuard** (VPN > WireGuard): remote access with WireGuard, built into FreeBSD - no extra packages.
  - Server settings: port, listening WAN, tunnel network, MTU, external address.
  - Choose exactly which LANs and VLANs peers may reach, plus extra networks.
  - Peers with key pairs generated on the firewall and preshared keys: the configuration is shown as a QR code or downloaded as a `.conf` file.
  - Live status per peer: connected, endpoint, last handshake, received and sent data.
- Private keys are kept outside the configuration file.

## 0.7.3 - 2026-11-02

### Added
- **OpenVPN advanced settings**: UDP or TCP, listening WAN, which LANs and VLANs VPN clients may reach, client-to-client traffic, several devices per user, maximum connections, DNS servers and domains, data ciphers, authentication digest, minimum TLS version, key renegotiation, keepalive, MTU/MSS, log level, and certificate-only login.
- **Fixed VPN address per user** (System > Users).
- The panel tells you when connection settings change and the users' profiles must be downloaded again.

### Changed
- VPN access checkboxes on the OpenVPN page take effect immediately.

## 0.7.2 - 2026-11-01

### Changed
- System > Users: OpenVPN access shown in its own VPN column.

## 0.7.1 - 2026-11-01

### Added
- **VPN-only accounts**: users with OpenVPN access but no web panel sign-in.
- Users list: last VPN login and failed VPN password attempts; download the `.ovpn` profile from the list.

### Changed
- Removing VPN access, disabling or deleting a user revokes the user's certificate immediately.
- OpenVPN profiles no longer use the deprecated `persist-key` option.

## 0.7.0 - 2026-11-01

### Added
- **OpenVPN remote access** (VPN > OpenVPN).
  - Built-in certificate authority (elliptic-curve keys, revocation list, tls-crypt).
  - Login with the user's certificate and password.
  - Per-user `.ovpn` profiles, certificate revocation, list of connected clients.
  - Firewall and NAT rules and DNS for VPN clients are created automatically.

## 0.6.13 - 0.6.19 - 2026-10-31

### Added
- **Unbound DNS resolver** (Services > DNS): validating resolver with DNSSEC, forwarder or full-recursion mode.
- **DNS blocklists** (Firewall > Blocklists > DNSBL), pfBlockerNG-style: catalogue derived from pfBlockerNG plus the HaGeZi lists, recommended defaults, own lists, "Check a domain", nightly updates, and an update screen with a percentage bar, current step and live log.
- **Dashboard**: update badge next to the version, live uptime, CPU temperature, PPPoE session address and a traffic chart for the last hour.

### Fixed
- DNS blocklists: links in list comments are no longer treated as entries; list sources, the update server and shared hosting platforms are never blocked.
- Applying the configuration with DNS blocklists enabled failed on 0.6.15.

## 0.6.6 - 0.6.12 - 2026-10-31

### Added
- **Offline updates**: download a signed update file (`.sfu`) and upload it on System > Update.
- The newest 5 boot environments are kept after every update.

### Fixed
- Booting an earlier boot environment works.
- Fresh installations report their version correctly.
- The online check fails fast without internet.

## 0.6.1 - 0.6.5 - 2026-10-31

### Added
- Update progress: percentage bar, step checklist with times, in-progress banner, and a "restarting" page.
- System alerts (IP forwarding off, update available) and an own account page with password change.

### Fixed
- IP forwarding always follows the configuration after apply, rollback and service starts.
- The web panel starts again by itself after an update.

## 0.6.0 - 2026-10-31

### Added
- **Users and roles** (System > Users): several accounts, administrator / operator / read-only roles, enable and disable, last login and failed attempts.
- **General settings** (System > General): hostname, local domain, idle session timeout.
- **Light theme** and follow-the-system mode.
- **Backup and restore** (System > Backup): download the configuration, verified restore, configuration history.
- **Signed software updates** (System > Update): signature-checked repository, a backup and a new ZFS boot environment before every update, live progress, rollback.
- Branded boot menu.

## 0.5.0 - 2026-10-30

### Added
- **Firewall Log**: live view with filters and rule names.
- **System Logs**: system, DHCP/DNS, PPPoE, NTP, authentication, panel and configuration logs.
- **Ping, Traceroute and DNS lookup**, and the **pf state table**.
- **Traffic Graphs** for every interface.

### Changed
- Broadcast and multicast noise is blocked without filling the log.

## 0.4.0 - 2026-10-30

### Added
- **DHCP Server**: settings per interface, static mappings, active leases.
- **DNS Resolver**: local domain, upstream servers, host and domain overrides.
- **NTP**: time servers, time zone.
- Time zone question in the console setup wizard.

## 0.3.0 - 2026-10-30

### Added
- **Interface assignments**: map ports to WAN, LAN and additional (OPT) interfaces.
- Interface settings: static, DHCP or none, gateway, MTU.
- **PPPoE** on every WAN-type interface.
- **VLANs** (802.1Q), assignable as WAN or LAN.

## 0.2.0 - 2026-10-30

### Added
- **Firewall rules** per interface with system rules shown, hit counters, drag-and-drop order, on/off switch.
- **Aliases** for hosts, networks and ports.
- **NAT**: port forwards and outbound NAT modes.
- Anti-lockout rule, bogon and private network blocking on WAN.
- **Safe apply**: every change is applied with an automatic rollback unless it is confirmed in time.

## 0.1.1 - 2026-10-30

### Changed
- Management access from WAN is closed by default.
- Overlapping interface networks are refused.

### Fixed
- Console selection on UEFI systems; serial console with both UEFI and BIOS.

## 0.1.0 - 2026-10-29

First release.

- FreeBSD-based firewall with a web panel (HTTPS) and a dashboard.
- One central configuration file, applied with automatic rollback.
- Console menu and first-boot setup wizard.
- Installer ISO: GPT for BIOS and UEFI, ZFS, fully offline installation.
