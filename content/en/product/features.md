---
title: "Features"
description: "Explore Stratum Firewall's core capabilities: Multi-VLAN network topology, native FreeBSD pf kernel packet filtering, DNSBL web filtering, IP blocklists, and FastAPI control."
lead: "Everything you need to segment, shield, filter, and automate your network infrastructure—built on raw FreeBSD 14/15 power and controlled via a modern FastAPI architecture."
full_width: true
---
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
<div class="feature-module" id="network-topology">
<div class="feature-module-head">
<span class="feature-badge-pill">01 · NETWORK TOPOLOGY &amp; VLAN SEGMENTATION</span>
<h2>Complete Network Isolation with Multi-VLAN Segmentation</h2>
<p class="feature-module-lead">Manage physical network interfaces and virtual LANs (VLANs) from a unified control plane to securely isolate office workstations, guest Wi-Fi, staff devices, and IoT traffic.</p>
<p class="feature-module-desc">Stratum Firewall provides a flexible multi-interface networking architecture designed to simplify complex enterprise and homelab topologies. Administrators can map physical network cards (such as <code>pppoe0</code> WAN, <code>igb1</code> LAN, <code>igb2</code> OPT1, and <code>igb3</code> OPT2) and carve out logically isolated 802.1Q VLAN segments—including Office LAN, Guest Wi-Fi, Staff, and Security Cameras—over shared physical cabling. With integrated DHCP Server, DNS Resolver, and NTP time synchronization services, every network zone operates with its own dedicated IP addressing, subnet boundaries, and inter-VLAN firewall policies.</p>
</div>
<div class="feature-visual-box">
<img src="/images/features/topology-vlan.jpg" alt="Stratum Firewall multi-interface WAN, Office LAN, Guest Wi-Fi, IoT VLAN segmentation, and WireGuard VPN topology diagram" loading="lazy">
</div>
<div class="card-grid grid-3">
<div class="card">
<h3>Multi-Interface &amp; WAN Mapping</h3>
<p>Configure dedicated WAN, LAN, and optional (<code>OPT1</code>/<code>OPT2</code>) physical interfaces with native PPPoE, static IPv4/IPv6, and dynamic DHCP client support.</p>
</div>
<div class="card">
<h3>Granular VLAN Segmentation</h3>
<p>Isolate untrusted IoT devices, IP cameras, and guest wireless clients into separate broadcast domains to eliminate lateral threat movement across your network.</p>
</div>
<div class="card">
<h3>Integrated Core Network Services</h3>
<p>Run independent DHCP address pools, static MAC leases, local DNS resolution, and NTP time synchronization seamlessly across every interface and VLAN.</p>
</div>
</div>
</div>
<div class="feature-module" id="pf-kernel-shield">
<div class="feature-module-head">
<span class="feature-badge-pill">02 · KERNEL-LEVEL PF PACKET FILTER</span>
<h2>Uncompromising Network Shield with Native FreeBSD pf Filtering</h2>
<p class="feature-module-lead">Powered directly by the FreeBSD 14/15 kernel pf engine, delivering wire-speed stateful packet inspection, ALTQ traffic shaping, and fail-safe rule validation.</p>
<p class="feature-module-desc">At the core of Stratum Firewall lies FreeBSD's battle-tested kernel-level <code>pf</code> (Packet Filter) engine. Every incoming and outgoing packet is evaluated against active state tables directly inside the operating system kernel, neutralizing unauthorized connection attempts while forwarding legitimate traffic at wire speed with minimal CPU overhead. Advanced Network Address Translation (Outbound &amp; Port Forward NAT) and ALTQ bandwidth shaping keep latency-sensitive applications responsive even under heavy load. Before any rule or interface modification is committed, our <strong>Safe-Apply</strong> workflow validates the generated syntax against <code>pfctl -nf</code> to guarantee you never lock yourself out.</p>
</div>
<div class="feature-visual-box">
<img src="/images/features/pf-kernel-shield.jpg" alt="Stratum Firewall FreeBSD kernel-level pf packet filtering and multi-layered network defense shield illustration" loading="lazy">
</div>
<div class="card-grid grid-3">
<div class="card">
<h3>Native Kernel Stateful Inspection</h3>
<p>High-throughput stateful packet filtering executed directly in the FreeBSD 14/15 kernel with zero user-space packet copying bottlenecks.</p>
</div>
<div class="card">
<h3>Safe-Apply Syntax Validation</h3>
<p>Automatic pre-flight verification of all firewall rulesets and interface assignments before applying changes to the active kernel state.</p>
</div>
<div class="card">
<h3>NAT, ALTQ &amp; Live Rule Telemetry</h3>
<p>Manage reusable IP/Port Aliases, granular <code>Pass / Block / Reject</code> rules, real-time rule hit counters, and ALTQ traffic queues from a single view.</p>
</div>
</div>
</div>
<div class="feature-module" id="dnsbl-web-filter">
<div class="feature-module-head">
<span class="feature-badge-pill">03 · THREAT INTELLIGENCE &amp; DNSBL</span>
<h2>Proactive Web Filtering (DNSBL) and Automated IP Blocklists</h2>
<p class="feature-module-lead">Neutralize malicious domains and known attacker IPs before they ever reach your endpoints using DNS-level sinkholing and automated threat intelligence feeds.</p>
<p class="feature-module-desc">Stratum Firewall reinforces traditional port and protocol rules with dual-layer threat intelligence. The <strong>IP Blocklists</strong> module automatically fetches and parses public or custom threat intelligence feeds, loading known botnet command-and-control servers, scanners, and abusive IP ranges into high-speed kernel <code>pf</code> tables to drop hostile packets on sight. Simultaneously, the <strong>DNS-Based Web Filtering (DNSBL)</strong> engine intercepts outbound DNS queries for phishing, malware, telemetry, and advertising domains—returning an immediate <code>NXDOMAIN</code> response across every device on your network without installing client-side software.</p>
</div>
<div class="feature-visual-box">
<img src="/images/features/dnsbl-web-filter.jpg" alt="Stratum Firewall DNSBL domain sinkholing and threat intelligence IP blocklist filtration layers" loading="lazy">
</div>
<div class="card-grid grid-3">
<div class="card">
<h3>Network-Wide DNS Sinkholing</h3>
<p>Block malware, phishing, tracking, and ad domains at the DNS resolution layer with instant <code>NXDOMAIN</code> responses for all connected clients.</p>
</div>
<div class="card">
<h3>Automated Threat Feed IP Blocking</h3>
<p>Ingest external IP reputation feeds into kernel-level <code>pf</code> tables to drop traffic from hostile networks at the perimeter with zero latency penalty.</p>
</div>
<div class="card">
<h3>Real-Time Domain Lookup Tool</h3>
<p>Instantly query and verify whether any specific domain name is blocked or permitted by your active DNSBL policies directly from the management UI.</p>
</div>
</div>
</div>
<div class="feature-module" id="modern-api-architecture">
<div class="feature-module-head">
<span class="feature-badge-pill">04 · MODERN ARCHITECTURE &amp; API CONTROL</span>
<h2>Zero-Dependency Cybersecurity Architecture with Modern API Control</h2>
<p class="feature-module-lead">"Raw FreeBSD power. Modern API control." Experience a lightweight Python 3.12 &amp; FastAPI backend, declarative config.json state management, and native WireGuard cryptography.</p>
<p class="feature-module-desc">Engineered from the ground up to replace bloated legacy PHP/XML firewall interfaces, Stratum Firewall pairs raw FreeBSD kernel performance with a clean, developer-first control plane. Its asynchronous <strong>Python 3.12</strong> and <strong>FastAPI</strong> backend serves both a responsive web UI and a comprehensive <strong>REST API</strong> while maintaining an ultra-low memory footprint. The entire system state lives in a single human-readable, declarative <code>config.json</code> file that dynamically compiles into native <code>pf.conf</code>, DHCP, and DNS files. Combined with zero-dependency X25519 (RFC 7748) <strong>WireGuard VPN</strong> and <strong>OpenVPN</strong> support, Role-Based Access Control (RBAC), and live hardware/log telemetry, Stratum brings true Infrastructure-as-Code simplicity to network security.</p>
</div>
<div class="feature-visual-box">
<img src="/images/features/api-architecture.jpg" alt="Stratum Firewall Python FastAPI REST API control plane, WireGuard X25519 encryption, and live telemetry architecture" loading="lazy">
</div>
<div class="card-grid grid-3">
<div class="card">
<h3>FastAPI &amp; Declarative config.json</h3>
<p>Control the entire appliance state through a single auditable JSON source of truth and automate every firewall operation via an asynchronous REST API.</p>
</div>
<div class="card">
<h3>Zero-Dependency WireGuard &amp; OpenVPN</h3>
<p>Native RFC 7748 X25519 cryptographic key generation written from scratch for high-speed WireGuard tunnels alongside full OpenVPN support.</p>
</div>
<div class="card">
<h3>RBAC &amp; Live System Telemetry</h3>
<p>Enforce Administrator, Operator, and Read-Only access roles while monitoring real-time CPU, memory, disk, thermal, traffic, and live firewall logs.</p>
</div>
</div>
</div>
</div>
