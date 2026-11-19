---
title: "Stratum Firewall v0.36.0 – v0.36.1: Privilege-Separated Captive Portal, RADIUS Accounting & MikroTik Rate-Limit Support"
date: 2026-11-19
author: "Stratum Security"
translationKey: "v0-36-1-captive-portal-radius"
aliases:
  - /blog/stratum-firewall-v0-36-0-privilege-separated-captive-portal/
description: "Stratum Firewall v0.36.0 and v0.36.1 introduce Services > Captive Portal with unprivileged web isolation, FreeRADIUS authentication & accounting, WISPr and Mikrotik-Rate-Limit per-device dummynet queues, and hitless pf reloads."
lead: "Run enterprise, campus, and hotel guest Wi-Fi portals on FreeBSD 15.1 with privilege-separated security, full RADIUS accounting, and drop-in MikroTik bandwidth attribute compatibility."
images: ["/images/hardware-stratum-family.jpg"]
tags: ["firewall", "stratum firewall", "freebsd 15.1", "captive portal", "radius", "freeradius", "mikrotik", "v0.36.1"]
---

Guest networks in hotels, campuses, dormitories, clinics, and multi-branch offices require two things that rarely come together cleanly: **minimal attack surface toward untrusted BYOD devices** and **seamless integration with existing RADIUS accounting and bandwidth-management servers**.

With **Stratum Firewall v0.36.0 and v0.36.1** on [Stratum Firewall](https://stratumfw.com/), **`Services > Captive Portal`** delivers a pfSense-style multi-zone guest portal with privilege-separated security, full `FreeRADIUS` authentication and accounting, per-device `dummynet` bandwidth queues, and drop-in `Mikrotik-Rate-Limit` support. Every change is listed in the [Official Changelog](https://stratumfw.com/resources/changelog/).

![Stratum Firewall v0.36.1 Captive Portal, RADIUS NAS, and Per-Device dummynet Architecture](/images/captive-portal-architecture.svg)

### 1. Multi-Interface Zones, OS Auto-Popup & Three Sign-In Methods

Each Captive Portal **zone** groups one or more internal interfaces (such as Guest Wi-Fi or Dormitory VLANs):

- **Strict Pre-Sign-In Boundary:** Before signing in, a device can reach only DNS on the firewall, the sign-in page, and the zone's allowed pre-sign-in destinations. Outbound `http://` requests redirect to the portal so phones and laptops open the sign-in window automatically via their native connectivity checks.
- **Three Flexible Onboarding Modes:**
  1. **Passwordless (Terms Acceptance):** Visitors accept your terms of use and connect immediately.
  2. **Local User Accounts (`bcrypt`):** Optional account expiry dates, concurrent device limits per user, and temporary IP lockouts after repeated wrong passwords.
  3. **RADIUS Server (`v0.36.1` — `FreeRADIUS` & Standard RADIUS):** Authenticates via `PAP` with **`Message-Authenticator` on every request**, cryptographically verifying every server reply against the shared secret (`0600` root-only file, never shown again), with automatic failover to a **secondary RADIUS server** if the primary does not answer.

### 2. Full RADIUS Accounting & Drop-In `Mikrotik-Rate-Limit` / `WISPr` Queues (`v0.36.1`)

Migrating a hotel, dormitory, or campus gateway shouldn't force you to rewrite your existing RADIUS database or compliance logging pipeline:

- **Start, Interim-Update & Stop Accounting:** Stratum FW reports session duration, RX/TX byte counters, and exact disconnect cause (`signed out`, `idle`, `time limit`, `administrator`, etc.) while echoing the server's `Class` attribute — so RADIUS session records and downstream logs cover devices behind Stratum FW just like any other NAS.
- **Dynamic Timeout & Bandwidth Attributes:** Honors `Session-Timeout`, `Idle-Timeout`, and `Acct-Interim-Interval`, plus per-user speed limits via **`WISPr-Bandwidth-Max-Up/Down`** or **`Mikrotik-Rate-Limit`** — allowing an existing MikroTik + FreeRADIUS hotspot deployment to switch to Stratum FW **unchanged**.
- **Per-Device `dummynet` Bandwidth Limits:** Every zone also supports default per-device download/upload caps (`Mbit/s`) across any sign-in method. Each signed-in client receives its own `dummynet` queue, with RADIUS-supplied rates taking precedence over the zone default. Live traffic and active speed limits are displayed right beside each signed-in device.

### 3. Privilege-Separated Security, Dashboard Visibility & Hitless `pf` Reloads

Because every untrusted device on a guest network can reach the captive portal listener before authenticating, Stratum FW isolates the web process strictly (see [Security & Verification](https://stratumfw.com/product/security/)):

| Architectural Layer | How `Services > Captive Portal` Works in `v0.36.0 – v0.36.1` |
| :--- | :--- |
| **Guest-Facing Web Process** | Runs as an **unprivileged user** and holds **zero secrets**, hashes, or RADIUS keys |
| **Root Verification Socket** | Separate `root` service verifies local `bcrypt` hashes or signed RADIUS replies over a local socket |
| **Hitless Session Continuity** | Signed-in guests **stay connected** across `pf` reloads, `120 s` Safe-Apply, and portal restarts |
| **MAC Check & Exceptions** | Signs out IP takeovers automatically; bypasses printers, TV boxes, and allowed destinations |
| **Dashboard Panel & RBAC** | Live **Captive portal** Dashboard widget warns if RADIUS fails to confirm accounting; **Operators** manage accounts & sessions |

### Deploy or Upgrade to `v0.36.1`

- **One-Click Online Upgrade:** Existing appliances update directly to signed `v0.36.1` under **`System > Update`** (`stratum-update run`) with automatic OpenZFS Boot Environment (`bectl`) protection.
- **Explore Hardware & Downloads:** Pair `v0.36.1` with our [4-Port, 8-Port SFP+ & 12-Port 1U Hardware Profiles](https://stratumfw.com/product/hardware/) or any `amd64` server/VM via [Download Stratum Firewall](https://stratumfw.com/download/), test the UI in the [Live Demo](https://demo.stratumfw.com), and follow releases on [GitHub (`stratum-panel`)](https://github.com/kaanozkan-81/stratum-panel).
