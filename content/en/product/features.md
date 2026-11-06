---
title: "Features"
description: "Explore Stratum Firewall's core capabilities: Multi-VLAN network topology, native FreeBSD pf kernel packet filtering, DNSBL web filtering, IP blocklists, and FastAPI control."
lead: "Built on raw FreeBSD 14/15 power and managed through an asynchronous Python 3.12 & FastAPI control plane."
---

<div class="feature-showcase-page" style="margin-top: 16px;">

  <div class="feature-row" style="display:flex;align-items:center;gap:48px;margin-bottom:80px;flex-wrap:wrap;">
    <div class="feature-image" style="flex:1.15;min-width:280px;">
      <img src="/images/features/topology-vlan.jpg" alt="Stratum Firewall multi-interface WAN, Office LAN, Guest Wi-Fi, IoT VLAN segmentation, and WireGuard VPN topology diagram" style="width:100%;border-radius:16px;box-shadow:0 20px 40px rgba(15,30,61,.15);" loading="lazy">
    </div>
    <div class="feature-content" style="flex:1;min-width:280px;">
      <span class="feature-badge" style="display:inline-block;padding:6px 14px;background:#fffbeb;color:#d97706;border:1px solid rgba(245,158,11,0.35);font-size:12px;font-weight:700;border-radius:30px;margin-bottom:16px;text-transform:uppercase;letter-spacing:1px;">NETWORK TOPOLOGY &amp; VLAN SEGMENTATION</span>
      <h2 style="margin-top:0;margin-bottom:16px;color:#0f1e3d;">Complete Network Isolation with Multi-VLAN Segmentation</h2>
      <p style="font-size:17px;font-weight:600;color:#2d3748;margin-bottom:16px;line-height:1.55;">Manage physical network interfaces and virtual LANs (VLANs) from a unified control plane to securely isolate office workstations, guest Wi-Fi, staff devices, and IoT traffic.</p>
      <p style="color:#4a5568;margin-bottom:20px;line-height:1.7;">Stratum Firewall provides a flexible multi-interface networking architecture designed to simplify complex network topologies. Administrators can map physical interfaces (<code>pppoe0</code> WAN, <code>igb1</code> LAN, <code>igb2</code> OPT1, <code>igb3</code> OPT2) and carve out logically isolated VLAN segments—such as Office LAN, Guest Wi-Fi, Staff, and Security Cameras—over shared physical infrastructure. With built-in DHCP Server, DNS Resolver, and NTP time synchronization services, each network zone operates with its own dedicated addressing and routing policies.</p>
      <ul>
        <li><strong>Multi-Interface &amp; WAN Mapping:</strong> Configure dedicated WAN, LAN, and optional (<code>OPT1</code>/<code>OPT2</code>) interfaces with native PPPoE and static/dynamic IP support.</li>
        <li><strong>Granular VLAN Segmentation:</strong> Separate untrusted IoT devices and guest wireless clients from core infrastructure to prevent lateral movement.</li>
        <li><strong>Integrated Core Network Services:</strong> Run dedicated DHCP scopes, local DNS resolution, and NTP services seamlessly across every network segment.</li>
      </ul>
    </div>
  </div>

  <div class="feature-row" style="display:flex;flex-direction:row-reverse;align-items:center;gap:48px;margin-bottom:80px;flex-wrap:wrap;">
    <div class="feature-image" style="flex:1.15;min-width:280px;">
      <img src="/images/features/pf-kernel-shield.jpg" alt="Stratum Firewall FreeBSD kernel-level pf packet filtering and multi-layered network defense shield illustration" style="width:100%;border-radius:16px;box-shadow:0 20px 40px rgba(15,30,61,.15);" loading="lazy">
    </div>
    <div class="feature-content" style="flex:1;min-width:280px;">
      <span class="feature-badge" style="display:inline-block;padding:6px 14px;background:#fffbeb;color:#d97706;border:1px solid rgba(245,158,11,0.35);font-size:12px;font-weight:700;border-radius:30px;margin-bottom:16px;text-transform:uppercase;letter-spacing:1px;">KERNEL-LEVEL PF PACKET FILTER</span>
      <h2 style="margin-top:0;margin-bottom:16px;color:#0f1e3d;">Uncompromising Network Shield with Native FreeBSD pf Filtering</h2>
      <p style="font-size:17px;font-weight:600;color:#2d3748;margin-bottom:16px;line-height:1.55;">Powered directly by the FreeBSD 14/15 kernel pf engine, delivering wire-speed stateful packet inspection, ALTQ traffic shaping, and fail-safe rule validation.</p>
      <p style="color:#4a5568;margin-bottom:20px;line-height:1.7;">At the core of Stratum Firewall lies FreeBSD's battle-tested kernel-level <code>pf</code> (Packet Filter) engine. Every network packet is inspected against active connection states at the kernel layer, neutralizing unauthorized access attempts while forwarding legitimate traffic with minimal latency. Integrated Network Address Translation (NAT) and ALTQ bandwidth shaping keep critical applications responsive under heavy load. Before any configuration change goes live, the <strong>Safe-Apply</strong> engine automatically validates rules against <code>pf</code> syntax to prevent accidental lockouts or broken rulesets.</p>
      <ul>
        <li><strong>Native Kernel Stateful Inspection:</strong> High-throughput packet filtering powered directly by FreeBSD 14/15 <code>pf</code> without user-space bottlenecks.</li>
        <li><strong>Safe-Apply Syntax Validation:</strong> Pre-flight verification of all firewall rules and interface updates before committing changes to the active kernel.</li>
        <li><strong>NAT, ALTQ &amp; Real-Time Rule Hits:</strong> Manage reusable Aliases, <code>Pass / Block / Reject</code> policies, live rule hit counters, and ALTQ traffic prioritization in one place.</li>
      </ul>
    </div>
  </div>

  <div class="feature-row" style="display:flex;align-items:center;gap:48px;margin-bottom:80px;flex-wrap:wrap;">
    <div class="feature-image" style="flex:1.15;min-width:280px;">
      <img src="/images/features/dnsbl-web-filter.jpg" alt="Stratum Firewall DNSBL domain sinkholing and threat intelligence IP blocklist filtration layers" style="width:100%;border-radius:16px;box-shadow:0 20px 40px rgba(15,30,61,.15);" loading="lazy">
    </div>
    <div class="feature-content" style="flex:1;min-width:280px;">
      <span class="feature-badge" style="display:inline-block;padding:6px 14px;background:#fffbeb;color:#d97706;border:1px solid rgba(245,158,11,0.35);font-size:12px;font-weight:700;border-radius:30px;margin-bottom:16px;text-transform:uppercase;letter-spacing:1px;">THREAT INTELLIGENCE &amp; DNSBL</span>
      <h2 style="margin-top:0;margin-bottom:16px;color:#0f1e3d;">Proactive Web Filtering (DNSBL) and Automated IP Blocklists</h2>
      <p style="font-size:17px;font-weight:600;color:#2d3748;margin-bottom:16px;line-height:1.55;">Neutralize malicious domains and known attacker IPs before they ever reach your endpoints using DNS-level sinkholing and automated threat intelligence feeds.</p>
      <p style="color:#4a5568;margin-bottom:20px;line-height:1.7;">Stratum Firewall extends packet filtering with multi-layered threat intelligence. The <strong>IP Blocklists</strong> module ingests external threat feeds to drop traffic from known command-and-control servers, scanners, and malicious hosts directly at the packet level. Simultaneously, <strong>DNS-Based Web Filtering (DNSBL)</strong> intercepts requests to phishing, malware, and tracking domains at the DNS resolution layer—returning an immediate <code>NXDOMAIN</code> response across all network devices without requiring client-side agents. An integrated domain lookup utility lets administrators test and verify domain blocking status in real time.</p>
      <ul>
        <li><strong>Network-Wide DNS Sinkholing (DNSBL):</strong> Block malicious, phishing, and unwanted domains at the DNS layer by returning <code>NXDOMAIN</code> responses.</li>
        <li><strong>Automated Threat Feed IP Blocking:</strong> Import curated IP blocklists to automatically drop packets from hostile networks at the perimeter.</li>
        <li><strong>Instant Domain Lookup Diagnostic:</strong> Test whether specific domains are permitted or blocked directly from the web dashboard in real time.</li>
      </ul>
    </div>
  </div>

  <div class="feature-row" style="display:flex;flex-direction:row-reverse;align-items:center;gap:48px;margin-bottom:40px;flex-wrap:wrap;">
    <div class="feature-image" style="flex:1.15;min-width:280px;">
      <img src="/images/features/api-architecture.jpg" alt="Stratum Firewall Python FastAPI REST API control plane, WireGuard X25519 encryption, and live telemetry architecture" style="width:100%;border-radius:16px;box-shadow:0 20px 40px rgba(15,30,61,.15);" loading="lazy">
    </div>
    <div class="feature-content" style="flex:1;min-width:280px;">
      <span class="feature-badge" style="display:inline-block;padding:6px 14px;background:#fffbeb;color:#d97706;border:1px solid rgba(245,158,11,0.35);font-size:12px;font-weight:700;border-radius:30px;margin-bottom:16px;text-transform:uppercase;letter-spacing:1px;">MODERN ARCHITECTURE &amp; API CONTROL</span>
      <h2 style="margin-top:0;margin-bottom:16px;color:#0f1e3d;">Zero-Dependency Cybersecurity Architecture with Modern API Control</h2>
      <p style="font-size:17px;font-weight:600;color:#2d3748;margin-bottom:16px;line-height:1.55;">"Raw FreeBSD power. Modern API control." Experience a lightweight Python 3.12 &amp; FastAPI backend, declarative config.json state management, and native WireGuard cryptography.</p>
      <p style="color:#4a5568;margin-bottom:20px;line-height:1.7;">Engineered to replace bloated legacy firewall appliances, Stratum Firewall pairs raw FreeBSD kernel performance with a clean, developer-first control plane. Its asynchronous Python 3.12 and FastAPI backend delivers both a responsive web UI and a full-featured REST API with a minimal memory footprint. The entire appliance state resides in a single declarative <code>config.json</code> file, which the core engine dynamically compiles into native <code>pf.conf</code>, DHCP, and DNS configurations. Complete with zero-dependency native X25519 (RFC 7748) WireGuard VPN and OpenVPN support, Role-Based Access Control (RBAC), and real-time hardware and firewall log telemetry, it brings modern infrastructure-as-code simplicity to network security.</p>
      <ul>
        <li><strong>FastAPI &amp; Declarative config.json Engine:</strong> Manage the entire firewall state through a single JSON source of truth and automate workflows via a complete REST API.</li>
        <li><strong>Zero-Dependency WireGuard &amp; OpenVPN:</strong> Built-in RFC 7748 X25519 cryptographic key generation for WireGuard alongside flexible OpenVPN tunneling.</li>
        <li><strong>RBAC &amp; Real-Time System Telemetry:</strong> Granular Administrator, Operator, and Read-Only roles paired with live CPU, memory, disk, thermal, traffic, and firewall log dashboards.</li>
      </ul>
    </div>
  </div>

</div>
