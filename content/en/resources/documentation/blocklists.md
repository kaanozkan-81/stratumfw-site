---
title: "IP &amp; DNS Blocklists"
lead: "Protect your network at both layers — block malicious IPs at the packet level and malicious domains at the DNS level."
---

Stratum Firewall provides a two-layer defense mechanism against modern cyber threats. You can block malicious IP addresses at the firewall level, and simultaneously block malicious domains (ads, trackers, phishing) at the DNS resolver level.

## Layer 1: IP Blocklists (IPv4/IPv6)
![IP Blocklists](/images/docs/blocklists.png)

IP Blocklists allow you to ingest public threat feeds (or custom lists) and block them directly using `pf` tables. This happens at the kernel level, ensuring zero performance impact even with millions of IPs.

### How to Enable

1. Navigate to **Security → Blocklists**.
2. Select the **IP Blocklists** tab.
3. Toggle **Enable IP Blocklists**.

### Adding a Feed

Stratum supports standard CIDR lists (e.g., Spamhaus DROP, DShield, Abuse.ch).

1. Click **Add Feed**.
2. Paste the URL of the raw text file (e.g., `https://www.spamhaus.org/drop/drop.txt`).
3. Set the update interval (e.g., every 12 hours).
4. Click **Save**.

Stratum will immediately download the list, parse the valid IPs, and inject them into a `pf` table named `<blocklist_feed_name>`. A default rule is automatically applied to block inbound and outbound traffic matching those IPs.

### Custom IP Blocking

You can also manually block an IP or a subnet without a feed:

1. In the IP Blocklists tab, scroll to **Custom IPs**.
2. Add the IP (e.g., `1.2.3.4`) or CIDR (e.g., `192.168.50.0/24`).
3. Click **Save**.

## Layer 2: DNSBL (DNS Blocking)

DNSBL prevents devices on your network from resolving malicious or unwanted domains. We use Unbound's RPZ (Response Policy Zones) feature to return `NXDOMAIN` for blocked domains.

### How to Enable

1. Navigate to **Security → Blocklists**.
2. Select the **DNSBL** tab.
3. Toggle **Enable DNSBL**.

### Selecting Categories

Instead of manually managing domains, Stratum provides curated feeds.
Select the categories you want to block (e.g., Ads, Malware, Phishing, Adult Content). The lists will be automatically downloaded and compiled into a local zone file.

### Whitelisting Domains

If a blocked feed inadvertently blocks a domain you need (a false positive), you can override it:
1. Scroll to the **Whitelist** section.
2. Add the domain (e.g., `example.com`).
3. Click **Save**.

## Monitoring and Logs

When a domain is blocked by DNSBL, Unbound logs it. You can view these logs under **Diagnostics → System Logs** (filter for `dnsbl`). For IP Blocklists, you can view blocked packets in **Diagnostics → Firewall Log**, where blocked packets will have the label `ipblock-in-*` or `ipblock-out-*`.

## Best Practices

- **Start with DNSBL:** It is the safest way to start blocking threats without the risk of breaking legitimate IP traffic.
- **Be careful with IP Blocklists:** Blocking large IP ranges can sometimes break access to CDNs (Content Delivery Networks). Always verify feeds before enabling outbound blocking.
- **Update Frequency:** Balance the need for fresh threat data with the bandwidth of your firewall. Daily updates are usually sufficient for small businesses.

## Next Steps

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>Explore the API</h3>
    <p>Automate blocklist management using the REST API.</p>
    <a href="/resources/documentation/rest-api/" class="card-link">API Reference →</a>
  </div>
  <div class="card">
    <h3>System Logs</h3>
    <p>Monitor blocked traffic and diagnose network issues.</p>
    <a href="/resources/documentation/system-logs/" class="card-link">Logs Guide →</a>
  </div>
</div>
