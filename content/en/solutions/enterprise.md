---
title: "Enterprise & Multi-Site"
description: "API-first FreeBSD 15.1 firewall OS for enterprise infrastructure teams — drop-in OPNsense migration, /29 Virtual IPs, dual-engine Suricata IPS, RBAC, SNMP, Remote Syslog, and ZFS rollback."
lead: "API-driven automation, deterministic declarative state, and wire-speed FreeBSD kernel security built for multi-site deployments, data centers, and regulated environments."
full_width: true
---
<div class="solutions-layout">

<div class="sol-block">
  <div class="sol-head">
    <span class="eyebrow">Enterprise Infrastructure Architecture</span>
    <h2>Engineered for automation, auditability, and zero-downtime maintenance</h2>
    <p>At enterprise scale, packet filtering is table stakes. Infrastructure teams require pre-validated change control, least-privilege RBAC, SIEM/NMS observability, compliance-ready audit trails (ISO 27001 / SOC 2), and painless migration from legacy appliances.</p>
  </div>

  <div class="card-grid grid-3">
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="4" y="4" width="16" height="16" rx="2"/><rect x="9" y="9" width="6" height="6"/><path d="M9 1v3M15 1v3M9 20v3M15 20v3M20 9h3M20 14h3M1 9h3M1 14h3"/></svg></div>
      <h3>API-First &amp; Declarative Single Source</h3>
      <p>Every subsystem is rendered deterministically from a single auditable <code>config.json</code> via an asynchronous Python 3.12 / FastAPI control plane—ideal for Ansible, Terraform, and GitOps configuration pipelines.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M16 3h5v5M4 20L21 3M21 16v5h-5M15 15l6 6M4 4l5 5"/></svg></div>
      <h3>Drop-In OPNsense &amp; OpenVPN Cutover</h3>
      <p>Import OPNsense <code>config.xml</code> files with interactive port mapping and pre-flight validation. Preserve your CA, certificates, <code>tls-crypt</code> keys, and <code>$2y$</code>/<code>argon2id</code> password hashes so hundreds of remote OpenVPN endpoints stay connected with zero touch.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><circle cx="6" cy="19" r="3"/><path d="M9 19h8.5a3.5 3.5 0 0 0 0-7h-11a3.5 3.5 0 0 1 0-7H15"/><circle cx="18" cy="5" r="3"/></svg></div>
      <h3>Routed VIP Blocks (<code>/29</code>), Static Routes &amp; Policy Routing</h3>
      <p>Bind multiple public IPv4 addresses via Virtual IPs with NAT reflection, route internal subnets behind core L3 switches cleanly via <code>stratum-netextra</code>, and steer traffic across redundant ISPs per rule.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/><path d="m9 12 2 2 4-4"/></svg></div>
      <h3>Dual-Engine Suricata 8 IPS &amp; 5-RIR GeoIP</h3>
      <p>Mix inline <code>netmap</code> IPS blocking and <code>pcap</code> IDS watching per interface with zero-drop <code>SIGUSR2</code> live rule reloads, backed by license-free Country (GeoIP) and CDN-aware Hostname tables.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M22 21v-2a4 4 0 0 0-3-3.87M16 3.13a4 4 0 0 1 0 7.75"/></svg></div>
      <h3>4-Tier RBAC, SNMP &amp; Remote Syslog</h3>
      <p>Enforce least privilege across <strong>Administrator</strong>, <strong>Operator</strong>, <strong>Read-Only</strong>, and <strong>VPN-only</strong> accounts. Poll 64-bit counters via native read-only <code>bsnmpd</code> (Zabbix, LibreNMS, PRTG) and stream parsed logs to remote SIEM/Syslog collectors.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M3 12a9 9 0 0 1 15-6.7L21 8"/><path d="M21 3v5h-5"/><path d="M21 12a9 9 0 0 1-15 6.7L3 16"/><path d="M3 21v-5h5"/></svg></div>
      <h3>Atomic ZFS Rollback, Air-Gapped <code>.sfu</code> &amp; MCP</h3>
      <p>Eliminate maintenance-window lockouts with <code>pfctl -n</code> pre-checks, 60-second Safe-Apply auto-rollback, bootable ZFS snapshots before every online or air-gapped <code>.sfu</code> upgrade, and read-only MCP AI diagnostics.</p>
    </div>
  </div>
</div>

<div class="sol-block">
  <div class="sol-head">
    <span class="eyebrow">Architectural Comparison</span>
    <h2>How Stratum FW compares to legacy firewall stacks</h2>
  </div>

  <div class="sol-table-wrap">
    <table class="sol-table">
      <thead>
        <tr>
          <th>Operational Dimension</th>
          <th>Traditional Firewall Stacks</th>
          <th>Stratum Firewall (FreeBSD 15.1)</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>Configuration Integrity</strong></td>
          <td>Sprawling XML files, plugin drift, and slow PHP reloads</td>
          <td>Single declarative <code>config.json</code> compiling clean native FreeBSD configs</td>
        </tr>
        <tr>
          <td><strong>Change &amp; Upgrade Safety</strong></td>
          <td>Remote branch lockouts requiring truck rolls or KVM-over-IP rescue</td>
          <td><code>pfctl -n</code> syntax pre-check + 60s Safe-Apply rollback + ZFS Boot Environments</td>
        </tr>
        <tr>
          <td><strong>Data Sovereignty &amp; Air-Gap</strong></td>
          <td>Mandatory cloud entitlement checks and vendor telemetry</td>
          <td>100% self-hosted, zero telemetry, signed offline <code>.sfu</code> updates</td>
        </tr>
        <tr>
          <td><strong>Disaster Recovery (DR)</strong></td>
          <td>Manual certificate/key re-imports when replacing hardware</td>
          <td>Scheduled <code>AES-256-GCM</code> encrypted SFTP backups (<code>.stbk</code>) bundling all keys &amp; certs</td>
        </tr>
        <tr>
          <td><strong>Incident Triage</strong></td>
          <td>Manual raw log grepping or costly reporting add-ons</td>
          <td>Unified Blocklist Reports, built-in Wireshark <code>.pcap</code> capture, and read-only MCP AI tools</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

<div class="sol-cta-box">
  <div>
    <h3>Inspect the architecture and live control plane</h3>
    <p>Review our FreeBSD 15.1 subsystem design, test-drive the read-only Live Demo, or connect with our engineering team.</p>
  </div>
  <div class="sol-cta-actions">
    <a href="/product/architecture/" class="btn btn-primary">Read Architecture</a>
    <a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="btn btn-secondary" style="background:#FFFFFF;color:#0F172A;">Launch Live Demo</a>
    <a href="/company/contact/" class="btn btn-secondary" style="background:transparent;color:#F8FAFC;border-color:rgba(248,250,252,0.4);">Contact Us</a>
  </div>
</div>

</div>
