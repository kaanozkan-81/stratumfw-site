---
title: "Services > DNS Resolver"
seo_title: "07.3 Services > DNS Resolver — Stratum FW User Manual"
description: "The firewall's DNS service: local domain, upstream servers, dnsmasq or Unbound, forwarder or resolver mode, DNSSEC, forcing DNS through the firewall, host overrides and domain overrides."
weight: 703
chapter_num: "07.3"
url: "/manual/services-dns-resolver/"
aliases:
  - "/manual/07-03-services-dns-resolver/"
translationKey: "manual-07-03-services-dns-resolver"
---

The devices in your networks ask the firewall for names; the firewall answers from its own records or asks the
internet. **Services > DNS Resolver** controls how.

Getting DNS right matters more than it seems: the DNS blocklists, the Web Filter (including **Keywords**), the [**Application Control**](/manual/firewall-application-control/) block page and Web Activity all work through
the firewall's resolver, and a device that asks another server bypasses DNS-based filtering and block pages.

![Services > DNS Resolver](/images/manual/dns.png)

## Resolver settings

| Setting | Meaning |
| :--- | :--- |
| **Local domain** | The domain of your internal networks (default `lan`). DHCP clients and static mappings are reachable as `name.<domain>`, and queries for this domain never leave the firewall. The same setting as on **System > Settings > General**. |
| **Upstream DNS servers** | The servers the firewall asks, up to four, separated by commas. Default `1.1.1.1, 8.8.8.8`. With Unbound they must be IP addresses. |
| **Use only these upstream servers** | Ignore the DNS servers the firewall would otherwise use (for example ones received with a DHCP WAN address). On by default. |
| **DNS resolver** | **dnsmasq** - DNS and DHCP in one service; or **Unbound** - a validating resolver (dnsmasq keeps doing DHCP). |
| **Unbound mode** | **Forwarder** - ask the upstream servers above (needs at least one); **Resolver** - ask the internet's root servers directly, without any third-party DNS provider. |
| **Validate DNSSEC signatures** | (Unbound.) Refuse answers whose DNSSEC signature is wrong - protection against forged answers for signed domains. |

### dnsmasq or Unbound?

| | dnsmasq | Unbound |
| :--- | :--- | :--- |
| DNS for the networks, local names | yes | yes |
| DNS blocklists, Web Filter & Keywords, Application Control block page, Web Activity | no | **yes** |
| DNSSEC validation | no | yes |
| Resolving from the root servers (no DNS provider) | no | yes |
| Memory | very little | more, growing with blocklists and Web Filter categories |

Choose **Unbound** for any firewall that should filter. dnsmasq is the lightest choice for a firewall that only
routes. DHCP keeps running on dnsmasq either way, so switching does not touch the devices' addresses.

**Forwarder or Resolver?** As a forwarder, the firewall sends every query to the upstream servers - fast, and the
DNS provider sees all names. As a resolver, the firewall asks the authoritative servers itself - no DNS provider
in between; the first lookup of a name can take a little longer.

Click **Save settings**, **Apply changes** and **Keep changes**.

## Force DNS through the firewall

A device can ignore the DNS server it got from DHCP and ask `8.8.8.8` directly - by its own setting, by an app's
built-in DNS, or deliberately to get around the filters. **Force DNS through the firewall** closes that door for
the networks you select:

- DNS queries (port 53) of these networks reach the firewall's resolver, whatever server a device asks - the
  device does not notice.
- DNS over TLS (port 853) from these networks is blocked, so devices fall back to normal DNS.
- Nothing is recorded by this setting itself.

**Except DNS servers:** an alias of servers the devices may still ask directly - for example the domain
controllers of an Active Directory domain, which clients must reach as their DNS servers.

If **Send all DNS to the firewall** is on under [**Diagnostics > Web Activity**](/manual/diagnostics-web-activity/), the page says so: it then applies
to every internal network.

DNS over HTTPS cannot be redirected, because it looks like any other HTTPS traffic. Block the public DoH servers
with the **DoH** category of the DNS blocklists ([**Firewall > Blocklists**](/manual/firewall-blocklists/)) or the *DNS over HTTPS servers*
category of the Web Filter.

The redirects appear as grey *Force DNS* entries on [**Firewall > NAT > Port Forward**](/manual/firewall-nat/).

## Host overrides

A host override answers exactly one name with an address you choose.

Typical uses:

- an internal server without a public DNS record - `nas.office.local`;
- a public name that should lead to the internal address for local devices - `cloud.example.com` to
  `192.168.1.20` instead of the WAN address. This is cleaner than NAT reflection ([**Firewall > NAT**](/manual/firewall-nat/)).

**Add host override**:

| Field | Meaning |
| :--- | :--- |
| **Host** | The first part of the name, for example `nas` (letters, digits and hyphens). It may be empty: then the override answers the domain name itself. |
| **Domain** | The rest of the name, for example `office.local`. |
| **IP address** | The address to answer with. |
| **Full name** | Shown for information: `nas.office.local`. |
| **Description** | Your note. |

Only the exact name is affected; other names in the same domain are resolved normally. Each name can have one
override.

## Domain overrides

A domain override sends every query for a domain - and all its subdomains - to a DNS server you choose.

Typical uses:

- an **Active Directory** domain: `corp.example.local` to the domain controller, so domain-joined computers find
  their services;
- **another site's names over a VPN**: `branch.lan` to the DNS server of the branch office.

**Add domain override**:

| Field | Meaning |
| :--- | :--- |
| **Domain** | All names in this domain, including subdomains. |
| **DNS server** | The IPv4 address of the server that answers for this domain. |
| **Description** | Your note. |

Changes to overrides become active after **Apply changes**. Devices may cache old answers for a few minutes.

## Checking DNS

- **Diagnostics > Ping / Traceroute / DNS** looks a name up through the firewall's resolver.
- [**Firewall > Blocklists**](/manual/firewall-blocklists/) and [**Firewall > Web Filter**](/manual/firewall-web-filter/) each have a check that tells you whether a name is
  blocked, and why.
- On a device: `nslookup <name>` should show the firewall as the server that answered.
