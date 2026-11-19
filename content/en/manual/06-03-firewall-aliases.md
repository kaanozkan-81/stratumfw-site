---
title: "Firewall > Aliases"
seo_title: "06.3 Firewall > Aliases — Stratum FW User Manual"
description: "Named groups of hosts, networks, ports, host names and countries for firewall rules and NAT - the five alias types, updates, renaming and deleting."
weight: 603
chapter_num: "06.3"
url: "/manual/firewall-aliases/"
aliases:
  - "/manual/06-03-firewall-aliases/"
translationKey: "manual-06-03-firewall-aliases"
---

An alias gives a name to a group of addresses or ports. Rules and NAT entries use the name instead of the
addresses, so:

- rules stay short and readable - *source `Office_IPs`* instead of a list of addresses;
- one change to the alias updates every rule that uses it;
- the firewall can keep some aliases up to date on its own - host names are resolved again and again, country
  lists are downloaded every week.

![Firewall > Aliases](/images/manual/fw-aliases.png)

## Alias types

| Type | Entries | Example | Used in |
| :--- | :--- | :--- | :--- |
| **Hosts** | Individual IPv4 addresses | `192.168.1.10`, `192.168.1.11` | Source and destination of rules, port forwards, outbound NAT |
| **Networks** | Networks in CIDR notation (host bits zero) | `10.0.0.0/8`, `192.168.50.0/24` | As Hosts |
| **Ports** | Ports and ranges | `443`, `8000:8010` | Destination port of rules, external port of port forwards |
| **Host names** | Host names (and, if useful, addresses) | `update.example.com`, `203.0.113.10` | As Hosts |
| **Countries** | Countries | Türkiye, Germany | As Hosts |

The firewall keeps **Host names** and **Countries** up to date by itself:

- **Host names** are resolved by the firewall every 5 minutes. An address stays in the alias for an hour after it
  was last seen, so services whose addresses rotate (cloud services, CDNs) keep working. Up to 200 names per
  alias.
- **Countries** contain all address blocks registered to those countries, from the five regional internet
  registries (ARIN, RIPE NCC, APNIC, LACNIC, AFRINIC), updated every week. No account or licence key is needed.
  Up to 100 countries per alias.

**Update now** on the list refreshes the host names and countries at once - for example right after you created
such an alias.

## The list

The tabs above the list filter it by type (**All**, **Hosts**, **Networks**, **Ports**, **Host names**,
**Countries**). Each alias shows its **Name**, **Type**, **Entries** (for host names and countries also how many
addresses they currently hold), **Description** and **Used by** - the rules and NAT entries that use it.

## Add an alias

Click **Add alias**.

| Field | Meaning |
| :--- | :--- |
| **Name** | Letters, digits and `_`, starting with a letter, up to 31 characters - for example `Office_IPs`, `Web_Ports`. It cannot be the name of an interface (`lan`, `opt2` ...) or a reserved word (`any`, `self`, `bogons`, `private`). Rules refer to the alias by this name. |
| **Type** | One of the five types above. |
| **Entries** | One entry per line. For **Countries**, choose the countries from the list instead. |
| **Description** | Your note. |

Click **Save**, then **Apply changes** on the list page.

## Typical aliases

| Alias | Type | Use |
| :--- | :--- | :--- |
| `RFC1918` | Networks: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` | *Reject guests to RFC1918* - keeps a network away from all internal networks, including ones added later. |
| `Office_IPs` | Hosts or networks: the public addresses of your offices and partners | The allowed source of port forwards and WAN rules for remote desktop, SSH, cameras. |
| `Web_Ports` | Ports: `80`, `443` | One rule for web traffic instead of two. |
| `Turkiye` | Countries: Türkiye | *Only Türkiye may reach the booking server's port forward*: the alias as the forward's allowed source. |
| `Backup_Server` | Host names: `backup.example.com` | A rule that follows the server when its address changes. |

> **Tip:** Country aliases are a coarse filter - an attacker can use a server in any country, and some legitimate
> users travel. Use them to cut down noise and exposure, not as the only protection of a service.

## Change, rename or delete

- **Rename:** changing the name updates every rule and NAT entry that uses the alias.
- **Type:** cannot be changed while the alias is in use - *The type of an alias that is in use cannot be changed.*
- **Delete:** an alias in use cannot be deleted; the panel lists where it is used, for example *Alias Office_IPs is
  in use: WAN rule #2, Port forward #1*. The edit page shows the same list under **Used by**.

