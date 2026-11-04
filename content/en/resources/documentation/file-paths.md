---
title: "File &amp; Path Reference"
lead: "Where configuration lives, where logs are stored, and what each file does."
---

One of the core design principles of Stratum Gateway is **transparency**. We do not hide configurations in binary databases. Everything is stored in plain text files that you can inspect, back up, or version control.

## Core Directories

| Path | Description |
|---|---|
| `/usr/local/etc/stratum/` | The main configuration directory. |
| `/usr/local/etc/stratum/config.json` | The single source of truth for all firewall settings. |
| `/usr/local/etc/stratum/wireguard/` | Stores WireGuard keys and generated `wg0.conf` files. |
| `/usr/local/etc/stratum/blocklists/` | Stores downloaded IP feeds for `pf` tables. |
| `/opt/stratum/` | The installation directory for the panel and Python virtual environment. |

## Generated Configuration Files

When you click **Apply** in the panel, the Python engine renders your `config.json` into native FreeBSD files. **Do not edit these files directly**, as they will be overwritten.

| Path | Rendered By | Description |
|---|---|---|
| `/etc/pf.conf` | `generators.py` | The main firewall rules, NAT, and routing configuration. |
| `/usr/local/etc/wireguard/wg0.conf` | `generators.py` | The WireGuard server interface configuration. |
| `/usr/local/etc/unbound/unbound.conf` | `generators.py` | The DNS resolver configuration, including DNSBL zones. |
| `/etc/rc.conf` | `generators.py` | FreeBSD system startup configuration (e.g., enabling `pf`). |

## Log Files

Logs are essential for troubleshooting. By default, Stratum uses FreeBSD's native logging mechanisms.

- **Panel & Auth Logs:**
  ~~~bash
  cat /var/log/auth.log
  ~~~
  *(Contains successful and failed login attempts, as well as API actions).*

- **Firewall Logs:**
  Blocked packets are logged to `pflog0`. You can read them via `tcpdump`:
  ~~~bash
  tcpdump -n -e -ttt -i pflog0
  ~~~

- **System Messages:**
  ~~~bash
  cat /var/log/messages
  ~~~

## Backups

You do not need to back up the entire filesystem. Backing up the `/usr/local/etc/stratum/` directory is sufficient to preserve your entire firewall state, as it contains your `config.json` and private keys.

## Next Steps

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>REST API</h3>
    <p>Automate configuration management via the API.</p>
    <a href="/resources/documentation/rest-api/" class="card-link">API Reference →</a>
  </div>
  <div class="card">
    <h3>Back to Installation</h3>
    <p>Review the installation steps.</p>
    <a href="/resources/documentation/building-from-source/" class="card-link">Read Guide →</a>
  </div>
</div>
