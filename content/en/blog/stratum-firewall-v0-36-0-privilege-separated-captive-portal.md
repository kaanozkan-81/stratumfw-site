---
title: "Stratum Firewall v0.36.0: Privilege-Separated Guest Captive Portal with Hitless pf Persistence"
date: 2026-11-19
author: "Stratum Security"
translationKey: "v0-36-0-captive-portal"
description: "Stratum Firewall v0.36.0 introduces Services > Captive Portal for guest networks — multi-interface zones, terms-accept or local bcrypt accounts, MAC checks, Operator delegation, and privilege-separated authentication that survives pf reloads."
lead: "Run customizable English/Turkish guest Wi-Fi sign-in portals on FreeBSD 15.1 without exposing secrets to the guest web process or dropping connected guests on Apply."
images: ["/images/og-stratum-firewall.jpg"]
tags: ["firewall", "stratum firewall", "freebsd 15.1", "captive portal", "guest wifi", "pf", "v0.36.0"]
---

Guest networks on hotels, campuses, clinics, and branch offices expose a web server directly to untrusted smartphones and laptops before a single firewall pass rule applies. In **Stratum Firewall v0.36.0** on [Stratum Firewall](https://stratumfw.com/), we added **`Services > Captive Portal`** in the manner of pfSense, engineered around strict privilege separation and uninterrupted guest sessions. Full release details are listed in the [Official Changelog](https://stratumfw.com/resources/changelog/).

![Stratum Firewall v0.36.0 Privilege-Separated Captive Portal Architecture](/images/captive-portal-architecture.svg)

### 1. Multi-Interface Zones & Automatic OS Sign-In Popups

Each Captive Portal **zone** binds one or more internal interfaces (such as a Guest VLAN or Visitor Wi-Fi interface):

- **Strict Pre-Sign-In Isolation:** An unauthenticated device can reach only DNS on the firewall, the captive portal sign-in page, and any pre-sign-in destinations explicitly allowed on that zone.
- **Native Connectivity Detection:** Outbound `http://` requests from unauthenticated clients are redirected to the portal, causing iOS, Android, macOS, Windows, and Linux devices to open the sign-in screen automatically through their built-in connectivity checks.
- **Standard `pf` Rules After Sign-In:** Once a guest signs in, the interface's own firewall rules, [Security Guard](https://stratumfw.com/product/security-guard/) Web/DNS filters, and `dummynet` per-device bandwidth limits apply just like any other client.

### 2. Click-Through Terms or Local Accounts with Device Limits & MAC Checks

Administrators can choose the right onboarding workflow per zone:

- **Sign-In Without a Password:** Visitors accept your terms of use and connect with one tap.
- **Local User Accounts:** Create guest or event accounts with an optional **expiry date** and a **maximum concurrent device limit per user**. Repeated wrong passwords lock the offending IP address out for a few minutes.
- **Session, Idle & MAC Enforcement:** Configure hard session durations and idle timeouts (when no new traffic flows). Built-in **MAC address verification** signs an address out immediately if another device takes over that IP, and changing a zone's sign-in method cleanly requires devices to sign in again under the new mode.
- **Device & Destination Exceptions:** Whitelist headless devices that never sign in (such as network printers or TV boxes) alongside allowed pre-sign-in destinations.

### 3. Unprivileged Guest Web Process & Hitless `pf` Persistence

Because every unauthenticated device on a guest VLAN can talk to the portal page, Stratum FW keeps the guest-facing attack surface minimal as documented on our [Security & Verification](https://stratumfw.com/product/security/) page:

| Architectural Layer | How `Services > Captive Portal` Operates in `v0.36.0` |
| :--- | :--- |
| **Guest-Facing Web Process** | Runs as an **unprivileged user** and holds **zero secrets** or password files |
| **Credential Verification** | Verified by a separate **`root` service** over a local Unix socket |
| **Password Storage** | Stored strictly as **`bcrypt` hashes** in a root-only file; never sent to the browser |
| **Live Session Continuity** | Signed-in devices **stay connected** across `pf` reloads, configuration `Apply`, and service restarts |
| **Role Separation & Audit** | **Operators** manage guest accounts and disconnect live devices; **Administrators** configure zones; every sign-in/out is logged |

### 4. Customizable English / Turkish Mobile & Dark-Mode Portal

Every zone lets you customize the **page title, welcome message, terms of use, button text, accent colour, logo, and language (English or Turkish)** — adapting automatically to phone screens and dark mode with zero external CDN dependencies.

### Upgrade or Try `v0.36.0`

- **Existing Deployments:** Update in one click via **`System > Update`** (`stratum-update run`), backed by an automatic OpenZFS Boot Environment (`bectl`) snapshot.
- **New Deployments:** Download the FreeBSD 15.1 `amd64` installer on [Download Stratum Firewall](https://stratumfw.com/download/) and update to `v0.36.0`, explore the UI on the [Live Demo](https://demo.stratumfw.com), review campus and SMB blueprints under [Education & Campus Solutions](https://stratumfw.com/solutions/education/), or follow releases on [GitHub (`stratum-panel`)](https://github.com/kaanozkan-81/stratum-panel).
