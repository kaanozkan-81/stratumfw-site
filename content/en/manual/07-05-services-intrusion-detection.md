---
title: "Services > Intrusion Detection"
seo_title: "07.5 Services > Intrusion Detection — Stratum FW User Manual"
description: "Suricata with the ET Open rules: watching or blocking per interface, rule categories, updates, the alerts, disabling noisy rules, notifications, memory and fail-open behaviour."
weight: 705
chapter_num: "07.5"
url: "/manual/services-intrusion-detection/"
aliases:
  - "/manual/07-05-services-intrusion-detection/"
translationKey: "manual-07-05-services-intrusion-detection"
---

Intrusion detection compares the traffic with thousands of rules that describe attacks - malware calling home,
exploits, scans, phishing and more - and raises an alert when a rule matches. In **Block** mode it also drops what
the rules of chosen categories match (intrusion *prevention*).

Stratum FW uses **Suricata** with the **ET Open** rule set from Proofpoint (Emerging Threats), free and updated
daily.

The page has two tabs: **Alerts** and **Settings**.

## Settings

![Services > Intrusion Detection > Settings](/images/manual/ids-settings.png)

### Switch on and choose interfaces

1. Switch on **Watch the traffic of the selected interfaces and raise alerts**.
2. Set each interface under **Interfaces**:

| Mode | What happens |
| :--- | :--- |
| **Off** | The interface is not inspected. |
| **Watch** | Alerts only. Suricata reads a copy of the traffic; nothing is ever dropped. |
| **Block** | Suricata sits in the path of the packets: the rules of the categories marked **Block** drop what they match on this interface. The other categories still only alert. |

Interfaces are set one by one - for example the WAN only watched and the local networks blocking.

3. Choose the **Rule categories** (below) and set **Automatic rule updates**.
4. **Save** and **Apply changes**. Loading the rules takes about a minute on small processors.

> **Watching the WAN and the local networks** shows most alerts twice. The local side has the advantage that it
> shows *which device* was involved; the WAN side also sees attacks that the firewall rules drop anyway.

### Rule categories

There are 52 categories in three groups:

| Group | Contains |
| :--- | :--- |
| **Threats** (26) | Malware, mobile malware, exploits and exploit kits, current events, phishing, attack responses, coin miners, scans, denial of service, shellcode, worms, web server, web application and web client attacks, SQL injection, malicious DNS, suspicious user agents, botnet command-and-control, compromised and known-attacker address lists ... |
| **Protocols and services** (15) | Rules for particular protocols and services: FTP, SMTP, IMAP, POP3, SNMP, Telnet, TFTP, NetBIOS, RPC, VoIP, SCADA, ICMP, dynamic DNS, ActiveX and others. |
| **Policy and information** (11, noisy) | Rules that describe *what* is used rather than attacks - info, policy, hunting, chat, games, P2P, file sharing, inappropriate content, remote access, abused services, Tor. Useful for auditing, noisy for daily alerts. |

**Recommended selection** selects the 24 categories that are on by default - the threat categories. Each
category has a **Block** mark that decides whether it drops traffic on blocking interfaces.

**Categories take effect at once when saved, without Apply**: the rules are tested first and Suricata reloads
them without a gap.

> **Tip - avoid double work:** the categories *ciarmy*, *dshield*, *drop* and *compromised* are lists of bad
> addresses. If the IP blocklists ([**Firewall > Blocklists**](/manual/firewall-blocklists/)) are on with similar lists, these addresses are
> dropped before Suricata sees them, and the categories only add alerts and memory use. Consider switching them
> off.

### Starting to block

Start with **Watch** everywhere for a week or two, look at the alerts, and disable the rules that alert on your
normal traffic. Then switch the local networks to **Block** with a few threat categories whose alerts you already
trust - for example **malware**, **botnet command-and-control** and **exploit kits** - and add more over time.

### How blocking works

- Blocking interfaces go through **netmap**, as in OPNsense. A blocking VLAN or PPPoE interface is inspected on its
  physical port, so the VLANs of one port share its mode, and hardware offloading is switched off on it.
- Watch and Block together run **two Suricata engines** (one blocking, one reading copies), as pfSense does. The
  rules are loaded twice, so memory use is about twice as high.
- **Fail open:** if Suricata stops, traffic flows unfiltered instead of stopping. A restart interrupts the traffic
  of the blocking ports for a few seconds.
- Memory use is about 1-2 GB with the recommended categories. On systems with less than 8 GB the page warns
  that each engine gets smaller limits - keep the categories few there.

Switching on and the interface modes need **Apply changes**.

### Rule updates

| Setting | Meaning |
| :--- | :--- |
| **Automatic rule updates** | Every hour, 6 hours, 12 hours, daily, weekly, or off. Daily is a good choice: ET Open is published daily. |
| **On**, **At** | Day and time. |

**Update rules now** updates at once; the **Update log** shows the result. A failed update keeps the previous rules.

## Alerts

![Services > Intrusion Detection > Alerts](/images/manual/ids-alerts.png)

The **Alerts** tab lists the latest alerts: **Time**, **Severity**, **Rule** (the rule's message), **Source**,
**Destination** and **Interface**. Filter by severity (**All severities**, **High only**, **High and medium**) or
search; **Refresh** reloads.

Badges on an alert:

| Badge | Meaning |
| :--- | :--- |
| **Blocked** | The packet was dropped on a blocking interface. |
| **Port closed** | The packet went to a port that is not open on the firewall - the firewall rules dropped it anyway. Typical for internet-wide scans of the WAN; harmless, and not sent as a notification. |

### Disable a noisy rule

Some rules alert on normal traffic in your network - a backup program that looks like data theft, a device with
an unusual user agent. **Disable rule** on the alert switches that one rule off. The change takes effect at once,
and the list of disabled rules is kept across rule updates.

Disabled rules are listed on the **Settings** tab under **Disabled rules**, each with **Enable again**.

### Reading an alert

- **Source outside, destination inside, port closed:** a scan that the firewall already dropped. Nothing to do.
- **Source inside, destination outside:** a device in your network did something a rule describes - for example
  contacting a known command-and-control server. Look at that device first.
- **The same rule again and again on normal traffic:** a false positive - disable the rule.

The AI Assistant ([**Diagnostics > AI Assistant**](/manual/diagnostics-ai-assistant/)) can summarise and explain the alerts of a period.

## Notifications

High-severity alerts can be sent by Telegram or e-mail (**System > Settings > Notifications**, event *Intrusion
detection*). Alerts for ports that are closed on the firewall are left out by default; a separate event switches
them on.
