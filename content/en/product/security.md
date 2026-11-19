---
title: "Security & Verification"
seo_title: "Stratum Firewall Security Model, Cryptography & Signed Updates"
description: "How Stratum Firewall verifies signed updates, isolates root-only secrets, relies on FreeBSD kernel & OpenSSL cryptography, and handles vulnerability disclosure."
seoDescription: "Verify Stratum Firewall security: signed FreeBSD packages & offline .sfu updates, 0600 isolated secrets, OpenSSL/kernel crypto, ZFS rollback & security@stratumfw.com."
translationKey: "product-security"
aliases: ["/security/"]
---

Stratum Firewall is engineered around verifiable operating-system primitives rather than opaque black-box daemons. Every cryptographic operation, update package, secret file, and configuration transition can be inspected directly on your own FreeBSD 15.1 appliance.

## 1. Cryptographic Architecture (Zero Custom Cryptography)

Stratum Firewall **does not implement cryptography of its own**:

* **WireGuard (`if_wg`):** Data-plane tunnel encryption runs inside the audited **FreeBSD 15.1 kernel `if_wg`** driver; Curve25519 keys are generated with **OpenSSL** (via the Python `cryptography` package, `v0.34.2+`).
* **Site-to-Site IPsec (`enc0`):** IKEv2 and IKEv1 key negotiation is handled by **`strongSwan`** (`swanctl`), while packet encryption (`AES-GCM`, `AES-CBC`, `SHA-2`, `DH 14-21`, `Curve25519`) executes in the FreeBSD kernel ESP stack (`enc0`).
* **OpenVPN & TLS:** Certificate authorities, EC server/client certificates, `tls-crypt` keys, and HTTPS session transport rely directly on system **OpenSSL**.

## 2. Signed Updates & Air-Gap Verification

* **Cryptographically Signed Repository:** Online update packages and the FreeBSD repository catalogue are signed; tampered, truncated, or altered packages are refused before extraction. Signature enforcement is tested on every release.
* **Signed Offline Bundles (`.sfu`):** Air-gapped and isolated management networks can upgrade using `.sfu` bundles, which carry the same signed repository metadata and are cryptographically verified on-box prior to installation.
* **Automatic ZFS Boot Environments (`bectl`):** Every online or offline update snapshots the OpenZFS root filesystem into a new boot environment first, so the previous working OS version can be booted again from the console or Web UI at any time.

## 3. Secret Isolation & Safe-Apply Rollback

* **Root-Only Secret Files (`0600`):** Sensitive credentials (VPN private keys, user password hashes, IPsec pre-shared keys, SFTP backup keys) live in root-only `0600` files outside `config.json` and are **never** sent back to the browser.
* **Pre-Checked Safe-Apply with Auto-Rollback:** Every configuration change is validated before activation (`pfctl -n`, `nginx -t`, `unbound-checkconf`, `dnsmasq --test`, `sshd -t`). If the administrator does not confirm reachability within the 120-second timeout, the previous state is restored automatically.
* **Open by Design:** Every generated daemon configuration (`config.json`, `pf.conf`, `swanctl.conf`, `wg*.conf`, `unbound.conf`) sits in plain, auditable text on your disk. Core FastAPI panel sources are currently maintained in a private repository during pre-`v1.0` iteration and will be published on GitHub.

## 4. Access Control, Local 2FA & Zero Outbound Telemetry

* **Local TOTP Two-Factor Authentication:** Administrator TOTP secrets and QR codes are generated locally on the appliance without calling external QR rendering services, paired with 10 single-use recovery codes and HTTPS `Secure` cookies.
* **Role-Based Access & Audit Logging:** Granular roles separate `Administrator`, `Operator`, and `Read-Only` accounts; sensitive logs (web filter activity, packet captures) are restricted strictly to administrators, and administrative access events are logged.
No telemetry: the appliance sends no usage analytics or configuration data. It connects out only for features an administrator turns on: signed update checks, IP blocklist, GeoIP, Web Filter, and Suricata rule downloads, DNS and NTP, notifications (Telegram, e-mail), remote backups (SFTP), and, if configured, the AI Assistant's chosen provider.

## 5. Reporting a Security Vulnerability

If you discover a security issue in Stratum Firewall, the installer media, or `stratumfw.com`, please report it privately so we can verify and ship a signed fix promptly:

* **Email:** [security@stratumfw.com](mailto:security@stratumfw.com)
* **RFC 9116 Policy File:** [`https://stratumfw.com/.well-known/security.txt`](/.well-known/security.txt)

Please include the affected Stratum FW version, reproduction steps, and impact assessment. We acknowledge vulnerability reports promptly and credit reporters in the [Official Changelog](/resources/changelog/) upon coordinated disclosure.
