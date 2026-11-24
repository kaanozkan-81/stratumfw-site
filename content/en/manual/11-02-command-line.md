---
title: "Command Line"
seo_title: "11.2 Command Line — Stratum FW User Manual"
description: "The stratum-config command - show, validate, render, diff, apply with automatic rollback, confirm, rollback, status - and the other stratum-* tools, services and files for working in the shell."
weight: 1102
chapter_num: "11.2"
url: "/manual/command-line/"
aliases:
  - "/manual/11-02-command-line/"
translationKey: "manual-11-02-command-line"
---

Everything in Stratum FW can be done in the web panel. The command line is for troubleshooting, for automation,
and for the moments the panel cannot be reached. Open it with [console](/manual/console-menu/) option **8) Shell** or over SSH; the
commands need `root`.

> **Edit the configuration in the panel, not by hand.** The generated files (`/etc/pf.conf`, dnsmasq, Unbound,
> OpenVPN ...) are rewritten at every Apply - changes made in them directly are lost. The single source of truth is
> `/usr/local/etc/stratum/config.json`.

## stratum-config

`stratum-config` turns `config.json` into the running system. The panel's **Apply**, **Keep changes** and the
automatic rollback all use it.

| Command | Does |
| :--- | :--- |
| `stratum-config show` | Print `config.json`. |
| `stratum-config validate` | Check `config.json`; prints the errors, if any. |
| `stratum-config render pf` | Print the generated pf rule set (also `dnsmasq`, `mpd5` - with the PPPoE password masked - or `rc` for the rc.conf settings). Nothing is changed. |
| `stratum-config diff` | Compare what would be generated with what is running: a diff per file, rc.conf changes, interfaces and services that would restart, and the number of pending changes. Nothing is changed. |
| `stratum-config apply` | Apply `config.json` with **automatic rollback after 180 seconds** unless confirmed. `--timeout <s>` sets another time; `--no-rollback` applies without a timer. |
| `stratum-config confirm` | Keep the last apply (the panel's **Keep changes**). |
| `stratum-config rollback` | Restore the state from before the last apply, now. |
| `stratum-config status` | Whether an apply is awaiting confirmation, how long ago it was applied and whether the rollback timer runs. |

Only one apply, confirm or rollback runs at a time; a new apply is refused while the previous one awaits
confirmation.

**Typical use - see what a change will do before applying it:**

```
stratum-config diff | less
stratum-config render pf | less
```

**Applying from the shell** (for example over SSH while the panel is unreachable):

```
stratum-config apply
# check that everything works, then within 180 s:
stratum-config confirm
```

If the SSH session is cut by the change, do nothing: the previous state returns by itself after the timeout.

> `stratum-config import` builds `config.json` from a running system. It is used once, at installation; do not run
> it on a configured firewall.

The log of every apply is `/var/log/stratum-apply.log` ([**Diagnostics > System Logs > Configuration**](/manual/diagnostics-system-logs/)).

## Other tools

Each page that downloads lists or runs a service has a tool behind it. Their `status` commands print JSON.

| Tool | Commands |
| :--- | :--- |
| `stratum-update` | `check` - ask the update server; `run` - install (backup, boot environment, upgrade, verification); `status`; `prune` - remove old boot environments. |
| `stratum-cert` | `show`; `selfsigned <name\|ip> ...` - new self-signed certificate; `install <cert.pem> <key.pem>`; `restore` - back to the previous certificate. |
| `stratum-passwd [user]` | Set a panel user's password (default `admin`). |
| `stratum-ipbl` | `update [--force]`, `check <address>` - which IP blocklist contains an address, `status`. |
| `stratum-dnsbl` | `update [--force]`, `check <domain>` - which DNS blocklist blocks a domain, `status`. |
| `stratum-webfilter` | `update [--force]`, `check <domain>` - its categories and which policies block it, `status`. |
| `stratum-ids` | `update [--force]` - download and load the ET Open rules, `build`, `status`. |
| `stratum-openvpn` | `status` - connected clients; `profile <user>` - print a user's `.ovpn`; `revoke <user>`. |
| `stratum-backup` | `run` - automatic backup now; `test` - test the SFTP or SMB connection; `list` - the backups on the server. |
| `stratum-notify` | `test telegram` or `test email`. |
| `stratum-gwmon status` | The gateway measurements (latency, loss, state). |
| `stratum-menu` | The console menu; `--setup` runs the first-boot wizard again. |

Examples:

```
stratum-dnsbl check doubleclick.net
stratum-ipbl check 198.51.100.23
stratum-webfilter check example.com
stratum-update check
```

## Services

Stratum's services are ordinary FreeBSD services:

| Service | Is |
| :--- | :--- |
| `stratum` | The web panel |
| `nginx` | HTTPS in front of the panel and the captive portal page |
| `stratum_gwmon` | Gateway monitor |
| `stratum_fwlog` | Firewall log collector |
| `stratum_traffic` | Traffic graph collector |
| `stratum_webact` | Web Activity collector |
| `stratum_ipbl`, `stratum_ids` | IP blocklists, intrusion detection |
| `stratum_wireguard`, `stratum_wgs2s`, `stratum_ipsec` | WireGuard and IPsec |
| `stratum_appd` | Application Control collector (`stratum-dpi`) |
| `stratum_captive` | Captive portal |
| `stratum_netextra` | Virtual IPs and static routes |

```
service stratum status
service stratum restart
```

[**Services > Service Status**](/manual/services-service-status/) in the panel shows and restarts them as well. Prefer the panel or Apply: a service
started by hand with other settings is set back at the next Apply.

## Files

| Path | Contains |
| :--- | :--- |
| `/usr/local/etc/stratum/config.json` | The configuration (no secrets) |
| `/usr/local/etc/stratum/` | Root-only secret files (keys, passwords, tokens), the panel certificate |
| `/usr/local/etc/stratum/backups/` | Configuration history ([**System > Backup**](/manual/system-backup/)) |
| `/var/db/stratum/` | Downloaded lists, statistics, Web Activity, captive portal sessions |
| `/var/log/stratum.log` | Web panel |
| `/var/log/stratum-apply.log` | Every Apply |
| `/var/log/stratum-update.log` | Updates |
| `/var/log/messages`, `/var/log/auth.log` | System and sign-ins |
