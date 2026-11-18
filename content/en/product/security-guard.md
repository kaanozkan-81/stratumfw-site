---
title: "Security Guard"
seoTitle: "Stratum Security Guard: Free 5-Layer Firewall Threat Defense"
description: "The threat-blocking layers built into Stratum FW - 63 IP blocklists, 5-RIR GeoIP, 93 DNSBL feeds, 56-category Web Filter, and Suricata IPS. Included free, zero subscriptions."
seoDescription: "Explore Stratum Security Guard: 63 pf IP blocklists, 5-RIR GeoIP, 93 Unbound DNSBL lists, 56 UT1 Web Filter categories, and fail-open Suricata IPS at $0/year."
date: 2026-11-18
translationKey: "product-security-guard"
aliases: ["/security-guard/"]
---

<style>
.container *:has(.sg-wrap), main article:has(.sg-wrap), .prose:has(.sg-wrap), .content:has(.sg-wrap), .markdown:has(.sg-wrap), .single-content:has(.sg-wrap), .article-content:has(.sg-wrap), .page-content:has(.sg-wrap), .post-content:has(.sg-wrap) { max-width:100% !important; width:100% !important; margin-left:0 !important; margin-right:0 !important; }
.sg-wrap { width:100% !important; max-width:1240px !important; margin:1.5rem auto 3rem auto !important; display:flex !important; flex-direction:column !important; gap:2.5rem !important; }
.sg-dark { width:100% !important; box-sizing:border-box !important; background:#08101f !important; border:1px solid #1e293b !important; border-radius:20px !important; padding:2.5rem !important; box-shadow:0 24px 52px -12px rgba(8,16,31,0.48) !important; }
.sg-eyebrow { display:inline-block !important; font-size:0.75rem !important; font-weight:700 !important; letter-spacing:0.09em !important; text-transform:uppercase !important; color:#fbbf24 !important; background:rgba(245,158,11,0.12) !important; border:1px solid rgba(245,158,11,0.32) !important; padding:0.3rem 0.78rem !important; border-radius:999px !important; margin-bottom:0.8rem !important; }
.sg-dark h2 { color:#f8fafc !important; font-size:clamp(1.6rem,2.4vw,2.05rem) !important; line-height:1.22 !important; margin:0 0 0.7rem 0 !important; font-weight:800 !important; max-width:none !important; }
.sg-lead { color:#cbd5e1 !important; font-size:1rem !important; line-height:1.65 !important; margin:0 0 1.65rem 0 !important; max-width:1040px !important; }
.sg-kpis { display:grid !important; grid-template-columns:repeat(4,minmax(0,1fr)) !important; gap:1.15rem !important; margin-bottom:1.75rem !important; }
.sg-kpi { background:#0f172a !important; border:1px solid #1e293b !important; border-radius:14px !important; padding:1.15rem 1.3rem !important; }
.sg-kpi strong { display:block !important; font-size:1.65rem !important; font-weight:800 !important; color:#f8fafc !important; line-height:1.15 !important; white-space:nowrap !important; }
.sg-kpi span { display:block !important; font-size:0.85rem !important; color:#94a3b8 !important; margin-top:0.35rem !important; line-height:1.4 !important; }
.sg-fig { width:100% !important; max-width:100% !important; margin:0 !important; border-radius:16px !important; overflow:hidden !important; border:1px solid #1e293b !important; background:#050b16 !important; }
.sg-fig img { display:block !important; width:100% !important; max-width:100% !important; height:auto !important; margin:0 !important; cursor:zoom-in !important; }
.sg-fig figcaption { padding:0.8rem 1.2rem !important; font-size:0.86rem !important; color:#94a3b8 !important; text-align:center !important; border-top:1px solid #1e293b !important; max-width:none !important; }
.sg-layers { display:grid !important; grid-template-columns:repeat(2,minmax(0,1fr)) !important; gap:1.35rem !important; }
.sg-card { background:#0f172a !important; border:1px solid #1e293b !important; border-radius:14px !important; padding:1.55rem 1.65rem !important; }
.sg-badge { display:inline-block !important; font-size:0.72rem !important; font-weight:700 !important; letter-spacing:0.07em !important; text-transform:uppercase !important; color:#38bdf8 !important; margin-bottom:0.45rem !important; }
.sg-card h3 { color:#f8fafc !important; font-size:1.18rem !important; font-weight:700 !important; margin:0 0 0.75rem 0 !important; }
.sg-card ul { margin:0 !important; padding-left:1.2rem !important; display:flex !important; flex-direction:column !important; gap:0.5rem !important; max-width:none !important; }
.sg-card li { color:#cbd5e1 !important; font-size:0.9rem !important; line-height:1.58 !important; margin:0 !important; max-width:none !important; }
.sg-card li strong { color:#f8fafc !important; font-weight:700 !important; }
.sg-card code { background:#1e293b !important; color:#7dd3fc !important; padding:0.12rem 0.4rem !important; border-radius:4px !important; font-size:0.82rem !important; }
.sg-grid4 { display:grid !important; grid-template-columns:repeat(4,minmax(0,1fr)) !important; gap:1.15rem !important; margin-top:1.5rem !important; }
.sg-why-item { background:#0f172a !important; border:1px solid #1e293b !important; border-top:3px solid #f59e0b !important; border-radius:12px !important; padding:1.2rem 1.25rem !important; }
.sg-why-item strong { display:block !important; color:#f8fafc !important; font-size:0.98rem !important; margin-bottom:0.4rem !important; }
.sg-why-item p { color:#cbd5e1 !important; font-size:0.87rem !important; line-height:1.55 !important; margin:0 !important; max-width:none !important; }
.sg-grid3 { display:grid !important; grid-template-columns:repeat(3,minmax(0,1fr)) !important; gap:1.25rem !important; margin-top:1.5rem !important; }
.sg-cta-row { display:flex !important; flex-wrap:wrap !important; gap:0.85rem !important; margin-top:1.6rem !important; }
.sg-btn-pri { display:inline-flex !important; align-items:center !important; padding:0.72rem 1.35rem !important; border-radius:9px !important; background:#f59e0b !important; color:#0f172a !important; font-weight:700 !important; font-size:0.9rem !important; text-decoration:none !important; }
.sg-btn-sec { display:inline-flex !important; align-items:center !important; padding:0.72rem 1.3rem !important; border-radius:9px !important; background:#1e293b !important; color:#f8fafc !important; border:1px solid #334155 !important; font-weight:600 !important; font-size:0.9rem !important; text-decoration:none !important; }
.sg-lightbox { position:fixed; inset:0; background:rgba(3,7,18,0.92); z-index:9999; display:none; align-items:center; justify-content:center; padding:2rem; cursor:zoom-out; }
.sg-lightbox.open { display:flex; }
.sg-lightbox img { max-width:95vw; max-height:92vh; border-radius:12px; border:1px solid #334155; box-shadow:0 25px 60px rgba(0,0,0,0.7); }
@media (max-width:1024px) {
.sg-kpis, .sg-grid4 { grid-template-columns:repeat(2,minmax(0,1fr)) !important; }
.sg-layers, .sg-grid3 { grid-template-columns:1fr !important; }
}
@media (max-width:640px) {
.sg-kpis, .sg-grid4 { grid-template-columns:1fr !important; }
.sg-dark { padding:1.4rem !important; }
}
</style>
<div class="sg-wrap">
<section class="sg-dark">
<span class="sg-eyebrow">MULTI-LAYER THREAT DEFENSE • $0/YEAR SUBSCRIPTION</span>
<h2>Layered Protection That Never Expires</h2>
<p class="sg-lead">Stratum Firewall protects your network in five coordinated layers. Each stage stops a distinct class of threat before it reaches downstream services, every layer is managed from the same panel, and all of them are included out of the box with no annual subscription, no per-feature license key, and no paid security bundle.</p>
<div class="sg-kpis">
<div class="sg-kpi"><strong>63 Feeds</strong><span>Bidirectional IP Blocklists in Kernel pf</span></div>
<div class="sg-kpi"><strong>5 RIRs</strong><span>Direct Weekly Country GeoIP Rules</span></div>
<div class="sg-kpi"><strong>93 + 56</strong><span>Unbound DNSBL Feeds & UT1 Web Categories</span></div>
<div class="sg-kpi"><strong>52 Sets</strong><span>Suricata ET Open IPS • Fail-Open netmap</span></div>
</div>
<figure class="sg-fig">
<img src="/images/security-guard-layers.jpg" alt="Stratum Security Guard 5-Layer Defense in Depth Funnel" loading="eager" width="1280" height="720">
<figcaption>Each layer filters what it knows best: packets dropped by kernel pf IP tables never consume DNS or deep-packet inspection CPU cycles downstream. (Click any diagram to inspect full size.)</figcaption>
</figure>
</section>
<section class="sg-dark">
<span class="sg-eyebrow">DEFENSE ARCHITECTURE • FIVE ACTIVE SHIELDS</span>
<h2>How Each Security Guard Layer Works</h2>
<p class="sg-lead">From wire-speed kernel packet drops to agentless IoT DNS filtering and inline Suricata payload inspection, every engine updates automatically from public threat-intelligence feeds while keeping your allow-lists authoritative.</p>
<div class="sg-layers">
<div class="sg-card">
<span class="sg-badge">LAYER 01 • FIREWALL → BLOCKLISTS</span>
<h3>IP Blocklists (63 Ready-Made Feeds)</h3>
<ul>
<li><strong>Stops known bad hosts at the perimeter:</strong> Drops botnet C2 servers, scanners, brute-force attackers, spam sources, and compromised hosts before they touch internal services.</li>
<li><strong>Curated catalogue + custom URLs:</strong> 63 feeds in one catalogue (<code>abuse.ch Feodo Tracker</code>, <code>Spamhaus DROP</code>, <code>CINS Army</code>, <code>Emerging Threats</code>, <code>blocklist.de</code>, <code>SANS ISC</code>, <code>GreenSnow</code>, and more) plus your own URL lists.</li>
<li><strong>Bidirectional protection:</strong> Inbound WAN attackers are dropped immediately, and any internal host trying to reach a listed C2 address is stopped on LAN/VLANs, catching infected devices early.</li>
<li><strong>Strict-policy extras:</strong> Optional lists for Tor exit nodes, open proxies, VPN services, DNS-over-HTTPS (DoH) servers, and torrent trackers.</li>
<li><strong>Hitless & fail-safe:</strong> Loaded into kernel <code>pf</code> tables without restarts; allow-lists always win, RFC1918 private ranges are never blocked, and a failed download keeps the previous good list active.</li>
</ul>
</div>
<div class="sg-card">
<span class="sg-badge">LAYER 02 • DIRECT 5-RIR DATA</span>
<h3>GeoIP Country Rules</h3>
<ul>
<li><strong>Country-aware firewall policies:</strong> Restrict sensitive services or VPN endpoints strictly to allowed countries (such as Türkiye only), or block entire regions that have no business reaching your network.</li>
<li><strong>Zero accounts or API keys:</strong> Country prefix allocations are fetched directly from the five Regional Internet Registries (<code>ARIN</code>, <code>RIPE NCC</code>, <code>APNIC</code>, <code>LACNIC</code>, <code>AFRINIC</code>) and updated weekly on schedule.</li>
</ul>
<span class="sg-badge" style="margin-top:1.15rem;">LAYER 03 • BUILT-IN UNBOUND RPZ</span>
<h3>DNS Blocklists (93 Feeds)</h3>
<ul>
<li><strong>Agentless protection for every device:</strong> Stops malware, phishing, tracking, ads, and crypto-mining domains across phones, Smart TVs, and IoT devices with nothing to install on endpoints.</li>
<li><strong>93 lists + subdomain coverage:</strong> Includes <code>EasyList</code> & regional lists, <code>AdGuard</code>, <code>StevenBlack</code>, <code>HaGeZi</code>, <code>OpenPhish</code>, <code>PhishTank</code>, and <code>NoCoin</code>; blocking a domain blocks all its subdomains while allow-lists always take priority.</li>
<li><strong>Zero extra hop:</strong> Runs directly inside Stratum's local <code>Unbound</code> resolver with no external proxy server required.</li>
</ul>
</div>
<div class="sg-card">
<span class="sg-badge">LAYER 04 • PER-NETWORK POLICY</span>
<h3>Web Filter (56 UT1 Categories & Force DNS)</h3>
<ul>
<li><strong>Different policies per VLAN:</strong> Enforce strict category rules on Guest Wi-Fi, balanced workplace policies for Staff, and zero restrictions on Server VLANs.</li>
<li><strong>56 daily-updated categories:</strong> Powered by the <code>UT1</code> lists from Université Toulouse Capitole (adult, gambling, malware, phishing, social networks, streaming, games, VPN/proxy bypass, and more).</li>
<li><strong>Safe Search & Force DNS:</strong> Enforces restricted Safe Search on Google, Bing, DuckDuckGo, and YouTube; redirects hardcoded client DNS (<code>8.8.8.8</code>) back to the firewall resolver and blocks DNS-over-TLS (<code>853</code>) bypasses while honoring explicit site/AD exceptions.</li>
</ul>
</div>
<div class="sg-card">
<span class="sg-badge">LAYER 05 • DEEP PACKET INSPECTION</span>
<h3>Intrusion Detection & Prevention (Suricata)</h3>
<ul>
<li><strong>52 Emerging Threats Open categories:</strong> Inspects live traffic for exploits, malware delivery, and C2 beacons with scheduled automatic rule updates.</li>
<li><strong>Watch or Block per interface:</strong> Start in <strong>Watch</strong> mode to learn normal baseline traffic on your network, then enable inline <code>netmap</code> <strong>Block</strong> only where active dropping adds value.</li>
<li><strong>Fail-open continuity & 1-click tuning:</strong> If Suricata stops, traffic continues flowing (fail-open) instead of taking the network down; noisy individual signatures can be disabled in one click and stay disabled across rule updates.</li>
</ul>
</div>
</div>
</section>
<section class="sg-dark">
<span class="sg-eyebrow">ZERO VENDOR TAX • WHY IT IS FREE</span>
<h2>Why Security Guard Costs $0/Year — And Why Engineers Switch</h2>
<p class="sg-lead">Commercial UTM vendors lock Web Filtering, DNS defense, and IPS updates behind recurring annual contracts that shut off protection the day a license expires. Stratum FW eliminates the middleman by compiling authoritative public threat intelligence directly on your own FreeBSD hardware.</p>
<figure class="sg-fig">
<img src="/images/security-guard-sources.jpg" alt="Why Stratum Security Guard Is Free Forever - Public Threat Intelligence Sources" loading="lazy" width="1200" height="900">
<figcaption>Public & academic threat intelligence feeds compiled locally on-box with fail-safe retention and zero annual subscription lock-in.</figcaption>
</figure>
<div class="sg-grid4">
<div class="sg-why-item"><strong>Included & Free Forever</strong><p>Every protection layer ships inside Stratum FW with no subscription, no per-feature key, and no expiring threat feed.</p></div>
<div class="sg-why-item"><strong>Built Against False Alarms</strong><p>Watch-before-Block IPS, 1-click signature suppression, allow-lists that always win, and last-good-list retention protect uptime.</p></div>
<div class="sg-why-item"><strong>One Unified Panel</strong><p>No separate plugins or extra VMs to babysit: one configuration, one encrypted backup, one RBAC model, and one alerting pipeline.</p></div>
<div class="sg-why-item"><strong>Proven in Production</strong><p>Running continuously in production hospitality networks with segmented VLANs, IPTV streams, guest Wi-Fi hotspots, and staff networks.</p></div>
</div>
</section>
<section class="sg-dark">
<span class="sg-eyebrow">VISIBILITY, ALERTS & SELF-DEFENSE</span>
<h2>See Every Blocked Threat — On an Appliance That Protects Itself</h2>
<p class="sg-lead">Blocking threats silently is only half the job. Security Guard gives administrators clear evidence of what was stopped, 30-day east-west VLAN visibility, instant Telegram/email alerts, and hardened self-protection for the firewall itself.</p>
<figure class="sg-fig">
<img src="/images/security-guard-visibility.jpg" alt="Stratum Security Guard Blocked Report, 30-Day VLAN Matrix and Telegram Alerts" loading="lazy" width="1200" height="900">
<figcaption>Unified Blocked Report, 30-day Inter-VLAN flow matrix, and real-time Telegram & email notifications.</figcaption>
</figure>
<div class="sg-grid3">
<div class="sg-card">
<span class="sg-badge">EVIDENCE & AUDITING</span>
<h3>See What Was Stopped</h3>
<ul>
<li><strong>Blocked Report:</strong> Shows exactly what IP blocklists, DNSBL, and Web Filter stopped and which feed triggered it.</li>
<li><strong>Between Networks (30 Days):</strong> 30-day subnet matrix to isolate VLANs safely and verify blocked east-west flows afterward.</li>
<li><strong>Audited Web Activity:</strong> Per-device lookups correlated with DHCP history; strictly admin-only with every view logged, alongside packet capture and live firewall logs.</li>
</ul>
</div>
<div class="sg-card">
<span class="sg-badge">INSTANT NOTIFICATIONS</span>
<h3>Telegram & Email Alerts</h3>
<ul>
<li><strong>Security events:</strong> High-severity Suricata IPS alerts and IP addresses blocked after repeated failed sign-ins.</li>
<li><strong>Operational continuity:</strong> WAN link down events, failed blocklist downloads, reverted/failed config changes, and failed backups.</li>
<li><strong>System health:</strong> Available OS updates, disk capacity thresholds, CPU temperature warnings, and TLS certificate expiry.</li>
</ul>
</div>
<div class="sg-card">
<span class="sg-badge">APPLIANCE HARDENING</span>
<h3>Self-Protecting Control Plane</h3>
<ul>
<li><strong>Local TOTP 2FA & Brute-Force Lock:</strong> On-box QR generation plus automatic 5-minute IP block after 5 failed sign-ins in 5 minutes.</li>
<li><strong>Locked-Down Access & Secrets:</strong> WAN panel/SSH closed by default (key-only SSH supported), 3 RBAC roles, and <code>0600</code> root-only secret files never sent to the browser.</li>
<li><strong>Safe-Apply & Signed Updates:</strong> Pre-checked changes with auto-rollback and signed updates backed by ZFS Boot Environments.</li>
</ul>
</div>
</div>
<div class="sg-cta-row">
<a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="sg-btn-pri">Explore Security Guard in Live Demo →</a>
<a href="/download/" class="sg-btn-sec">Download Free v0.34.2 ISO / IMG</a>
<a href="/product/security/" class="sg-btn-sec">Cryptographic & Update Verification</a>
</div>
</section>
</div>
<div class="sg-lightbox" id="sg-lb" onclick="this.classList.remove('open')"><img id="sg-lb-img" src="" alt=""></div>
<script>
document.querySelectorAll('.sg-fig img').forEach(function(img){
img.addEventListener('click',function(){
var lb=document.getElementById('sg-lb'), target=document.getElementById('sg-lb-img');
if(lb&&target){ target.src=img.src; target.alt=img.alt; lb.classList.add('open'); }
});
});
document.addEventListener('keydown',function(e){ if(e.key==='Escape'){ var lb=document.getElementById('sg-lb'); if(lb) lb.classList.remove('open'); }});
</script>
