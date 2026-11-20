---
title: "Migrating from OPNsense"
seo_title: "12 Migrating from OPNsense — Stratum FW User Manual"
description: "Importing an OPNsense config.xml: what is carried over and what is not, mapping the network ports, the preview and report, taking over OpenVPN with the users' existing profiles, and a step-by-step plan for replacing the running firewall."
weight: 1201
chapter_num: "12"
url: "/manual/migrating-from-opnsense/"
aliases:
  - "/manual/12-01-migrating-from-opnsense/"
  - "/manual/opnsense-migration/"
translationKey: "manual-12-01-migrating-from-opnsense"
---

**System > Backup > Import from OPNsense** converts an OPNsense configuration into a Stratum FW configuration: the
interfaces, rules, NAT, aliases, DHCP and DNS of the old firewall, ready on the new one. OpenVPN can be taken over
completely, so the **profiles on the users' computers connect unchanged**.

![Import from OPNsense](/images/manual/opnimport.png)

## What is imported

| Imported | Notes |
| :--- | :--- |
| Host name, domain, time zone, DNS and time servers | |
| Interfaces and VLANs | Mapped to this system's ports (below). Static, DHCP and **PPPoE** (with user name and password). |
| Gateways | Priorities (the failover order), monitor addresses, the default gateway. |
| Virtual IPs, static routes | |
| Interface groups | |
| Aliases | Hosts, networks, ports; **host names** become FQDN aliases; **GeoIP** becomes country aliases. Downloaded lists (URL tables) that match a known blocklist switch on that **IP blocklist**; others are imported empty. |
| Firewall rules | The newer *Firewall > Rules* model and the older rules; rules with a gateway become policy routing. |
| Port forwards, outbound NAT | See *Port forwards* below. A DNS redirect to the firewall becomes **Force DNS**. |
| DHCP | Ranges and static mappings (dnsmasq and ISC DHCP). |
| DNS | Host and domain overrides; Unbound with its DNS blocklists. |
| Bandwidth limits | Shaper pipes per interface. |
| Bogon and private-network blocking, NAT reflection | |
| **OpenVPN** (optional) | The certificate authority, server certificate, TLS key, the users' certificates, and the VPN users with their passwords. |

**Not imported** - set them up again: panel users (VPN users come with OpenVPN), **IPsec**, **WireGuard**, captive
portal, link aggregation (LAGG), bridges and plugins.

**This firewall's own settings stay**: panel users, management access, notifications, backups, intrusion
detection, web filter and the other services.

The file is only read; it is not stored, and nothing secret appears in the report.

## Before you start

- Install Stratum FW on the new hardware and finish the first-boot wizard ([**Installation**](/manual/installation/), [**Initial Configuration**](/manual/initial-configuration/)). Connect it to your computer only - not to the production network yet.
- On OPNsense: **System > Configuration > Backups > Download configuration** - **not encrypted**. Keep this file
  safe: it contains the configuration's secrets.
- Note which OPNsense port (igb0, igb1 ...) carries what.

## Import step by step

### 1. Read the file

**Choose file** - the `config.xml` - and **Read the file**.

### 2. Map the network ports

**Network ports** lists each OPNsense port, what **uses** it (interfaces, VLANs), and a **Port here** to assign.
On identical hardware the names usually match; on other hardware choose the port that will get that cable.

A port left unassigned is not imported - with its interfaces, VLANs and rules.

**Update the preview** after changing the mapping.

### 3. Check the preview

The preview shows:

- **Interfaces** - each with its port, type and address;
- **Changed while importing** - everything converted in another way than in OPNsense, with the reason: renamed
  aliases, rules switched off, port forwards given a source, schedules not carried over ...;
- **Not imported** - what was left out, and why;
- **warnings** at the top, for example *The LAN address changes to ...* (connect from the new LAN network after
  Apply) or *This firewall's WAN has the address ... inside the imported network* (connect the WAN to its real
  line first, or Apply would cut it off).

Read **Changed while importing** carefully - it is the list of what to look at after the import.

### 4. OpenVPN

Tick **Also import OpenVPN** to take over the server: the same CA, server certificate and TLS key, the users'
certificates and their passwords. The users' existing profiles then connect to Stratum FW unchanged - when it has
the old firewall's address.

- The certificates are written at once (this firewall's earlier ones are kept in a separate folder); the settings
  wait for Apply.
- VPN users that do not exist yet are created as **VPN-only** accounts with their OPNsense passwords. Users that
  already exist here keep the password set here.
- Revoked certificates are not carried over: if OPNsense used a revocation list, revoke them again on [**VPN > OpenVPN**](/manual/vpn-openvpn/). Passwords in a format that cannot be taken over are listed - set them on [**System > Users**](/manual/system-users/).

### 5. Import and apply

Tick **Replace the interfaces, rules, NAT, aliases, DHCP and DNS settings with the imported ones** and click
**Import**.

The result becomes the **pending configuration**: nothing changes until **Apply**, and Apply can still be reverted.
The configuration from before the import stays in the history ([**System > Backup**](/manual/system-backup/)).

## How rules are converted

**Port forwards.** In OPNsense a port forward is often let in by a *separate* WAN rule. In Stratum FW a port forward
passes its own traffic, from its own source. So the source of each forward is taken from the WAN rules that let
that traffic in. A forward that no rule allowed is imported **switched off**.

**Aliases that cannot be carried over** become empty aliases: block rules that use them block nothing, pass rules
allow nothing - the rules keep a safe meaning.

**Rules that cannot be expressed** - IPv6, *out* direction, source ports, several interfaces in one rule, unknown
protocols - are left out and listed. Rules of the OpenVPN and WireGuard interfaces are left out too: access for VPN
clients is set on [**VPN > OpenVPN**](/manual/vpn-openvpn/) (the networks they may reach).

**Rules with a schedule** are imported switched off: add the schedule on [**Firewall > Schedules**](/manual/firewall-schedules/), choose it on the
rule and switch the rule on.

## Replacing the running firewall

A plan that keeps the interruption to a few minutes and the way back open:

1. **Prepare** the new firewall offline: import, Apply, **Keep changes**. Check **Interfaces**, **Firewall >
   Rules**, **NAT**, **Services > DHCP Server** and **VPN > OpenVPN** against the old firewall and the *Changed while
   importing* list.
2. Add what was not imported: IPsec and WireGuard tunnels, the captive portal, other panel users.
3. **Download a backup** of the new firewall.
4. At a quiet time, **move the cables** from the old firewall to the new one, port by port as mapped.
5. If the provider's modem or router binds to the old firewall's MAC address, restart it, or wait until it learns
   the new one.
6. Test: internet from each network, DHCP, DNS, the port forwards from outside, a VPN connection with an existing
   profile, the dashboard's gateways.
7. **The way back:** keep the old firewall as it is for a few days. Moving the cables back restores the old
   state at once.

After the switch, watch [**Diagnostics > Firewall Log**](/manual/diagnostics-firewall-log/) for blocks of traffic that should pass - a rule that did not
come over as intended shows up there first.
