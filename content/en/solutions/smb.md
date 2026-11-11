---
title: "Small & Midsize Business (SMB)"
description: "Subscription-free enterprise network security for SMBs and branch offices — Multi-WAN failover, QR WireGuard VPN, ransomware DNSBL/IP defense, and Safe-Apply rollback."
lead: "Protect your office network with Multi-WAN continuity, effortless remote access, and layered ransomware defense—without a dedicated security team or annual UTM license renewals."
full_width: true
---
<div class="solutions-layout">

<div class="sol-block">
  <div class="sol-head">
    <span class="eyebrow">SMB &amp; Branch Architecture</span>
    <h2>Complete office gateway protection — zero recurring subscriptions</h2>
    <p>Small and midsize businesses are often forced to choose between fragile consumer routers and commercial UTM boxes whose annual IPS, Web Filtering, and VPN seat renewals cost more than the hardware itself. Stratum Firewall delivers the full enterprise stack on any compact x86-64 appliance or VM with zero recurring fees.</p>
  </div>

  <div class="card-grid grid-3">
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><circle cx="6" cy="19" r="3"/><path d="M9 19h8.5a3.5 3.5 0 0 0 0-7h-11a3.5 3.5 0 0 1 0-7H15"/><circle cx="18" cy="5" r="3"/></svg></div>
      <h3>Always-On Internet &amp; Guest Traffic Steering</h3>
      <p>Pair your primary fiber line with a secondary cable or 4G/5G LTE modem. Stratum detects outages in 1 second so VoIP phones, POS terminals, and cloud apps stay online—while Policy Routing keeps Guest Wi-Fi on your secondary ISP.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="11" width="18" height="11" rx="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg></div>
      <h3>30-Second Remote Access (WireGuard &amp; OpenVPN)</h3>
      <p>Stop exposing RDP or NAS ports to the internet. Provision kernel-speed WireGuard tunnels via instant mobile/laptop QR codes, or issue dedicated <code>VPN-only</code> OpenVPN profiles with zero per-seat fees.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/><path d="m9 12 2 2 4-4"/></svg></div>
      <h3>Ransomware, Phishing &amp; Ad Sinkholing</h3>
      <p>Protect every workstation and IoT device with zero client software using 63 automated IP threat feeds (<em>Spamhaus DROP, Feodo, Abuse.ch</em>), Unbound RPZ DNSBL, 55-category Web Filtering, and Suricata 8 IPS.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><polygon points="12 2 2 7 12 12 22 7 12 2"/><polyline points="2 17 12 22 22 17"/><polyline points="2 12 12 17 22 12"/></svg></div>
      <h3>Staff, Camera/IoT &amp; Guest VLAN Isolation</h3>
      <p>Segment office workstations, security cameras, printers, and visitor Wi-Fi across clean 802.1Q VLANs, and apply <code>dummynet</code> bandwidth limits so guest downloads never throttle business calls.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M3 12a9 9 0 0 1 15-6.7L21 8"/><path d="M21 3v5h-5"/><path d="M21 12a9 9 0 0 1-15 6.7L3 16"/><path d="M3 21v-5h5"/></svg></div>
      <h3>Zero-Lockout Administration (Safe-Apply)</h3>
      <p>Manage remote branch offices without fear: every firewall or interface change is pre-checked and guarded by a 60-second confirmation timer that rolls back automatically if connectivity drops.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="2" width="20" height="8" rx="2"/><rect x="2" y="14" width="20" height="8" rx="2"/><path d="M6 6h.01M6 18h.01"/></svg></div>
      <h3>Automated Encrypted Backups &amp; Alerts</h3>
      <p>Schedule encrypted <code>AES-256-GCM</code> (<code>.stbk</code>) backups—complete with VPN keys and TLS certificates—directly to your office NAS over SFTP, and receive real-time Telegram or SMTP alerts.</p>
    </div>
  </div>
</div>

<div class="sol-block">
  <div class="sol-head">
    <span class="eyebrow">Deployment Blueprint</span>
    <h2>From bare box to protected office in 15 minutes</h2>
  </div>

  <div class="sol-steps">
    <div class="sol-step">
      <span class="sol-step-num">STEP 01 · HARDWARE OR VM</span>
      <h3>Pick Compact x86-64 Hardware</h3>
      <p>Use any fanless 2–4 port Intel mini-appliance (4 GB RAM, 32 GB SSD) or allocate a lightweight VM on Proxmox VE, VMware ESXi, or KVM.</p>
    </div>
    <div class="sol-step">
      <span class="sol-step-num">STEP 02 · ZFS INSTALL</span>
      <h3>Flash ISO or Serial IMG</h3>
      <p>Boot the offline ZFS installer from the <a href="/download/" style="color:#2DD4BF;text-decoration:underline;">Download</a> page—or import your existing OPNsense <code>config.xml</code> directly.</p>
    </div>
    <div class="sol-step">
      <span class="sol-step-num">STEP 03 · TURNKEY DEFENSE</span>
      <h3>Enable Feeds, VLANs &amp; VPN</h3>
      <p>Activate IP/DNSBL blocklists, set Guest VLAN bandwidth limits, scan WireGuard QR codes for remote staff, and link Telegram/email notifications.</p>
    </div>
  </div>
</div>

<div class="sol-block">
  <div class="sol-head">
    <span class="eyebrow">Total Cost &amp; Capability Comparison</span>
    <h2>Why growing businesses replace commercial UTMs with Stratum FW</h2>
  </div>

  <div class="sol-table-wrap">
    <table class="sol-table">
      <thead>
        <tr>
          <th>Criteria</th>
          <th>Commercial SMB Firewalls (UTM)</th>
          <th>Stratum Firewall (FreeBSD 15.1)</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>Annual Security Subscriptions</strong></td>
          <td>Paid yearly renewals required for IPS, Web Filter &amp; GeoIP</td>
          <td><strong>$0 / year</strong> — All 63 IP feeds, DNSBL, 55-cat Web Filter, GeoIP &amp; Suricata IPS included</td>
        </tr>
        <tr>
          <td><strong>Remote VPN Licensing</strong></td>
          <td>Per-user or concurrent tunnel license caps</td>
          <td><strong>Unlimited</strong> WireGuard peers &amp; OpenVPN accounts</td>
        </tr>
        <tr>
          <td><strong>Upgrade Resilience</strong></td>
          <td>Failed firmware upgrades can brick remote branch routers</td>
          <td>Automatic bootable <strong>ZFS Boot Environment</strong> snapshot before every update</td>
        </tr>
        <tr>
          <td><strong>Hardware Freedom</strong></td>
          <td>Locked to proprietary vendor appliances</td>
          <td>Runs on any standard <code>amd64</code> hardware or virtual hypervisor</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

<div class="sol-cta-box">
  <div>
    <h3>See how clean office network security can be</h3>
    <p>Click around our live FreeBSD 15.1 demo appliance right now, or download the installer for your hardware.</p>
  </div>
  <div class="sol-cta-actions">
    <a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="btn btn-primary">Try Live Demo</a>
    <a href="/download/" class="btn btn-secondary" style="background:#FFFFFF;color:#0F172A;">Download Installer</a>
  </div>
</div>

</div>
