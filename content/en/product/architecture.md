---
title: "System Architecture"
seo_title: "Stratum Firewall v0.30.0 Architecture — FreeBSD 15.1, pf, FastAPI, TOTP 2FA & ZFS"
description: "Inside Stratum Firewall v0.30.3 — FreeBSD 15.1-RELEASE kernel pf & 1:1 binat data plane, dual-engine Suricata 8 IPS, Force DNS with AD exceptions, Python 3.12 FastAPI control plane with TOTP 2FA, and ZFS Boot Environments."
lead: "Zero PHP overhead, a single declarative source of truth, a 60-second Safe-Apply rollback guarantee, and a pure FreeBSD 15.1 kernel data plane."
full_width: true
---

<div class="solutions-layout">
<div class="sol-block">
<div class="sol-head">
<span class="eyebrow">01 · Four-Layer Appliance Stack (v0.30.0)</span>
<h2>Strict separation between control plane and kernel data plane</h2>
<p>Legacy firewall distributions entangle web UI rendering, plugins, and system orchestration inside a monolithic PHP/XML runtime. Stratum Firewall keeps line-rate packet forwarding strictly inside the <strong>FreeBSD 15.1-RELEASE</strong> kernel while isolating management, validation, and telemetry inside an asynchronous <strong>Python 3.12 &amp; FastAPI</strong> control plane.</p>
</div>
<div class="arch-stack">
<div class="arch-layer">
<div class="arch-layer-meta">
<span class="arch-badge">Layer 04 · Control &amp; Identity</span>
<h3>Async Management &amp; Observability Plane</h3>
</div>
<div class="arch-layer-body">
<p>Served behind an <strong>Nginx TLS</strong> reverse proxy, the <strong>Python 3.12 / FastAPI</strong> application powers the responsive web UI, REST API, customizable 12-column dashboard, and <strong>1h/24h Firewall Log Summary</strong> analytics. Accounts are protected by local-QR <strong>TOTP Two-Factor Sign-In</strong> (<code>v0.30.0</code>), 10 one-time recovery codes, brute-force sign-in protection, and 4-tier RBAC.</p>
<div class="arch-pills">
<span class="arch-pill"><b>UI/API:</b> Python 3.12 + FastAPI</span>
<span class="arch-pill"><b>Identity:</b> On-Box QR TOTP 2FA + RBAC</span>
<span class="arch-pill"><b>AI:</b> Read-Only MCP Server (15+ Tools)</span>
<span class="arch-pill"><b>Telemetry:</b> bsnmpd + Remote Syslog + Telegram/SMTP</span>
</div>
</div>
</div>
<div class="arch-layer">
<div class="arch-layer-meta">
<span class="arch-badge">Layer 03 · State &amp; Orchestration</span>
<h3>Declarative Compiler &amp; Zero-Apply Refresh</h3>
</div>
<div class="arch-layer-body">
<p>All network intent lives in one versioned, diff-auditable <code>config.json</code> file, while passwords, <strong>TOTP 2FA secrets</strong>, and VPN/TLS private keys remain isolated in root-only (<code>0600</code>) files. <code>stratum-config</code> pre-validates every generated daemon file before starting the <strong>60s Safe-Apply</strong> rollback timer, while <code>stratum-pfrefresh</code> updates scheduled rules and dynamic DHCP WAN gateways live without an Apply.</p>
<div class="arch-pills">
<span class="arch-pill"><b>State:</b> config.json + SQLite WAL</span>
<span class="arch-pill"><b>Compiler:</b> stratum-config (Pre-Checked)</span>
<span class="arch-pill"><b>Live pf:</b> stratum-pfrefresh + stratum-netextra</span>
<span class="arch-pill"><b>Migration:</b> OPNsense XML + OpenVPN PKI Importer</span>
</div>
</div>
</div>
<div class="arch-layer">
<div class="arch-layer-meta">
<span class="arch-badge">Layer 02 · Threat &amp; Tunnel Daemons</span>
<h3>Deep Packet Inspection, Force DNS &amp; PKI</h3>
</div>
<div class="arch-layer-body">
<p><strong>Suricata 8</strong> supervises simultaneous inline <code>netmap</code> IPS and passive <code>pcap</code> IDS processes per interface with hitless <code>SIGUSR2</code> rule reloads. <strong>Unbound 1.26 RPZ</strong> DNSBL and the 55-category Web Filter become unbypassable with <strong>Force DNS</strong> (<code>v0.29.2</code>) and DoT (<code>TCP 853</code>) blocking—complete with an Active Directory exception alias and zero browsing history logged. <strong>OpenVPN</strong> runs with a built-in EC CA and Multi-WAN <code>multihome</code>.</p>
<div class="arch-pills">
<span class="arch-pill"><b>IPS/IDS:</b> Suricata 8 (netmap + pcap)</span>
<span class="arch-pill"><b>DNS:</b> Unbound 1.26 RPZ + Force DNS</span>
<span class="arch-pill"><b>Analytics:</b> Unified Blocklist Reports (7d)</span>
<span class="arch-pill"><b>SSL VPN:</b> OpenVPN EC CA + multihome</span>
</div>
</div>
</div>
<div class="arch-layer">
<div class="arch-layer-meta">
<span class="arch-badge">Layer 01 · Kernel &amp; ZFS Storage</span>
<h3>FreeBSD 15.1 Data Plane &amp; Atomic Boot</h3>
</div>
<div class="arch-layer-body">
<p>Inside the <strong>FreeBSD 15.1-RELEASE</strong> (<code>amd64</code>) kernel, <strong>pf</strong> executes stateful inspection, bidirectional <strong>1:1 NAT</strong> (<code>binat</code>), Port Forwards, Policy Routing, 63 IP threat feeds, and license-free 5-RIR GeoIP radix tables at wire speed—alongside kernel <strong>WireGuard</strong> (<code>if_wg</code>), <strong>dummynet</strong> traffic shaping, 1-second Static/PPPoE/<strong>DHCP Multi-WAN</strong> probes, and bootable <strong>ZFS Boot Environments</strong>.</p>
<div class="arch-pills">
<span class="arch-pill"><b>Kernel:</b> FreeBSD 15.1 pf + binat + dummynet</span>
<span class="arch-pill"><b>VPN:</b> Kernel WireGuard (if_wg)</span>
<span class="arch-pill"><b>WAN:</b> Static / PPPoE (mpd5) / Dynamic DHCP WAN2</span>
<span class="arch-pill"><b>Storage:</b> ZFS Boot Environments (bectl)</span>
</div>
</div>
</div>
</div>
</div>
<div class="sol-block">
<div class="sol-head">
<span class="eyebrow">02 · Packet &amp; Configuration Lifecycles</span>
<h2>Deterministic pipelines for both packets and human operators</h2>
<p>Stratum Firewall processes network frames and administrative changes through predictable, self-healing pipelines.</p>
</div>
<div class="sol-steps">
<div class="sol-step">
<span class="sol-step-num">PIPELINE 01 · DATA PLANE</span>
<h3>NIC &rarr; Suricata netmap &rarr; pf &rarr; dummynet</h3>
<p>Arriving frames pass inline <code>netmap</code> Suricata IPS rings (when enabled on that interface), kernel <code>pf</code> IP blocklist radix tables, <strong>Force DNS</strong> / Port Forward / <strong>1:1 NAT</strong> translation, interface-group &amp; scheduled rules, and <code>dummynet</code> bandwidth pipes before exiting the selected WAN gateway.</p>
</div>
<div class="sol-step">
<span class="sol-step-num">PIPELINE 02 · 60S SAFE-APPLY</span>
<h3>Pre-Flight Check &rarr; Atomic Load &rarr; Auto-Rollback</h3>
<p>On Apply, <code>stratum-config</code> renders native daemon files and verifies syntax via <code>pfctl -n</code>, <code>nginx -t</code>, <code>sshd -t</code>, and <code>unbound-checkconf</code>. Unless the administrator clicks <strong>Keep changes</strong> within <strong>60 seconds</strong>, Stratum automatically reverts to the last confirmed snapshot.</p>
</div>
<div class="sol-step">
<span class="sol-step-num">PIPELINE 03 · LIVE REFRESH</span>
<h3>stratum-pfrefresh &amp; Dynamic Tables</h3>
<p>Time-based rule schedules evaluate every minute and automatically close open states created by scheduled <code>pass</code> rules when their window expires. Secondary <strong>DHCP WAN</strong> gateway renewals, WAN failover transitions, 5-minute Hostname lookups, and IP feeds update live without requiring an Apply.</p>
</div>
</div>
</div>
<div class="sol-block">
<div class="sol-head">
<span class="eyebrow">03 · Subsystem Reference Matrix (v0.30.0)</span>
<h2>Kernel drivers, daemons, and isolation boundaries</h2>
<p>Every subsystem maps cleanly onto native FreeBSD 15.1 primitives with zero proprietary cloud dependencies.</p>
</div>
<div class="sol-table-wrap">
<table class="sol-table">
<thead>
<tr>
<th>Subsystem</th>
<th>Core Driver / Daemon</th>
<th>State &amp; Security Boundary</th>
<th>v0.30.0 Architectural Capabilities</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Base OS &amp; Storage</strong></td>
<td>FreeBSD 15.1-RELEASE · <code>ZFS</code> (<code>bectl</code>)</td>
<td><code>zroot/ROOT/*</code> (Newest 5 BEs retained)</td>
<td>Offline VGA <code>.iso</code> &amp; Serial <code>.img</code> installers; signed online and air-gapped <code>.sfu</code> updates snapshot a bootable ZFS environment before touching disk.</td>
</tr>
<tr>
<td><strong>Packet Filter, NAT &amp; Schedules</strong></td>
<td>Kernel <code>pf</code> · <code>dummynet</code> · <code>stratum-pfrefresh</code></td>
<td><code>/etc/pf.conf</code> + persistent radix tables</td>
<td>Interface groups, bidirectional <strong>1:1 NAT</strong> (refuses WAN primary IP to prevent lockout), Port Forwards + NAT Reflection, minute-accurate <strong>Schedules</strong> (auto state teardown), and WAN default-block noise toggle.</td>
</tr>
<tr>
<td><strong>Multi-WAN &amp; Routing</strong></td>
<td>Gateway Monitor · <code>mpd5</code> · <code>dhclient</code> · <code>stratum-netextra</code></td>
<td>Clean kernel VIP/route sync outside <code>rc.conf</code></td>
<td>1-second latency/loss probes, Static/PPPoE plus non-hijacking <strong>Dynamic DHCP additional WANs</strong> (<code>WAN2</code>/<code>WAN3</code>), symmetric <code>reply-to</code>, per-rule Policy Routing, and <code>/29</code> Virtual IPs.</td>
</tr>
<tr>
<td><strong>DNS, Force DNS &amp; Web Filter</strong></td>
<td><code>Unbound 1.26</code> (DNSSEC + RPZ) · <code>dnsmasq</code></td>
<td>Zero browsing history logged by Force DNS</td>
<td>pfBlockerNG + HaGeZi DNSBL, per-VLAN <strong>55-category Web Filter</strong> + forced Safe Search, <strong>Force DNS with Active Directory exception aliases</strong>, and DoT (<code>TCP 853</code>) blocking.</td>
</tr>
<tr>
<td><strong>Threat Feeds &amp; IPS/IDS</strong></td>
<td><code>Suricata 8</code> (<code>netmap</code> + <code>pcap</code>) · <code>stratum-ipbl</code></td>
<td>7-day root-only SQLite block report store</td>
<td>63 automated IPv4 feeds, license-free 5-RIR Country (GeoIP) aliases, per-interface dual-engine Suricata IPS/IDS (<code>SIGUSR2</code> live reloads), and pfBlockerNG-style <strong>Unified Blocklist Reports</strong>.</td>
</tr>
<tr>
<td><strong>Remote Access VPN</strong></td>
<td>Kernel <code>if_wg</code> (WireGuard) · <code>OpenVPN</code></td>
<td>On-box RFC 7748 X25519 &amp; built-in EC PKI</td>
<td>Wire-speed QR WireGuard provisioning (hitless peer updates) and Multi-WAN <code>multihome</code> OpenVPN with <code>tls-crypt</code>, CRL revocation, and <code>VPN-only</code> user accounts.</td>
</tr>
<tr>
<td><strong>Identity, 2FA &amp; Backups</strong></td>
<td>FastAPI Auth · Local TOTP · OpenSSH SFTP</td>
<td><code>0600 root</code> secret vault + <code>AES-256-GCM</code> (<code>scrypt</code>)</td>
<td>Browser-local QR <strong>TOTP 2FA</strong> (<code>v0.30.0</code>), single-use replay protection, 10 recovery codes, console 2FA reset (<code>stratum-menu</code>), and scheduled encrypted <code>.stbk</code> SFTP backups.</td>
</tr>
<tr>
<td><strong>OPNsense Migration Engine</strong></td>
<td>Declarative XML Translator</td>
<td>Interactive pre-flight report before Apply</td>
<td>Imports OPNsense <code>config.xml</code> VLANs, VIPs, rules, Policy Routing, <strong>DHCP WAN2</strong>, <strong>Force DNS redirects</strong>, catalog <strong>URL-table IP feeds</strong>, and OpenVPN PKI + <code>$2y$</code>/<code>argon2id</code> password hashes.</td>
</tr>
<tr>
<td><strong>Diagnostics, Log Summary &amp; MCP</strong></td>
<td><code>filterlog</code> Parser · <code>tcpdump</code> · MCP · <code>bsnmpd</code></td>
<td>Strictly read-only AI boundary + hashed tokens</td>
<td><strong>1h/24h Firewall Log Summary</strong> (top IPs, GeoIP countries, service ports, 5m/1h charts), Wireshark <code>.pcap</code> capture, ARP/WoL, and read-only <strong>MCP diagnostics</strong> for Claude, ChatGPT, and Gemini.</td>
</tr>
</tbody>
</table>
</div>
</div>
<div class="sol-block">
<div class="sol-head">
<span class="eyebrow">04 · Security Engineering Principles</span>
<h2>Hardened by default against lockouts and data leaks</h2>
<p>Every layer is engineered to protect both network perimeters and administrative operations.</p>
</div>
<div class="card-grid grid-3">
<div class="card">
<div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><circle cx="7.5" cy="15.5" r="5.5"/><path d="m21 2-9.6 9.6"/><path d="m15.5 7.5 3 3L22 7l-3-3"/></svg></div>
<h3>Isolated Secret Vault (<code>0600</code>)</h3>
<p>Password hashes, <strong>TOTP 2FA secrets</strong>, WireGuard/OpenVPN private keys, PPPoE credentials, and alert tokens never touch <code>config.json</code>—they live in dedicated <code>0600 root</code> files.</p>
</div>
<div class="card">
<div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="11" width="18" height="11" rx="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg></div>
<h3>Zero-Cloud TOTP QR Provisioning</h3>
<p>Two-factor sign-in QR codes are drawn directly inside your browser from a bundled library with zero external API calls; secrets never leave the firewall and are never shown again after setup.</p>
</div>
<div class="card">
<div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/><path d="m9 12 2 2 4-4"/></svg></div>
<h3>Anti-Self-Lockout Validators</h3>
<p>Changes that would lock out your active management IP on SSH/HTTPS—or bind the WAN interface's own IP to <strong>1:1 NAT</strong>—are rejected before touching the running ruleset.</p>
</div>
<div class="card">
<div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><polygon points="22 3 2 3 10 12.46 10 19 14 21 14 12.46 22 3"/></svg></div>
<h3>Privacy-First Force DNS</h3>
<p><strong>Force DNS</strong> enforces DNSBL and Web Filtering across selected VLANs independently of <em>Web Activity</em>—recording zero per-device browsing logs when enabled on its own.</p>
</div>
<div class="card">
<div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><polyline points="4 17 10 11 4 5"/><line x1="12" y1="19" x2="20" y2="19"/></svg></div>
<h3>100% Read-Only AI Boundary (MCP)</h3>
<p>The Model Context Protocol endpoint exposes zero mutating operations, hashes access tokens on disk, supports source-IP restrictions, and lets admins disable individual tools.</p>
</div>
<div class="card">
<div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M3 12a9 9 0 0 1 15-6.7L21 8"/><path d="M21 3v5h-5"/><path d="M21 12a9 9 0 0 1-15 6.7L3 16"/><path d="M3 21v-5h5"/></svg></div>
<h3>Clean Deterministic System Files</h3>
<p>Generated <code>/etc/rc.conf</code> and <code>/etc/pf.conf</code> outputs never fight FreeBSD defaults (<code>v0.28.7</code>), and pre-existing <code>/etc/sysctl.conf.local</code> settings are backed up automatically.</p>
</div>
</div>
</div>
<div class="sol-cta-box">
<div>
<h3>Inspect the v0.30.0 architecture live</h3>
<p>Explore a running FreeBSD 15.1 QEMU appliance right in your browser with zero registration, or download the offline ZFS installers.</p>
</div>
<div class="sol-cta-actions">
<a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="btn btn-primary">Launch Live Demo</a>
<a href="/download/" class="btn btn-secondary" style="background:rgba(255,255,255,0.12);color:#FFFFFF !important;border-color:rgba(255,255,255,0.28);">Download ISO / IMG</a>
<a href="/resources/documentation/" class="btn btn-secondary" style="background:rgba(255,255,255,0.12);color:#FFFFFF !important;border-color:rgba(255,255,255,0.28);">Documentation</a>
</div>
</div>
</div>
