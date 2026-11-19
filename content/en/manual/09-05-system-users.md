---
title: "System > Users"
seo_title: "09.5 System > Users — Stratum FW User Manual"
description: "Panel and VPN accounts: roles, VPN-only accounts, fixed VPN addresses, passwords, disabling, your own account page and two-factor sign-in with recovery codes."
weight: 905
chapter_num: "09.5"
url: "/manual/system-users/"
aliases:
  - "/manual/09-05-system-users/"
translationKey: "manual-09-05-system-users"
---

Give each person their own account: the logs then show who changed what, and an account can be closed when
someone leaves without changing everyone else's password.

An account can sign in to the **web panel**, connect with **OpenVPN**, or both.

![System > Users](/images/manual/users.png)

## The users list

The list shows each account's **User**, **Role**, **Status**, **VPN** access, **Last login** and **Last failed
attempt**, with actions to edit, download the OpenVPN profile and delete.

Changes to users take effect immediately - no Apply needed. Changing a password or disabling a user signs that
user out everywhere.

## Add or edit a user

**Add user**:

| Field | Meaning |
| :--- | :--- |
| **User name** | 2-32 characters, starting with a letter; letters, digits, `.`, `_` and `-`. Cannot be changed later. |
| **Full name / description** | For example *Front desk - night shift*. |
| **Role** | See below. |
| **Account enabled** | A disabled user cannot sign in and existing sessions end immediately. The account and its history are kept. |
| **May sign in to the web panel** | Off for a VPN-only account. |
| **May connect with OpenVPN** | Issues a personal certificate. Needs the OpenVPN server to be on ([**VPN > OpenVPN**](/manual/vpn-openvpn/)). |
| **Fixed VPN address** | Optional: the same VPN address at every connection - for firewall rules that name this user's address. |
| **Password**, **Repeat password** | At least 10 characters. When editing, leave empty to keep the current password. |

### Roles

| Role | May |
| :--- | :--- |
| **Administrator** | Everything. |
| **Operator** | View the pages and use the diagnostics - ping and traceroute, killing connections, Wake-on-LAN, clearing ARP entries - and do front-desk work: disconnect captive portal devices and manage captive portal users. No configuration changes. |
| **Read-only** | View only. |

Some pages are for administrators only, because they show who did what or give access to secrets: **Users**,
**Backup**, **Update**, **Power**, the **Admin Access**, **SNMP** and **MCP** settings, **Web Activity**, the
Web Filter report, the blocklists' **Blocked** report, **Packet Capture**, the IPsec log, the **AI Assistant**,
and the downloads of OpenVPN profiles and WireGuard configurations.

The panel refuses changes that would leave no enabled administrator with panel access, and you cannot disable,
delete, demote or remove panel access from your own account - ask another administrator.

### VPN-only accounts

For staff who only need the VPN: switch off **May sign in to the web panel** and switch on **May connect with
OpenVPN**. The user then signs in to the VPN with the user name, password and their personal profile (`.ovpn`),
but cannot open the panel.

Download the profile from the users list or from [**VPN > OpenVPN**](/manual/vpn-openvpn/). **Removing VPN access, disabling or deleting
the user revokes the certificate at once** - old profiles stop working.

## Your account

The user name at the top right of every page opens **Your account**, where everyone - whatever their role - can:

- see their user name and role (the role and description are managed by an administrator);
- **change their password** - with the current password; you stay signed in here and are signed out on every
  other device;
- set up **two-factor sign-in**.

![Your account](/images/manual/account.png)

## Two-factor sign-in

After the password, the sign-in asks for a 6-digit code from an authenticator app on the phone - Google or
Microsoft Authenticator, 1Password, Bitwarden and others. A stolen password alone is then not enough.

**Strongly recommended for every administrator**, especially when the panel is reachable from outside.

### Set it up

1. **Your account > Set up two-factor sign-in**, enter your password.
2. Scan the QR code with the authenticator app (or type the key).
3. Enter the 6-digit code the app shows and click **Switch on**.
4. **Save the 10 recovery codes** that appear - they are shown only once. Each code signs in one time when the
   phone is not at hand. Store them in a password manager or print them.
5. Click **I have saved them**.

Switching it on signs you out on every other device.

### Later

- **Your account** shows how many recovery codes are left. **New recovery codes** replaces them all (with your
  password and a current code).
- **Switch off** with your password and a current code.
- **Phone lost and no recovery code left:** an administrator opens the user on **System > Users** and clicks
  **Switch off two-factor sign-in**. The user signs in with the password and sets it up again.

> **Locked out as the only administrator?** On the console, menu option **3) Reset admin password** sets a new
> password for the panel.
