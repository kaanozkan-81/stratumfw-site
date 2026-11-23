---
title: "Firewall > Application Control"
seo_title: "06.8 Firewall > Application Control & Block Page — Stratum FW User Manual"
description: "Which applications the devices in your networks use and which they may not: identification per interface, Applications and Devices reports, categories and custom applications, Monitor-only and Block rules, customizable HTTPS Block Page with Block Page CA, Dashboard widget and AI Assistant."
weight: 608
chapter_num: "06.8"
url: "/manual/firewall-application-control/"
aliases:
  - "/manual/06-08-firewall-application-control/"
translationKey: "manual-06-08-firewall-application-control"
---

Application Control names the application behind every connection of the networks you choose - WhatsApp,
Instagram, YouTube, Netflix, Microsoft 365, BitTorrent, OpenVPN, WireGuard, AnyDesk and hundreds more - and sorts
them into categories: social media, messaging and calls, video and music, downloads and P2P, VPN and proxies,
remote access, games, AI assistants and others.

With **rules** it also blocks them - per interface, per category or application, with exceptions for devices - and
every rule can first run in **Monitor only**, which blocks nothing and records what it would have blocked.

The page has seven tabs: **Applications**, **Devices**, **Rules**, **Blocked**, **Block page**, **Categories** and **Settings**. Only
administrators can open it.

## How it works

- The engine reads a **copy** of the traffic of each selected interface. It is never in the path of the packets: if
  it stops, the traffic is not affected. Blocking is done by the firewall itself (pf), on the engine's word - see
  [Rules](#rules).
- It looks at the first packets of every connection: the protocol itself (OpenVPN, WireGuard, BitTorrent, SSH,
  game protocols ...), the server name of HTTPS and QUIC / HTTP/3 connections, and known addresses of the services.
- The engine runs as its own unprivileged user in a sandbox (FreeBSD Capsicum): it can read the traffic copy and
  write its results - nothing else.
- Only **daily totals per device and application** are kept: connections, data downloaded and uploaded, first and
  last time. No server names and no single connections are written to disk.
- Device names come from the DHCP and ARP history, as in [**Diagnostics > Web Activity**](/manual/diagnostics-web-activity/).

The engine uses nDPI (its LGPLv3 components only) and Stratum FW's own code for TLS and QUIC.

## Switch it on

![Firewall > Application Control > Settings](/images/manual/appctl-settings.png)

1. **Settings**: switch on **Identify the applications used in the selected networks**.
2. Tick the **Interfaces** to watch. Only internal interfaces with a static address are offered, VLANs included.
   Each interface gets its own engine.
3. **Keep the records (days)**: 1-730, default 30. Hourly totals for the dashboard are kept 14 days at most.
4. **Save**, **Apply changes** and **Keep changes**.

The **Status** table shows each interface's engine: whether it runs, packets per second, open connections, packets
dropped in the last 30 seconds and memory. The first applications appear in the reports within about a minute.

> **Which interfaces?** The internal ones, where the devices are: on a WAN every device hides behind the
> firewall's address. Watching a busy interface costs some memory - about 100-300 MB per interface, depending on the
> number of connections.

## Applications

![Firewall > Application Control > Applications](/images/manual/appctl-apps.png)

The **Categories** bar shows how the data of the period divides between categories. Click a category under the bar,
or choose one in **All categories**, to list only its applications.

The table lists every application with its **Category**, the number of **Devices** that used it, the data
**Download** and **Upload**, the number of **Connections** and when it was **Last seen** - most data first. Filter
by period (**Today**, **Last 7 days** ... **Last year**), by category, by network, or search for an application or
category. A **changed** badge marks an application you moved to another category.

Click an application to see its category and which devices used it, with their data and the first and last time.

Some connections can only be named by their protocol:

| Shown as | Means |
| :--- | :--- |
| **Other HTTPS** / **Other QUIC** | An encrypted connection to a site no application is known for, or a connection already running when the engine started. Add the site as a custom application to name it. |
| **Encrypted site (ECH)** | The browser hid the name of the site (Encrypted Client Hello). Nobody on the way can see it. |
| **chatgpt.com**, **claude.ai** ... | A site the engine knows only by its category, named after the site. |
| **Unknown** | A protocol the engine does not know, or a connection too short to tell. |

## Devices

The **Devices** tab lists every device with the number of applications it used, its three biggest applications,
and its data. Click a device for its applications. **Sites in Web Activity** on that page shows which sites it looked
up (when Web Activity is on).

## Categories

![Firewall > Application Control > Categories](/images/manual/appctl-categories.png)

The engine names each application with one of nDPI's categories. Stratum sorts those into its own categories - the
ones you see in the reports, on the dashboard and in the AI Assistant, and the ones the [Rules](#rules) use. The panel on the right lists every category with examples and the nDPI categories it holds.

Stratum also moves some applications to where you would look for them: the photos and videos of WhatsApp
(**WhatsAppFiles**) are **Messaging and calls**, not downloads; HTTP proxies, SOCKS and encrypted DNS (DoH / DoT) are
**VPN and proxies**, because they get around the firewall's filters; Zoom and Webex are calls.

### Move an application

**Applications** lists every application of the last 30 days with its **Category**, its **nDPI category** and its
data. Choose another category in its row; the default is marked *(default)*, and choosing it again undoes the
change. **Changed only** lists your changes. Save, then Apply. A change applies to the history too: the reports of
past days use the new category.

### Custom applications

Name sites the engine does not know - a company portal, an online tool, a local service. Click **Add** and give:

| Field | Meaning |
| :--- | :--- |
| **Name** | Shown in the reports, e.g. `DeepL`. |
| **Category** | Where it belongs. |
| **Domains** | One or more, separated by spaces. A domain covers its subdomains: `deepl.com` also matches `www.deepl.com`, not `notdeepl.com`. You can paste a full address; only the host name is kept. |

Custom applications are named from the server name of HTTPS and QUIC connections, so they work for encrypted sites.
They win over the engine's own name. They count from Apply on; connections before that keep their old name.
Applying them does not restart the engines.

## Rules

![Firewall > Application Control > Rules](/images/manual/appctl-rules.png)

A rule says which connections of which networks to block. The list is checked from the top: the **first matching
rule decides**. Click **Add rule**, or a rule's description to change it:

| Field | Meaning |
| :--- | :--- |
| **Rule is on** | Off keeps the rule in the list without effect. |
| **Description** | Shown in the lists and the reports, e.g. *No VPN for guests*. |
| **Interfaces** | The networks the rule covers. Only interfaces watched on **Settings** are offered. |
| **Categories** | Stratum's categories (see [Categories](#categories)): *VPN and proxies*, *Social media*, *Games* ... **Network services** (DNS, DHCP, NTP) cannot be chosen: the network needs them. |
| **Applications** | Names as on the Applications tab, separated by commas - also custom applications. |
| **Encrypted connections without a server name** | HTTPS / QUIC to port 443 that does not say which site it wants. Browsers always say it; VPN and tunnel apps often do not. Some old apps and devices do not either. |
| **Except these devices** | Addresses or networks the rule leaves alone, e.g. the IT laptops. |
| **Mode** | **Monitor only** blocks nothing and records what would be blocked. **Block** blocks (and can optionally **show a block page** — see [Block page](#block-page)). |

On the list, the arrow moves a rule up, the box switches it on or off; **Save order and on / off** keeps both.
**Today** shows how often each rule blocked or would have blocked. Rules take effect after **Apply**; the engines keep
running.

> **Start every rule in Monitor only.** Watch the **Blocked** tab for a few days - above all with *Encrypted
> connections without a server name* - and switch the rule to **Block** when it catches only what it should.

### What Block does

- Only names the engine is **sure** of are acted upon: the server name of the connection, or the content of its
  packets. A guess from a port or an address range never blocks anything.
- A connection to a private address, a connection already open when the engine started, and one the engine could
  name only as it ended, are left alone.
- **Applications named by their server name** (TikTok, Instagram, a custom application ...) are cut one connection
  at a time, as soon as the engine has named them - usually before the TLS handshake ends - and every new try again.
  Nothing else is blocked: one server address often serves many other sites (Google, Akamai, Cloudflare). For QUIC
  (HTTP/3), QUIC from that device to that server is refused for 10 minutes; the browser then uses HTTPS over TCP for
  every site on that address, and those connections are checked by name one by one.
- **Connections without a server name** (VPN tunnels, protocols found in the packets) also block that device from
  that server address for 10 minutes: there is no name to recognise the next try by.
- Unless **Show block page** is switched on for the rule (see [Block page](#block-page)), the device sees **connection reset** at once. Showing a page for HTTPS sites requires the firewall's [**Block Page CA**](/manual/system-certificates/#block-page-ca) to be trusted on the devices.
- When the collector stops or a rule is switched off, every block is lifted at once.

### Blocked

![Firewall > Application Control > Blocked](/images/manual/appctl-blocked.png)

Per device, application and rule: when it was last and first blocked, how many connections or block pages shown, and whether it was
**Page shown**, **Blocked** or **Would block** (a rule in Monitor only). Filter by mode, network and period, or search. The line above
the table shows how many block pages were shown, how many connections were cut since the collector started, and how many devices are blocked from
servers without a name right now.


### Block page

![Firewall > Application Control > Block page](/images/manual/appctl-blockpage.png)

A rule in **Block** mode can show a customizable page in the browser instead of a connection error - naming the **site**, the **application**, the **rule**, the **device** and the **time**:

- **How it is reached (`198.18.0.1` + Unbound DNS):** For the networks a rule covers (and never for its device exceptions), the firewall's [**Unbound DNS resolver**](/manual/services-dns-resolver/) answers the domain names of the blocked applications (~1 000 domains from nDPI's application lists plus your **Custom applications**) with `198.18.0.1`, and `pf` sends web traffic for that address to the isolated block page server (`_stratumbp`). QUIC to `198.18.0.1` is refused so browsers use HTTPS over TCP immediately, and Firefox is signalled not to switch to DNS over HTTPS while a rule shows the block page.
- **HTTPS & Block Page CA:** For `https://` sites, the block page generates a certificate for the requested host name on the spot, signed by your [**Block Page CA**](/manual/system-certificates/#block-page-ca) (**System > Certificates > Block Page CA**). Create the CA once, download it as `.cer` or `.crt`, and deploy it to your managed devices (for example with an Active Directory Group Policy). Devices without the CA see a browser certificate warning instead of the page - the site stays blocked either way.
- **Fallback when another DNS server is used:** Devices that bypass the firewall's DNS still have their matching connections cut by `stratum-dpi` and `pf` as above, without the page.
- **Customise on the Block page tab:** Upload your own **logo** and a wide **top picture / banner** (PNG, JPEG or WebP), set the **organisation name**, **title**, **message texts**, **contact line**, **colour**, and choose which details (**Site**, **Application**, **Rule**, **Device**, **Time**) and labels appear - in any language, with a **live preview**. Saved changes take effect at once, without **Apply**.
- **Privilege separation:** The block page web server runs as its own unprivileged user (`_stratumbp`), its listening ports cannot be reached directly from any network, and only `_stratumbp` and `root` can read the Block Page CA key.

## Dashboard and AI Assistant

- The [dashboard](/manual/dashboard/) panel **Applications** shows the last hour's top applications and categories by data. It shows no
  devices, so every role can add it.
- The [AI Assistant](/manual/diagnostics-ai-assistant/) and MCP clients can ask with the tool **Application usage**: *"Which applications did the guest
  network use most today?"*, *"What does 10.90.0.57 use?"*, *"What did the rules block this week?"*. It reads only -
  rules are changed in the panel. It can be switched off on [**System > Settings > MCP**](/manual/system-settings-snmp-mcp/).

## Privacy

Application Control shows what people in your networks do. Only administrators can open the reports, and every
look at the data is written to the system log. Inform your users as your local law requires.

## Troubleshooting

| Symptom | Check |
| :--- | :--- |
| *Apply to start* stays | Apply and keep the changes. |
| An interface shows **Restarting** or **Stopped** | The error under it, and [**Diagnostics > System Logs**](/manual/diagnostics-system-logs/) (program `stratum-appd`). After 5 stops within 10 minutes an interface is left off until the service restarts ([**Services > Service Status**](/manual/services-service-status/)). |
| **Dropped** packets | The engine cannot keep up with that interface: some connections are counted without a name or with less data. Traffic is not affected. |
| Much **Other HTTPS** or **Other QUIC** | Connections already running when the engine started stay unnamed until they end; this falls within the day. For sites you care about, add a custom application. |
| A custom application shows nothing | Apply after saving. Check the domain: the device must reach the site by that name or a subdomain of it. Connections that were already open keep their old name. |
| A rule in **Block** does not block a site | Was the connection already open? Close the browser or app and open it again: connections open before the rule (or before the collector restarted) are left alone. Check the rule's interfaces and exceptions, and that the application is named on the Applications tab - not as *Other HTTPS*. |
| A rule blocks too much | Switch it to **Monitor only** and Apply: every block is lifted at once. The **Blocked** tab shows what it caught. *Encrypted connections without a server name* is the usual cause. |
| **pf: N error(s)** in the status line | The collector could not load its blocks into pf; the message is in the tooltip and in the system log. |
| An interface with intrusion detection in **Block** mode shows nothing | Suricata takes that interface's packets through netmap; this combination is still being tested. Use Watch mode for intrusion detection on that interface meanwhile. |
