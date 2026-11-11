---
title: "File & Path Reference"
description: "Reference guide to Stratum Firewall's configuration files, root-only secret storage, generated FreeBSD daemon configs, logs, and ZFS boot environments."
lead: "Transparent by design — see where Stratum stores its single declarative configuration, cryptographic secrets, generated service files, and logs."
---

Stratum Firewall never locks your settings inside opaque binary databases. All configuration state is driven from one auditable JSON file, while sensitive keys remain isolated in root-only files on your ZFS pool.

## 1. Source of Truth &amp; Isolated Secrets

| Path / Location | Purpose |
| :--- | :--- |
| `config.json` | Single declarative source of truth for interfaces, rules, NAT, aliases, services, and system settings, versioned with configuration history and diffs. |
| **Root-Only Secret Files** (`0600`) | Kept strictly outside `config.json`: panel user password hashes, WireGuard private/preshared keys, OpenVPN CA/server/user keys &amp; `tls-crypt`, PPPoE passwords, SMTP/Telegram notification tokens, SNMP community, SFTP backup password, and hashed MCP tokens. Included when exporting an encrypted `.stbk` backup with secrets enabled. |

## 2. Generated System Configurations (Managed by `stratum-config`)

Every **Apply** validates and regenerates native FreeBSD configuration files from `config.json` (do not hand-edit generated files, as the next Apply will overwrite them):

| Generated Target | Daemon / Subsystem | Notes |
| :--- | :--- | :--- |
| `/etc/pf.conf` &amp; `pf` table files | FreeBSD `pf` &amp; `dummynet` | Filter rules, NAT, policy routing, shaper pipes, IP blocklists, and Hostname/Country (GeoIP) tables. |
| `/etc/rc.conf` | FreeBSD `rc(8)` | Interface addresses, VLANs, daemons (VIPs and static routes are reconciled dynamically by `stratum-netextra` rather than cluttering `rc.conf`). |
| `/etc/sysctl.conf.local` | Kernel Tunables (`v0.12.6`) | Managed from **System → Settings → Tunables**; any pre-existing file is preserved as `/etc/sysctl.conf.local.stratum-orig`. |
| Unbound &amp; `dnsmasq` configs | DNS, DNSBL &amp; DHCP | DNSSEC trust anchor, RPZ DNSBL zones, 55-category Web Filter zones, and DHCP static mappings. |
| Suricata (`netmap` / `pcap`) | Intrusion Detection / IPS | Supervised per-engine configs and ET Open ruleset categories. |
| OpenVPN &amp; `wg0` | Remote Access VPNs | Server configs, CRL, `multihome` flags, and kernel `if_wg` peer definitions. |
| `nginx` &amp; `sshd` | Management Plane | Verified via `nginx -t` and `sshd -t` prior to every Apply. |

## 3. Logs, Rotation &amp; ZFS Boot Environments

* **System &amp; Security Logs (`/var/log/`):** Managed under **System → Settings → Logging** with configurable file sizes, retention counts, and optional **Remote Syslog** forwarding (up to 2 UDP syslog servers with readability-parsed `filterlog`, auth, system, and DHCP/DNS streams).
* **Blocklist &amp; Web Activity Databases:** Stored in root-only SQLite databases on the appliance (`7-day` retention for Unified Blocklist Reports; `1–730` days configurable for Web Activity; `24-hour` auto-expiry for `.pcap` packet captures).
* **ZFS Boot Environments (`zroot/ROOT/*`):** Every online or `.sfu` offline update snapshots the active root dataset into a bootable ZFS environment (retaining the newest 5 environments automatically).
