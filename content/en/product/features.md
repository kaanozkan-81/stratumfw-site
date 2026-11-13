---
title: "Features"
description: "Full feature matrix of Stratum Firewall v0.30.0: stateful pf rules, Multi-WAN policy routing, 63 IP blocklists, Unbound RPZ DNSBL, 55-category Web Filter, Suricata 8 IPS/IDS, WireGuard, OpenVPN, and ZFS rollback."
lead: "Stratum Firewall combines the battle-tested pf packet filter with a lightning-fast FastAPI management panel. Zero proprietary dependencies, absolute control, and a security-first architecture built for modern infrastructure."
full_width: true
seo_title: "Firewall Features — pf, Suricata 8 IPS, 55-Category Web Filter & Multi-WAN VPN"
seoTitle: "Firewall Features: pf, Suricata 8 DPI & WireGuard | Stratum FW"
seoDescription: "Explore Stratum Firewall features on FreeBSD 15.1: stateful pf rules with separators, Suricata 8 IPS/IDS, Unbound DNSBL, Force DNS for AD, TOTP 2FA & Multi-WAN."
---

<!-- STRATUM-DPI-SHOWCASE-START -->
<style>
.stratum-dpi-showcase{position:relative;margin:2.5rem 0;border-radius:20px;overflow:hidden;background:radial-gradient(120% 90% at 50% 0%,#101f3c 0%,#08101f 52%,#060b16 100%) !important;border:1px solid rgba(245,158,11,0.24);box-shadow:0 24px 60px rgba(2,6,23,0.55),inset 0 1px 0 rgba(255,255,255,0.06);}
.stratum-dpi-top{position:relative;z-index:3;padding:1.85rem 2.25rem 0.5rem;text-align:center;}
.stratum-dpi-eyebrow{display:inline-flex;align-items:center;gap:0.5rem;padding:0.32rem 0.85rem;border-radius:999px;background:rgba(245,158,11,0.12);border:1px solid rgba(245,158,11,0.32);color:#fbbf24 !important;font-size:0.74rem;font-weight:700;letter-spacing:0.09em;text-transform:uppercase;margin-bottom:0.8rem;}
.stratum-dpi-dot{width:7px;height:7px;border-radius:50%;background:#34d399;box-shadow:0 0 10px #34d399;}
.stratum-dpi-title{margin:0 auto !important;padding:0 !important;border:0 !important;max-width:960px;color:#f8fafc !important;font-size:clamp(1.35rem,2.2vw,1.88rem) !important;font-weight:800 !important;line-height:1.24 !important;letter-spacing:-0.02em;}
.stratum-dpi-metrics{position:relative;z-index:3;max-width:980px;margin:1.15rem auto 0;display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:1rem;}
.stratum-dpi-metric{padding:0.9rem 1.05rem;border-radius:14px;background:rgba(10,18,35,0.86);backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);border:1px solid rgba(148,163,184,0.18);box-shadow:0 10px 24px rgba(2,6,23,0.4);text-align:center;}
.stratum-dpi-metric--amber{border-top:2px solid rgba(251,191,36,0.8);}
.stratum-dpi-metric--cyan{border-top:2px solid rgba(56,189,248,0.8);}
.stratum-dpi-metric--emerald{border-top:2px solid rgba(52,211,153,0.8);}
.stratum-dpi-val{display:block;font-size:clamp(1.45rem,2.05vw,1.92rem);font-weight:800;line-height:1.15;letter-spacing:-0.02em;font-variant-numeric:tabular-nums;white-space:nowrap;margin-bottom:0.28rem;}
.stratum-dpi-val--amber{color:#fbbf24 !important;}
.stratum-dpi-val--cyan{color:#38bdf8 !important;}
.stratum-dpi-val--emerald{color:#34d399 !important;}
.stratum-dpi-lbl{display:block;color:#cbd5e1 !important;font-size:0.82rem;font-weight:600;line-height:1.35;}
.stratum-dpi-visual{position:relative;z-index:1;margin:0.5rem auto 0;max-width:1100px;}
.stratum-dpi-visual img{display:block;width:100% !important;max-width:100% !important;height:auto !important;margin:0 !important;padding:0 !important;border:0 !important;border-radius:0 !important;box-shadow:none !important;-webkit-mask-image:linear-gradient(180deg,transparent 0%,#000 6%,#000 95%,transparent 100%);mask-image:linear-gradient(180deg,transparent 0%,#000 6%,#000 95%,transparent 100%);}
.stratum-dpi-bottom{position:relative;z-index:3;padding:1.4rem 2.25rem 2.05rem;background:linear-gradient(180deg,rgba(7,13,25,0.88) 0%,rgba(5,9,18,0.98) 100%);border-top:1px solid rgba(148,163,184,0.14);text-align:center;}
.stratum-dpi-desc{max-width:880px;margin:0 auto 1.1rem !important;color:#cbd5e1 !important;font-size:1rem !important;line-height:1.72 !important;}
.stratum-dpi-desc strong{color:#f8fafc !important;font-weight:700;}
.stratum-dpi-pills{display:flex;flex-wrap:wrap;justify-content:center;gap:0.55rem;}
.stratum-dpi-pill{display:inline-flex;align-items:center;padding:0.34rem 0.8rem;border-radius:999px;background:rgba(30,41,59,0.65);border:1px solid rgba(148,163,184,0.22);color:#e2e8f0 !important;font-size:0.78rem;font-weight:600;}
@media (max-width:900px){.stratum-dpi-top{padding:1.5rem 1.25rem 0.4rem;}.stratum-dpi-val{white-space:normal;}.stratum-dpi-bottom{padding:1.25rem 1.25rem 1.65rem;}}
@media (max-width:680px){.stratum-dpi-metrics{grid-template-columns:1fr;gap:0.65rem;}.stratum-dpi-visual{margin-top:0.35rem;}}
</style>
<section class="stratum-dpi-showcase">
  <div class="stratum-dpi-top">
    <div class="stratum-dpi-eyebrow"><span class="stratum-dpi-dot"></span>Live Production Telemetry</div>
    <h2 class="stratum-dpi-title">Field-Proven Deep Packet Inspection at Scale</h2>
    <div class="stratum-dpi-metrics">
      <div class="stratum-dpi-metric stratum-dpi-metric--amber">
        <span class="stratum-dpi-val stratum-dpi-val--amber">37,287</span>
        <span class="stratum-dpi-lbl">Active ET Open Suricata Rules</span>
      </div>
      <div class="stratum-dpi-metric stratum-dpi-metric--cyan">
        <span class="stratum-dpi-val stratum-dpi-val--cyan">2.11B+</span>
        <span class="stratum-dpi-lbl">Live Production Packets Inspected</span>
      </div>
      <div class="stratum-dpi-metric stratum-dpi-metric--emerald">
        <span class="stratum-dpi-val stratum-dpi-val--emerald">0.00%</span>
        <span class="stratum-dpi-lbl">Kernel Buffer Packet Drop</span>
      </div>
    </div>
  </div>
  <div class="stratum-dpi-visual">
    <img src="/images/features/dpi-telemetry.jpg" alt="Stratum Firewall FreeBSD 15.1 pf and Suricata 8 DPI Live Production Telemetry" loading="lazy" decoding="async">
  </div>
  <div class="stratum-dpi-bottom">
    <p class="stratum-dpi-desc">While conventional firewall distributions often suffer kernel buffer exhaustion and packet drops once tens of thousands of deep packet inspection signatures are enabled, <strong>Stratum Firewall</strong> pairs pure <strong>FreeBSD 15.1 pf</strong> kernel forwarding with RAM-aware <strong>Suricata 8</strong> inspection — sustaining <strong>2.11+ billion packets</strong> across live multi-WAN, multi-VLAN enterprise traffic under <strong>37,287 active ET Open rules</strong> with <strong>0.00% packet loss</strong>.</p>
    <div class="stratum-dpi-pills">
      <span class="stratum-dpi-pill">Pure FreeBSD 15.1 pf</span>
      <span class="stratum-dpi-pill">Suricata 8 Dual-Engine (netmap + pcap)</span>
      <span class="stratum-dpi-pill">RAM-Aware Auto-Scaling</span>
      <span class="stratum-dpi-pill">Multi-WAN & Multi-VLAN</span>
    </div>
  </div>
</section>
<!-- STRATUM-DPI-SHOWCASE-END -->

<!-- STRATUM-OPNSENSE-SHOWCASE-START -->
<div class="opn-feature-showcase">
  <div class="opn-showcase-head">
    <span class="opn-showcase-eyebrow">Drop-In Migration Engine &middot; Zero Rebuild Required</span>
    <h2>Direct OPNsense <code>config.xml</code> &amp; OpenVPN Migration</h2>
    <p>Switching firewall distributions shouldn't mean spending an entire weekend re-typing hundreds of rules or re-issuing VPN profiles to every laptop. Upload your existing OPNsense <code>config.xml</code> backup under <strong>System &rarr; Backup &rarr; Import from OPNsense</strong> to preview, map interfaces, and cut over cleanly in minutes.</p>
  </div>

  <div class="opn-showcase-visual">
    <img src="/images/features/opnsense-migration.jpg" alt="Direct OPNsense config.xml backup migration to Stratum Firewall FreeBSD 15.1 architecture" width="1280" height="714" loading="eager">
    <div class="opn-showcase-caption">
      <span><b>SOURCE:</b> OPNsense config.xml (Rules, VLANs, VIPs, DHCP, DNS, OpenVPN PKI)</span>
      <span><b>ENGINE:</b> Interactive Port Mapper &amp; Dry-Run Validator &rarr; Stratum FW v0.30.0</span>
    </div>
  </div>

  <div class="opn-showcase-grid">
    <div class="opn-mig-card">
      <h3>1. Interactive Port, VLAN &amp; VIP Mapping</h3>
      <p>Map OPNsense NICs onto your target appliance's physical ports before saving, carrying over <code>802.1Q</code> VLANs, interface groups, static routes, <code>/29</code> IP-alias Virtual IPs, and static/PPPoE/DHCP Multi-WAN interfaces.</p>
    </div>
    <div class="opn-mig-card">
      <h3>2. Stateful pf Rules, NAT &amp; Policy Routing</h3>
      <p>Imports both legacy and MVC firewall rules, outbound NAT, and port forwards with their original source restrictions intact. Per-rule WAN gateways translate to Policy Routing, while scheduled rules import safely disabled for review.</p>
    </div>
    <div class="opn-mig-card">
      <h3>3. Zero-Touch OpenVPN Server Takeover</h3>
      <p>Transfers your OpenVPN CA, server certificate, <code>tls-crypt</code> / <code>tls-auth</code> keys, client certs, and user passwords (supporting PHP <code>$2y$</code> bcrypt and <code>argon2id</code>) so existing remote laptops and phones reconnect unchanged.</p>
    </div>
    <div class="opn-mig-card">
      <h3>4. Aliases, GeoIP &amp; Threat Blocklist Feeds</h3>
      <p>Flattens nested aliases, turns FQDN entries into live 5-minute Hostname aliases, maps GeoIP rules to license-free 5-RIR Country tables, and activates matching catalog IP blocklists (such as <code>CINS Army</code> or <code>blocklist.de</code>).</p>
    </div>
    <div class="opn-mig-card">
      <h3>5. Force DNS, DHCP &amp; Shaper Continuity</h3>
      <p>Migrates <code>dnsmasq</code> and <code>ISC dhcpd</code> ranges, static MAC leases, Unbound host/domain overrides, <code>dummynet</code> bandwidth limits, and converts DNS redirect port forwards (with exception aliases) into native <strong>Force DNS</strong>.</p>
    </div>
    <div class="opn-mig-card">
      <h3>6. Privacy-Safe Audit &amp; 60s Safe-Apply</h3>
      <p>Your uploaded XML is parsed in memory and never stored on disk. A detailed pre-apply report flags translated items, LAN/WAN subnet conflicts, and unsupported objects—backed by Stratum's 60-second automatic rollback timer.</p>
    </div>
  </div>

  <div class="opn-showcase-actions">
    <span class="opn-showcase-note">Tested with production OPNsense profiles, multi-VLAN topologies, and live OpenVPN road-warrior clients.</span>
    <div class="opn-showcase-btns">
      <a href="/resources/documentation/opnsense-migration/" class="btn btn-primary">Read Migration Guide &rarr;</a>
      <a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="btn btn-secondary" style="background:#FFFFFF;color:#0F172A;">Inspect in Live Demo</a>
    </div>
  </div>
</div>
<!-- STRATUM-OPNSENSE-SHOWCASE-END -->

<!-- STRATUM-V0300-FEATURES-START -->
<section class="section stratum-dark-module">
  <div class="section-head" style="max-width: 820px; margin: 0 auto 28px auto; text-align: center;">
    <span class="eyebrow stratum-module-eyebrow">New in v0.28.8 – v0.31.1</span>
    <h2>Enterprise Identity, Unbypassable DNS &amp; Dynamic Multi-WAN</h2>
    <p>Every capability below ships built into Stratum Firewall v0.31.1 on FreeBSD 15.1—with zero plugins, zero external cloud dependencies, and zero recurring license fees.</p>
  </div>
  <div class="card-grid grid-3 stratum-subgrid">
<div class="card stratum-subcard"><h3>Rule &amp; NAT Separators + Views (v0.31.1)</h3><p>Group Firewall, Port Forward, and manual Outbound NAT rules with named, coloured pfSense-style separators and up/down arrows, and filter cleanly across All, Default, and Custom views.</p></div>
<div class="card stratum-subcard"><h3>HTTPS-Only Sessions &amp; Multi-WAN VPN (v0.30.5)</h3><p>Protects panel sessions with strict HTTPS-only Secure cookies, writes ordered per-address OpenVPN remote lines for multi-WAN profiles, and ships FreeBSD-ipfw dummynet built in.</p></div>
<div class="card stratum-subcard"><h3>OPNsense DNSBL &amp; Full-Span DHCP Import (v0.30.2)</h3><p>Imports OPNsense Unbound DNSBL feeds (Steven Black, AdGuard, custom HTTPS lists, allow/block/wildcards) and preserves full-span DHCP pools that cross the interface gateway IP.</p></div>
    <div class="card stratum-subcard">
      <h3>Built-In TOTP Two-Factor Sign-In (<code>v0.30.0</code>)</h3>
      <p>Protect administrator and operator accounts with 6-digit authenticator codes (<em>Google/Microsoft Authenticator, 1Password, Bitwarden</em>). QR codes render locally in your browser so secrets never leave the firewall—backed by single-use replay protection, 10 recovery codes, and console reset.</p>
    </div>
    <div class="card stratum-subcard">
      <h3>Force DNS with AD Exceptions (<code>v0.29.2</code>)</h3>
      <p>Redirect hardcoded client DNS queries (<code>8.8.8.8</code>) on selected internal VLANs to Stratum's resolver and block DNS over TLS (port 853) so DNSBL and the 55-category Web Filter cannot be bypassed—with an Exception alias for Active Directory domain controllers and zero browsing logs.</p>
    </div>
    <div class="card stratum-subcard">
      <h3>Dynamic DHCP Multi-WAN (<code>v0.29.1</code>)</h3>
      <p>Attach cable, 4G/5G LTE, or Starlink modems as secondary or tertiary DHCP WANs without hijacking your default route or DNS. Failover, symmetric <code>reply-to</code>, and Policy Routing follow ISP lease gateway changes within seconds without an Apply.</p>
    </div>
    <div class="card stratum-subcard">
      <h3>Time-Based Rule Schedules (<code>v0.29.0</code>)</h3>
      <p>Bind reusable time windows (<code>Mon-Fri 08:00-18:00</code>) to any <code>pf</code> rule. Evaluated every minute via <code>stratum-pfrefresh</code> without an Apply, automatically closing open sessions created by scheduled <code>pass</code> rules the moment the window ends.</p>
    </div>
    <div class="card stratum-subcard">
      <h3>Bidirectional 1:1 NAT (<code>v0.28.9</code>)</h3>
      <p>Map internal servers or subnets 1:1 to public WAN Virtual IPs or routed blocks in both directions—complete with anti-lockout protection that refuses the WAN's own management IP and live WAN pass-rule visibility.</p>
    </div>
    <div class="card stratum-subcard">
      <h3>1h / 24h Firewall Log Summary (<code>v0.28.8</code>)</h3>
      <p>Analyze top source IPs with GeoIP country flags, top targeted service ports (<code>SSH</code>, <code>RDP</code>), top countries, human-readable system rule names, and 5-minute/hourly interface charts—with one-click drilldown into the live log.</p>
    </div>
  </div>
</section>
<!-- STRATUM-V0300-FEATURES-END -->

<style>
.feature-showcase-page {
  width: 100%;
  max-width: 100%;
}
.feature-module {
  margin-bottom: 88px;
  padding-bottom: 72px;
  border-bottom: 1px solid #e2e8f0;
}
.feature-module:last-of-type {
  margin-bottom: 24px;
  padding-bottom: 0;
  border-bottom: none;
}
.feature-module-head {
  width: 100%;
  max-width: 100%;
  margin-bottom: 28px;
}
.feature-badge-pill {
  display: inline-block;
  padding: 6px 14px;
  background: #fffbeb;
  color: #d97706;
  border: 1px solid rgba(245, 158, 11, 0.35);
  font-size: 12.5px;
  font-weight: 700;
  border-radius: 30px;
  margin-bottom: 14px;
  text-transform: uppercase;
  letter-spacing: 0.08em;
}
.feature-module-head h2 {
  font-family: var(--font-display);
  margin: 0 0 14px 0;
  font-size: 34px;
  font-weight: 800;
  line-height: 1.22;
  letter-spacing: -0.015em;
  color: #0f1e3d;
}
.feature-module-lead {
  font-size: 19px;
  font-weight: 600;
  color: #1e293b;
  margin: 0 0 14px 0;
  line-height: 1.6;
}
.feature-module-desc {
  font-size: 16.5px;
  color: #475569;
  line-height: 1.8;
  margin: 0;
}
.feature-visual-box {
  width: 100%;
  margin: 32px 0 36px 0;
  border-radius: 18px;
  overflow: hidden;
  box-shadow: 0 24px 48px rgba(15, 30, 61, 0.16);
  border: 1px solid rgba(15, 30, 61, 0.08);
  background: #0b1320;
}
.feature-visual-box img {
  display: block;
  width: 100%;
  height: auto;
  object-fit: cover;
}
.feature-module .card-grid.grid-3 {
  margin-top: 28px;
}
.feature-module .card h3 {
  font-family: var(--font-display);
  margin: 0 0 10px 0;
  font-size: 18.5px;
  font-weight: 700;
  color: #0f1e3d;
}
.feature-module .card p {
  margin: 0;
  font-size: 15px;
  line-height: 1.65;
  color: #475569;
}
</style>
<div class="feature-showcase-page">
<div class="feature-module stratum-dark-pillar" id="network-topology">
<div class="feature-module-head">
<span class="feature-badge-pill stratum-module-eyebrow">NETWORK TOPOLOGY &amp; VLAN SEGMENTATION</span>
<h2>Complete Network Isolation with Multi-VLAN Segmentation</h2>
<p class="feature-module-lead">Manage physical network interfaces and virtual LANs (VLANs) from a unified control plane to securely isolate office workstations, guest Wi-Fi, staff devices, and IoT traffic.</p>
<p class="feature-module-desc">Stratum Firewall provides a flexible multi-interface networking architecture designed to simplify complex enterprise and homelab topologies. Administrators can map physical network cards (such as <code>pppoe0</code> WAN, <code>igb1</code> LAN, <code>igb2</code> OPT1, and <code>igb3</code> OPT2) and carve out logically isolated 802.1Q VLAN segments—including Office LAN, Guest Wi-Fi, Staff, and Security Cameras—over shared physical cabling. With integrated DHCP Server, DNS Resolver, and NTP time synchronization services, every network zone operates with its own dedicated IP addressing, subnet boundaries, and inter-VLAN firewall policies.</p>
</div>
<div class="feature-visual-box stratum-pillar-media">
<img src="/images/features/topology-vlan.jpg" alt="Stratum Firewall multi-interface WAN, Office LAN, Guest Wi-Fi, IoT VLAN segmentation, and WireGuard VPN topology diagram" loading="lazy">
</div>
<div class="card-grid grid-3 stratum-subgrid">
<div class="card stratum-subcard">
<h3>Multi-Interface &amp; WAN Mapping</h3>
<p>Configure dedicated WAN, LAN, and optional (<code>OPT1</code>/<code>OPT2</code>) physical interfaces with native PPPoE, static IPv4/IPv6, and dynamic DHCP client support.</p>
</div>
<div class="card stratum-subcard">
<h3>Granular VLAN Segmentation</h3>
<p>Isolate untrusted IoT devices, IP cameras, and guest wireless clients into separate broadcast domains to eliminate lateral threat movement across your network.</p>
</div>
<div class="card stratum-subcard">
<h3>Integrated Core Network Services</h3>
<p>Run independent DHCP address pools, static MAC leases, local DNS resolution, and NTP time synchronization seamlessly across every interface and VLAN.</p>
</div>
</div>
</div>
<div class="feature-module stratum-dark-pillar" id="pf-kernel-shield">
<div class="feature-module-head">
<span class="feature-badge-pill stratum-module-eyebrow">KERNEL-LEVEL PF PACKET FILTER</span>
<h2>Uncompromising Network Shield with Native FreeBSD pf Filtering</h2>
<p class="feature-module-lead">Powered directly by the FreeBSD 15.1/15 kernel pf engine, delivering wire-speed stateful packet inspection, ALTQ traffic shaping, and fail-safe rule validation.</p>
<p class="feature-module-desc">At the core of Stratum Firewall lies FreeBSD's battle-tested kernel-level <code>pf</code> (Packet Filter) engine. Every incoming and outgoing packet is evaluated against active state tables directly inside the operating system kernel, neutralizing unauthorized connection attempts while forwarding legitimate traffic at wire speed with minimal CPU overhead. Advanced Network Address Translation (Outbound &amp; Port Forward NAT) and ALTQ bandwidth shaping keep latency-sensitive applications responsive even under heavy load. Before any rule or interface modification is committed, our <strong>Safe-Apply</strong> workflow validates the generated syntax against <code>pfctl -nf</code> to guarantee you never lock yourself out.</p>
</div>
<div class="feature-visual-box stratum-pillar-media">
<img src="/images/features/pf-kernel-shield.jpg" alt="Stratum Firewall FreeBSD kernel-level pf packet filtering and multi-layered network defense shield illustration" loading="lazy">
</div>
<div class="card-grid grid-3 stratum-subgrid">
<div class="card stratum-subcard">
<h3>Native Kernel Stateful Inspection</h3>
<p>High-throughput stateful packet filtering executed directly in the FreeBSD 15.1/15 kernel with zero user-space packet copying bottlenecks.</p>
</div>
<div class="card stratum-subcard">
<h3>Safe-Apply Syntax Validation</h3>
<p>Automatic pre-flight verification of all firewall rulesets and interface assignments before applying changes to the active kernel state.</p>
</div>
<div class="card stratum-subcard">
<h3>NAT, ALTQ &amp; Live Rule Telemetry</h3>
<p>Manage reusable IP/Port Aliases, granular <code>Pass / Block / Reject</code> rules, real-time rule hit counters, and ALTQ traffic queues from a single view.</p>
</div>
</div>
</div>
<div class="feature-module stratum-dark-pillar" id="dnsbl-web-filter">
<div class="feature-module-head">
<span class="feature-badge-pill stratum-module-eyebrow">THREAT INTELLIGENCE &amp; DNSBL</span>
<h2>Proactive Web Filtering (DNSBL) and Automated IP Blocklists</h2>
<p class="feature-module-lead">Neutralize malicious domains and known attacker IPs before they ever reach your endpoints using DNS-level sinkholing and automated threat intelligence feeds.</p>
<p class="feature-module-desc">Stratum Firewall reinforces traditional port and protocol rules with dual-layer threat intelligence. The <strong>IP Blocklists</strong> module automatically fetches and parses public or custom threat intelligence feeds, loading known botnet command-and-control servers, scanners, and abusive IP ranges into high-speed kernel <code>pf</code> tables to drop hostile packets on sight. Simultaneously, the <strong>DNS-Based Web Filtering (DNSBL)</strong> engine intercepts outbound DNS queries for phishing, malware, telemetry, and advertising domains—returning an immediate <code>NXDOMAIN</code> response across every device on your network without installing client-side software.</p>
</div>
<div class="feature-visual-box stratum-pillar-media">
<img src="/images/features/dnsbl-web-filter.jpg" alt="Stratum Firewall DNSBL domain sinkholing and threat intelligence IP blocklist filtration layers" loading="lazy">
</div>
<div class="card-grid grid-3 stratum-subgrid">
<div class="card stratum-subcard">
<h3>Network-Wide DNS Sinkholing</h3>
<p>Block malware, phishing, tracking, and ad domains at the DNS resolution layer with instant <code>NXDOMAIN</code> responses for all connected clients.</p>
</div>
<div class="card stratum-subcard">
<h3>Automated Threat Feed IP Blocking</h3>
<p>Ingest external IP reputation feeds into kernel-level <code>pf</code> tables to drop traffic from hostile networks at the perimeter with zero latency penalty.</p>
</div>
<div class="card stratum-subcard">
<h3>Real-Time Domain Lookup Tool</h3>
<p>Instantly query and verify whether any specific domain name is blocked or permitted by your active DNSBL policies directly from the management UI.</p>
</div>
</div>
</div>
<div class="feature-module stratum-dark-pillar" id="modern-api-architecture">
<div class="feature-module-head">
<span class="feature-badge-pill stratum-module-eyebrow">MODERN ARCHITECTURE &amp; API CONTROL</span>
<h2>Zero-Dependency Cybersecurity Architecture with Modern API Control</h2>
<p class="feature-module-lead">"Raw FreeBSD power. Modern API control." Experience a lightweight Python 3.12 &amp; FastAPI backend, declarative config.json state management, and native WireGuard cryptography.</p>
<p class="feature-module-desc">Engineered from the ground up to replace bloated legacy PHP/XML firewall interfaces, Stratum Firewall pairs raw FreeBSD kernel performance with a clean, developer-first control plane. Its asynchronous <strong>Python 3.12</strong> and <strong>FastAPI</strong> backend serves both a responsive web UI and a comprehensive <strong>REST API</strong> while maintaining an ultra-low memory footprint. The entire system state lives in a single human-readable, declarative <code>config.json</code> file that dynamically compiles into native <code>pf.conf</code>, DHCP, and DNS files. Combined with zero-dependency X25519 (RFC 7748) <strong>WireGuard VPN</strong> and <strong>OpenVPN</strong> support, Role-Based Access Control (RBAC), and live hardware/log telemetry, Stratum brings true Infrastructure-as-Code simplicity to network security.</p>
</div>
<div class="feature-visual-box stratum-pillar-media">
<img src="/images/features/api-architecture.jpg" alt="Stratum Firewall Python FastAPI REST API control plane, WireGuard X25519 encryption, and live telemetry architecture" loading="lazy">
</div>
<div class="card-grid grid-3 stratum-subgrid">
<div class="card stratum-subcard">
<h3>FastAPI &amp; Declarative config.json</h3>
<p>Control the entire appliance state through a single auditable JSON source of truth and automate every firewall operation via an asynchronous REST API.</p>
</div>
<div class="card stratum-subcard">
<h3>Zero-Dependency WireGuard &amp; OpenVPN</h3>
<p>Native RFC 7748 X25519 cryptographic key generation written from scratch for high-speed WireGuard tunnels alongside full OpenVPN support.</p>
</div>
<div class="card stratum-subcard">
<h3>RBAC &amp; Live System Telemetry</h3>
<p>Enforce Administrator, Operator, and Read-Only access roles while monitoring real-time CPU, memory, disk, thermal, traffic, and live firewall logs.</p>
</div>
</div>
</div>
</div>

<!-- STRATUM-UNIFIED-DARK-STYLE-START -->
<style>
.opn-feature-showcase,.stratum-dark-module,.stratum-dark-pillar{position:relative;width:100% !important;max-width:100% !important;box-sizing:border-box;margin:2.5rem 0 !important;border-radius:20px !important;overflow:hidden;background:radial-gradient(120% 90% at 50% 0%,#101f3c 0%,#08101f 52%,#060b16 100%) !important;border:1px solid rgba(56,189,248,0.22) !important;box-shadow:0 24px 60px rgba(2,6,23,0.55),inset 0 1px 0 rgba(255,255,255,0.06) !important;}
.stratum-dark-module,.stratum-dark-pillar{padding:2.3rem 2.25rem 2.2rem !important;text-align:center;}
.stratum-dark-module > .wrap,.stratum-dark-module > .container,.stratum-dark-pillar > .wrap,.stratum-dark-pillar > .container{max-width:100% !important;width:100% !important;padding:0 !important;margin:0 !important;}
.feature-showcase-page{max-width:100% !important;width:100% !important;margin:0 !important;padding:0 !important;background:transparent !important;border:0 !important;box-shadow:none !important;}
.feature-showcase-page hr,.stratum-dark-pillar hr{display:none !important;}
.stratum-dark-module .stratum-module-eyebrow,.stratum-dark-pillar .stratum-module-eyebrow{display:inline-flex !important;align-items:center;justify-content:center;gap:0.45rem;padding:0.32rem 0.9rem !important;margin:0 auto 0.85rem !important;border-radius:999px !important;background:rgba(56,189,248,0.12) !important;border:1px solid rgba(56,189,248,0.32) !important;color:#38bdf8 !important;font-size:0.74rem !important;font-weight:700 !important;letter-spacing:0.09em !important;text-transform:uppercase !important;width:auto !important;}
.stratum-dark-module .stratum-module-eyebrow{background:rgba(245,158,11,0.12) !important;border-color:rgba(245,158,11,0.34) !important;color:#fbbf24 !important;}
.stratum-dark-module h1,.stratum-dark-module h2,.stratum-dark-pillar h2{margin:0 auto 0.8rem !important;padding:0 !important;border:0 !important;max-width:920px !important;color:#f8fafc !important;font-size:clamp(1.38rem,2.25vw,1.92rem) !important;font-weight:800 !important;line-height:1.24 !important;letter-spacing:-0.02em !important;text-align:center !important;}
.stratum-dark-module p,.stratum-dark-pillar p{color:#cbd5e1 !important;font-size:1rem !important;line-height:1.7 !important;margin:0 auto 1.5rem !important;max-width:860px;text-align:center;}
.stratum-dark-pillar .stratum-pillar-media{background:transparent !important;border:0 !important;box-shadow:none !important;padding:0 !important;margin:0.4rem auto 1.65rem !important;max-width:1040px !important;border-radius:16px !important;overflow:hidden;}
.stratum-dark-pillar img{display:block;width:100% !important;max-width:100% !important;height:auto !important;margin:0 auto !important;padding:0 !important;border:0 !important;border-radius:14px !important;box-shadow:none !important;-webkit-mask-image:linear-gradient(180deg,transparent 0%,#000 5%,#000 95%,transparent 100%);mask-image:linear-gradient(180deg,transparent 0%,#000 5%,#000 95%,transparent 100%);}
.stratum-dark-module .stratum-subgrid,.stratum-dark-pillar .stratum-subgrid{display:grid !important;grid-template-columns:repeat(auto-fit,minmax(275px,1fr)) !important;gap:1rem !important;margin-top:1.25rem !important;text-align:left !important;}
.stratum-dark-module .stratum-subcard,.stratum-dark-pillar .stratum-subcard{padding:1.2rem 1.25rem !important;border-radius:14px !important;background:rgba(10,18,35,0.84) !important;backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);border:1px solid rgba(148,163,184,0.18) !important;border-top:2px solid rgba(56,189,248,0.75) !important;box-shadow:0 10px 24px rgba(2,6,23,0.4) !important;text-align:left !important;transition:transform 0.18s ease,border-color 0.18s ease;}
.stratum-dark-module .stratum-subcard:nth-child(3n+1),.stratum-dark-pillar .stratum-subcard:nth-child(3n+1){border-top-color:rgba(251,191,36,0.8) !important;}
.stratum-dark-module .stratum-subcard:nth-child(3n+2),.stratum-dark-pillar .stratum-subcard:nth-child(3n+2){border-top-color:rgba(56,189,248,0.8) !important;}
.stratum-dark-module .stratum-subcard:nth-child(3n+3),.stratum-dark-pillar .stratum-subcard:nth-child(3n+3){border-top-color:rgba(52,211,153,0.8) !important;}
.stratum-dark-module .stratum-subcard:hover,.stratum-dark-pillar .stratum-subcard:hover{transform:translateY(-2px);border-color:rgba(148,163,184,0.34) !important;}
.stratum-dark-module .stratum-subcard h3,.stratum-dark-module .stratum-subcard h4,.stratum-dark-pillar .stratum-subcard h3,.stratum-dark-pillar .stratum-subcard h4{color:#f8fafc !important;font-size:1.02rem !important;font-weight:700 !important;line-height:1.35 !important;margin:0 0 0.5rem !important;text-align:left !important;}
.stratum-dark-module .stratum-subcard p,.stratum-dark-module .stratum-subcard li,.stratum-dark-pillar .stratum-subcard p,.stratum-dark-pillar .stratum-subcard li{color:#cbd5e1 !important;font-size:0.92rem !important;line-height:1.62 !important;margin:0 !important;max-width:none !important;text-align:left !important;}
.stratum-dark-module .stratum-subcard strong,.stratum-dark-module .stratum-subcard em,.stratum-dark-pillar .stratum-subcard strong,.stratum-dark-pillar .stratum-subcard em{color:#f8fafc !important;}
.stratum-dark-module .stratum-subcard code,.stratum-dark-pillar .stratum-subcard code{background:rgba(56,189,248,0.12) !important;color:#7dd3fc !important;border:1px solid rgba(56,189,248,0.24) !important;padding:0.1rem 0.36rem !important;border-radius:5px !important;font-size:0.84em !important;}
@media (max-width:900px){.stratum-dark-module,.stratum-dark-pillar{padding:1.65rem 1.3rem 1.65rem !important;}}
@media (max-width:680px){.stratum-dark-module .stratum-subgrid,.stratum-dark-pillar .stratum-subgrid{grid-template-columns:1fr !important;gap:0.75rem !important;}}
</style>
<!-- STRATUM-UNIFIED-DARK-STYLE-END -->
