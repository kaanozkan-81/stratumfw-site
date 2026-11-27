---
title: "Stratum Firewall: HTTP/HTTPS Download Control, Disguised Executable Detection & Closed-by-Default Interfaces"
date: 2026-11-26
author: "Stratum Security"
translationKey: "v0-40-0-download-control"
description: "Stratum Firewall introduces Firewall > Download Control (Squid :3128 proxy, dedicated Inspection CA, magic-byte file-type verification that catches .exe disguised as .pdf/.jpg, Monitor & Block modes, 1-click Bypass list, and instant mid-Apply error rollback), following closed-by-default interfaces and the Internet (not private networks) rule destination."
lead: "Inspect HTTP and HTTPS downloads by their real magic bytes — catching .exe programs disguised as .pdf or .jpg — while starting every new VLAN closed by default with one-click internet-only rules."
images: ["/images/download-control-hero.jpg"]
tags: ["firewall", "stratum firewall", "freebsd 15.1", "download control", "squid", "proxy", "inspection ca", "zero trust", "vlan"]
---

![Stratum Firewall HTTP/HTTPS Download Control & Magic-Byte Content Inspection](/images/download-control-hero.jpg)

Two consecutive releases on [Stratum Firewall](https://stratumfw.com/) — (`2026-11-25`) and (`2026-11-26`) — harden both how devices leave your networks and what files come back in:

1. **`Firewall > Download Control` + `Inspection CA` + Instant Mid-Apply Undo:** Checks HTTP and HTTPS file downloads across chosen internal networks by inspecting the **first bytes of the file itself**, catching disguised executables (`invoice.pdf` that is really a Windows `.exe`), offering **Monitor** and **Block** modes, a one-click **Bypass list** (`gov.tr`, banks, OS & security updates), and a searchable **Downloads** audit log.
2. (Closed-by-Default Interfaces + `"Internet (not private networks)"`):** Fresh installations and factory resets now start **every interface closed** like a zone-based firewall (while keeping LAN web-panel anti-lockout and DHCP working), paired with a dedicated **`Internet (not private networks)`** rule destination and a one-click **`Allow internet access`** button.

Full reference documentation is live in [Chapter 06.9 — Firewall > Download Control](/manual/firewall-download-control/), [Chapter 09.6 — System > Certificates](/manual/system-certificates/#inspection-ca), [Chapter 06.1 — Firewall > Rules](/manual/firewall-rules/), and the [Official Changelog](/download/#changelog).

![Stratum Firewall Download Control & Zero-Trust Interfaces Architecture](/images/download-control-architecture.svg)

### 1. `Firewall > Download Control`: True File Types & Disguised Programs

![Firewall > Download Control > Settings](/images/manual/dlctl-settings.png)

Attackers routinely rename an executable installer or trojan as `efatura.pdf` or `scan_09.jpg` hoping users will double-click it, or deliver scripts over HTTPS links that DNS blocklists have not yet seen. **Download Control** runs a Squid web proxy on the firewall's own address (`:3128`) on the internal interfaces you choose:

- **What the file really is (First-Byte Inspection):** Decisions combine the **file name**, the **type the site claims**, and the **first bytes of the file itself** — recognising Windows, Linux and macOS programs, `MSI`, `APK`, `JAR`, `MSIX`, `ZIP`, `RAR`, `7z`, `CAB`, `PDF` and Office documents, when a file's name looks like a program, archive or disk image (such as Google `response.bin` JSON attachments) but its content is plainly text, the content wins, and `gzip`- or `deflate`-compressed transfers are opened far enough to inspect their first bytes — while ignoring normal web page parts (page scripts, inline images, compressed HTML).
- **Disguised programs blocked regardless of category:** An `.exe` renamed `.jpg` or `.pdf` is flagged from its binary header and can be blocked whatever file-type categories are ticked.
- **Start in `Monitor`, enforce in `Block`:** Run **Monitor** for a few days to record what *would* be blocked without interrupting anyone; switch to **Block** to replace blocked downloads with a clean browser page sharing your [Block page](/manual/firewall-application-control/#block-page) design.
- **1-Click Bypass List, `Always allowed` & Fail-Open:** Add banks, government sites (`gov.tr`), OS updates, antivirus updates and certificate-pinned apps to the **Bypass list** (never opened or checked) with one click; place trusted internal or partner hosts in **Always allowed** (checked and recorded, never blocked). If the checker stops, downloads pass through (**fail-open**) while PC antivirus software continues its own checks.

![Firewall > Download Control > Downloads Audit Tab](/images/manual/dlctl-downloads.png)

Every inspected download is recorded under **Downloads** (1–365 days retention, **30 days** by default) with time, device, site, file, true type, size and result — accessible to **Administrators only**, with every view written to the system log.

### 2. Dedicated `Inspection CA` & Instant Mid-Apply Error Undo

![System > Certificates — Web Panel, Block Page CA & Inspection CA](/images/manual/certs.png)

- **`System > Certificates > Inspection CA`:** Separate from the **Block Page CA**, downloadable as **`.cer`** (Windows & Active Directory Group Policy) or **`.crt`** (PEM), with its **SHA-256 fingerprint** displayed for client verification and its private key stored strictly on the firewall.
- **Halfway-Failed `Apply` Undone at Once:** If an unexpected error interrupts an **Apply** after the previous state has been saved, Stratum FW now rolls back to that saved state **immediately** instead of waiting for the 120-second confirmation timer — reporting the rollback (or exact recovery command) in the message and logging precisely where the error occurred.

### 3. Every Interface Starts Closed & `"Allow internet access"`

![Firewall > Rules — Closed Interface Banner, Allow Internet Access & Internet Destination](/images/manual/fw-rules-allow-internet.png)

| Enhancement | How It Simplifies Secure VLAN Segmentation |
| :--- | :--- |
| **Every Interface Starts Closed** | New installations and factory resets no longer let LAN and VLANs reach everything (or each other); LAN web-panel anti-lockout and DHCP stay active, and existing installations keep their rules |
| **`Internet (not private networks)`** | Matches any public destination except `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` and `100.64.0.0/10` (unlike *WAN net*, which is only the WAN link's subnet) |
| **`Allow internet access` Button** | Shown on any interface without custom rules — one click adds **This firewall** (DNS, NTP, ping) plus **Internet (not private networks)** while keeping all other internal VLANs isolated |

### Try or Upgrade Stratum Firewall

Upgrade any Stratum Firewall appliance in one click via **`System > Update`** (`stratum-update run`) with automatic OpenZFS Boot Environment rollback protection, read [Chapter 06.9 in the User Manual](/manual/firewall-download-control/), grab installer media on [Download](/download/), or explore the interface in the [Live Demo](https://demo.stratumfw.com).
