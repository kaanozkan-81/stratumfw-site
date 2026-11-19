---
title: "System > Backup"
seo_title: "09.7 System > Backup — Stratum FW User Manual"
description: "Downloading a backup with or without users and secrets, encrypting it, restoring, the configuration history with diffs, importing from OPNsense and automatic encrypted backups to an SFTP server."
weight: 907
chapter_num: "09.7"
url: "/manual/system-backup/"
aliases:
  - "/manual/09-07-system-backup/"
translationKey: "manual-09-07-system-backup"
---

**System > Backup** keeps your configuration safe in three ways: a **backup file** you download, the
**configuration history** on the firewall, and **automatic backups to a server**.

![System > Backup](/images/manual/backup.png)

## Download a backup

A single file with the complete configuration.

| Option | Meaning |
| :--- | :--- |
| **Include panel users** | Names, roles and password hashes. |
| **Encrypt with a password (AES-256)** | The file can only be opened with the password: at least 12 characters. |
| **Include certificates and secrets** | (Encrypted backups only.) The OpenVPN CA and user certificates, WireGuard keys, notification, SNMP and MCP secrets and the panel certificate. With them, a new firewall takes over **without re-issuing VPN profiles**. |

**Download**. An unencrypted backup is a `.json` file; an encrypted one is a `.stbk` file.

> **Keep it somewhere safe.** Even without the secrets option, a backup contains the configuration's secrets,
> such as the PPPoE password. Without its password an encrypted file cannot be restored - not even by Stratum -
> so keep the password apart from the file.

**When to download one:** before larger changes, before an update (the update makes its own as well), and
regularly - or set up the automatic backup below.

## Restore from a backup

1. **Choose file** - a Stratum FW backup (`.json`, or `.stbk` when encrypted), or a plain `config.json`.
2. For an encrypted file, enter its password.
3. Choose **Also restore panel users** (takes effect immediately) and **Also restore certificates and secrets**
   (replaces the current ones).
4. **Restore**.

The configuration is restored as **pending changes**: nothing changes on the firewall until you **Apply** - and
Apply can still be reverted. Look over the pending changes first.

**Restoring onto new hardware:** the interfaces are assigned by port name (`igb0`, `em1` ...). If ports of the
backup do not exist on the new machine, the restore says which, and you reassign them on [**Interfaces > Assignments**](/manual/interfaces-assignments/) before Apply.

A backup without certificates and secrets cannot restore them, and a users restore is refused when it contains no
enabled administrator with panel access.

**Import from OPNsense** converts an OPNsense `config.xml` - see the chapter **Migrating from OPNsense**.

## Configuration history

A copy is kept every time the configuration is saved - the newest 30. For each: when it was **Saved**, the
**File**, **Size**, and the actions:

- **Diff** shows what differs from the current configuration, line by line;
- **Download** saves it as a file;
- **Restore** loads it as pending changes - Apply as usual.

The history is the quickest way back when a change turns out wrong days later.

## Automatic backup to a server

**Automatic backup to a server > Configure** uploads an encrypted backup on a schedule over **SFTP** - to a NAS
(Synology, QNAP, TrueNAS) or a Linux or Windows OpenSSH server.

![Automatic backup](/images/manual/backup-auto.png)

### 1. The firewall's SSH key

The firewall signs in to the server with its own SSH key - no password is stored.

1. **Create SSH key**.
2. **Copy** the line shown and add it to `~/.ssh/authorized_keys` of the user on the server.

At the first connection the server's host key is recorded; from then on it must match - protection against a
server that pretends to be yours. If the server is reinstalled, **Forget the host key**.

### 2. Settings

| Field | Meaning |
| :--- | :--- |
| **Upload an encrypted backup to the server on a schedule** | Switches it on. |
| **Server (SFTP)**, **Port** | Address or name, port (default 22). |
| **User name** | The user on the server. |
| **Folder** | Relative to the user's home folder, or absolute. It must exist. |
| **Schedule**, **On**, **At** | Every hour, 6 or 12 hours, daily, weekly, or **Off (manual only)**; day and time. |
| **Keep on the server** | The newest backups to keep (1-365, default 30); older ones are removed. |
| **Include panel users**, **Include certificates and secrets** | As for a download; both on by default. |
| **Encryption password** | At least 12 characters. **Keep it somewhere else as well.** |

**Test connection** signs in and checks the folder. **Back up now** uploads one at once. **Save** takes effect at
once (no Apply needed).

### How it works

The server only ever sees encrypted files (AES-256-GCM) named `stratum-<host>-<date>.stbk`. Restore one on
**System > Backup** with the same password - on this or on a new firewall.

**Last backup** shows the time and result of the last run. A failed backup is retried at the next hour and
reported through [**System > Settings > Notifications**](/manual/system-settings-logging-notifications/).
