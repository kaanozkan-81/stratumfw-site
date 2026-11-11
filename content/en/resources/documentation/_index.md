---
title: "Documentation"
description: "Official Stratum Firewall v0.30.0 documentation — VGA ISO & Serial IMG installation, OPNsense migration, TOTP 2FA, Force DNS, 1:1 NAT, rule schedules, DHCP Multi-WAN, Suricata IPS, WireGuard, OpenVPN, and REST API."
lead: "Everything you need to install, migrate, configure, and operate Stratum Firewall v0.30.0 on FreeBSD 15.1."
full_width: true
hide_pages: true
---

<div class="docs-layout">

<div class="docs-intro">
  <p>Stratum Firewall ships as a turnkey FreeBSD 15.1 appliance OS with ZFS Boot Environments, TOTP two-factor sign-in, a declarative configuration engine, and a 60-second Safe-Apply rollback workflow. Grab the <a href="/download/">VGA ISO or Serial IMG installer</a>, or explore the interface first in our <a href="https://demo.stratumfw.com" target="_blank" rel="noopener">Live Browser Demo</a>.</p>
</div>

<section id="getting-started" class="docs-section">
  <div class="docs-section-head">
    <span class="eyebrow">01</span>
    <h2>Getting Started &amp; Migration</h2>
    <p>Install from ISO/IMG, harden admin sign-in with TOTP 2FA, or migrate an existing OPNsense appliance.</p>
  </div>
  <div class="card-grid grid-3">
    <div class="card">
      <h3>Installation (.iso &amp; .img)</h3>
      <p>Verify SHA-256 hashes, flash VGA ISO or Serial USB IMG media, and install onto ZFS.</p>
      <a href="/resources/documentation/installation/" class="card-link">Read guide →</a>
    </div>
    <div class="card">
      <h3>Initial Configuration &amp; 2FA</h3>
      <p>Assign WAN/LAN interfaces, enable TOTP 2FA with recovery codes, configure DNS/DHCP, and run signed updates.</p>
      <a href="/resources/documentation/initial-configuration/" class="card-link">Read guide →</a>
    </div>
    <div class="card">
      <h3>Migrating from OPNsense</h3>
      <p>Import OPNsense <code>config.xml</code> rules, DHCP Multi-WAN, Force DNS, URL-table feeds, and OpenVPN PKI with zero client changes.</p>
      <a href="/resources/documentation/opnsense-migration/" class="card-link">Read guide →</a>
    </div>
  </div>
</section>

<section id="networking" class="docs-section">
  <div class="docs-section-head">
    <span class="eyebrow">02</span>
    <h2>Panel, Firewall &amp; Routing</h2>
    <p>Operate the customizable web panel, schedule stateful pf rules, map 1:1 NAT, and steer Multi-WAN traffic.</p>
  </div>
  <div class="card-grid grid-3">
    <div class="card">
      <h3>Panel Tour &amp; Operations</h3>
      <p>Customize the 12-column dashboard, manage services, enforce TOTP 2FA &amp; RBAC, and schedule encrypted SFTP backups.</p>
      <a href="/resources/documentation/panel-tour/" class="card-link">Read guide →</a>
    </div>
    <div class="card">
      <h3>pf Rules, Schedules, 1:1 NAT &amp; Aliases</h3>
      <p>Configure time-based rule schedules, bidirectional 1:1 NAT, port forwards, GeoIP/Hostname aliases, and dummynet shaping.</p>
      <a href="/resources/documentation/pf-rules/" class="card-link">Read guide →</a>
    </div>
    <div class="card">
      <h3>Multi-WAN (DHCP/PPPoE), Policy Routing &amp; VIPs</h3>
      <p>Set up 1-second gateway monitoring, dynamic DHCP secondary WANs, automatic failover, Virtual IPs, and static routes.</p>
      <a href="/resources/documentation/multi-wan/" class="card-link">Read guide →</a>
    </div>
  </div>
</section>

<section id="security-vpn" class="docs-section">
  <div class="docs-section-head">
    <span class="eyebrow">03</span>
    <h2>Threat Protection &amp; VPN</h2>
    <p>Enforce Force DNS, block threats across IP, DNS, and payload layers, and connect remote users securely.</p>
  </div>
  <div class="card-grid grid-3">
    <div class="card">
      <h3>Blocklists, Force DNS, Web Filter &amp; IPS</h3>
      <p>Enable 63 IP feeds, Force DNS with AD exceptions, Unbound DNSBL, 55-category Web Filter, Suricata IPS, and block reports.</p>
      <a href="/resources/documentation/blocklists/" class="card-link">Read guide →</a>
    </div>
    <div class="card">
      <h3>VPN Overview (WireGuard &amp; OpenVPN)</h3>
      <p>Compare WireGuard and OpenVPN, configure multi-WAN OpenVPN access, and manage VPN-only accounts.</p>
      <a href="/resources/documentation/vpn/" class="card-link">Read guide →</a>
    </div>
    <div class="card">
      <h3>WireGuard VPN Setup</h3>
      <p>Deploy kernel-native <code>if_wg</code> tunnels with on-box key generation, QR codes, and per-VLAN access control.</p>
      <a href="/resources/documentation/wireguard-vpn/" class="card-link">Read guide →</a>
    </div>
  </div>
</section>

<section id="reference" class="docs-section">
  <div class="docs-section-head">
    <span class="eyebrow">04</span>
    <h2>Automation, Diagnostics &amp; Reference</h2>
    <p>Automate via REST API, inspect 1h/24h Firewall Log Summaries, capture packets, and audit system paths.</p>
  </div>
  <div class="card-grid grid-3">
    <div class="card">
      <h3>REST API, MCP &amp; SNMP</h3>
      <p>Automate firewall workflows via FastAPI, query live state with AI over MCP, and monitor via SNMP.</p>
      <a href="/resources/documentation/rest-api/" class="card-link">Read guide →</a>
    </div>
    <div class="card">
      <h3>Diagnostics, Log Summary &amp; CLI</h3>
      <p>Inspect 24h Firewall Log Summary charts, Wireshark <code>.pcap</code> capture, ARP/Wake-on-LAN, and Stratum CLI tools.</p>
      <a href="/resources/documentation/command-line/" class="card-link">Read guide →</a>
    </div>
    <div class="card">
      <h3>File &amp; Path Reference</h3>
      <p>Understand <code>config.json</code>, isolated 2FA/VPN secret storage, generated configs, logs, and ZFS boot environments.</p>
      <a href="/resources/documentation/file-paths/" class="card-link">Read guide →</a>
    </div>
  </div>
</section>

<section class="docs-contact">
  <h3>Need help with your deployment?</h3>
  <p>Explore the <a href="/resources/faq/">FAQ</a>, check the <a href="/resources/changelog/">Changelog</a>, or reach out directly to our engineering team.</p>
  <a href="/company/contact/" class="btn btn-primary">Contact Us</a>
</section>

</div>
