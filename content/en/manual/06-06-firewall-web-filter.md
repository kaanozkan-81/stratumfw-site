---
title: "Firewall > Web Filter"
seo_title: "06.6 Firewall > Web Filter — Stratum FW User Manual"
description: "Blocking site categories per network with policies: the 56 categories, Safe Search, exceptions, list updates, the Blocked view and making the filter hard to bypass."
weight: 606
chapter_num: "06.6"
url: "/manual/firewall-web-filter/"
aliases:
  - "/manual/06-06-firewall-web-filter/"
translationKey: "manual-06-06-firewall-web-filter"
---

The Web Filter blocks whole categories of web sites - adult content, gambling, malware, social networks ... -
for the networks you choose. Each network gets a **policy**: one policy for the guest Wi-Fi, another for staff,
none for servers.

It works through the firewall's DNS resolver: for a site in a blocked category, the resolver answers *does not
exist*, so the site does not open - on every device, with nothing to install, also for HTTPS sites (nothing is
decrypted).

> **Note:** The Web Filter works inside **Unbound**. Choose **Unbound** as the DNS resolver under
> **Services > DNS Resolver** first.

![Firewall > Web Filter](/images/manual/wf-page.png)

## Switch it on

1. Switch on **Block site categories with the policies below**.
2. Add at least one policy (below).
3. Set **Category list updates** and click **Save**.
4. **Apply changes**. The categories used by the policies are downloaded and loaded.

Switching the filter on or off, and changes to policies, need **Apply changes**; exceptions take effect at once.

## Policies

**Add policy** opens the policy page.

| Field | Meaning |
| :--- | :--- |
| **Name** | For example *Guests* or *Staff*. |
| **Enabled** | A disabled policy keeps its settings and blocks nothing. |
| **Networks** | The internal networks - and the OpenVPN and WireGuard client networks - the policy applies to. A network can be in one policy only. |
| **Safe Search** | Google, Bing and DuckDuckGo without explicit results, and YouTube in restricted mode - enforced by the firewall, not a setting users can switch off. |
| **Blocked categories** | The categories to block for these networks. |

New categories are downloaded when you apply. Large categories need memory: the page shows an estimate.

![A Web Filter policy](/images/manual/wf-policy.png)

### The categories

| Group | Categories |
| :--- | :--- |
| **Adult** | Adult (full list), Adult (compact list), Mixed adult content, Lingerie, Dating, Sexual education, Gambling, Drugs, Violence and hate, Dangerous material, Sects |
| **Security** | Malware, Phishing, Cryptojacking, Stalkerware, DDoS services, Hacking, Dialers, Fake news, Marketingware, Residential proxies |
| **Getting around the filter** | VPN services, DNS over HTTPS servers, Proxies and anonymizers, URL shorteners, Remote control, Dynamic DNS |
| **Social and communication** | Social networks, Chat, Forums, Blogs, Webmail |
| **Entertainment** | Games, Audio and video, Radio, Sports, Manga, Celebrities, News, Astrology |
| **Shopping and money** | Shopping, Banks, Finance, Cryptocurrency, Job search, Advertising |
| **Downloads and hosting** | Downloads, File hosting, Warez, Web hosting |
| **Other** | Artificial intelligence, Translation, Cheating, Mobile phones, Cooking, Educational games |

**Choosing categories.** Start with what everyone expects to be blocked on a guest network: **Adult**,
**Gambling**, **Malware** and **Phishing**. Add more over time; the **Blocked** view on the page shows what the
filter stops.

**Adult (full list)** has millions of names and needs a lot of memory; **Adult (compact list)** covers the
well-known sites with a fraction of it - the right choice for most firewalls.

**Getting around the filter** - VPN services, DNS-over-HTTPS servers and web proxies - is what a determined user
tries first. Block this group wherever the filter must not be bypassed.

The categories come from the UT1 blacklists of Université Toulouse Capitole (CC BY-SA 4.0), updated daily, and
from the Block List Project.

## Make the filter hard to bypass

Because the filter works through DNS, a device that asks another DNS server is not filtered. Close those doors:

1. **Force DNS to the firewall** for the filtered networks (**Services > DNS Resolver**, or *Send all DNS to the
   firewall* on **Diagnostics > Web Activity**): DNS queries to any other server are redirected to the firewall.
2. **Block the "Getting around the filter" categories** - VPN services, DNS over HTTPS servers, proxies.
3. Optionally add the **DoH** category of the DNS blocklists ([**Firewall > Blocklists**](/manual/firewall-blocklists/)), so browsers cannot use
   encrypted DNS.

No DNS-based filter stops a user who connects through a VPN the lists do not yet know - but together these steps
stop almost everyone.

## Exceptions

**Exceptions**: one domain per line; its subdomains are allowed too. An exception is never blocked by the Web
Filter (the DNS blocklists still apply). Use it for the sites that must always work - for example your own
company's site that a category happens to contain. **Save exceptions** - they take effect at once.

## Check a site

**Check a site** tells you which categories contain a domain, which policies block it, and whether an exception allows it - the quickest
answer to *"why does this site not open on the guest Wi-Fi?"*.

## Updates

| Setting | Meaning |
| :--- | :--- |
| **Category list updates** | Every 6 hours, every 12 hours, daily, weekly, or **Off (manual updates only)**. |
| **On**, **At** | The day (weekly) and time. |

**Update lists now** updates at once. A failed download keeps the previous list.

## The Blocked view

The **Blocked** card shows what the Web Filter stopped **today**, in the **last 7 days** or the **last 30 days**,
per category, with the most blocked sites. When **Web Activity** is on (**Diagnostics > Web Activity**), blocks are also counted
per device.

