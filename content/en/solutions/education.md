---
title: "Education, Campus & Public Sector"
description: "Network security for schools, universities, dormitories, and libraries — 55-category Web Filter, forced Safe Search, per-student bandwidth shaping, and zero headcount licensing."
lead: "Deliver clientless 55-category content filtering, enforced Safe Search, per-student bandwidth fairness, and uninterrupted online exam connectivity—with zero per-student fees."
full_width: true
---
<div class="solutions-layout">

<div class="sol-block">
  <div class="sol-head">
    <span class="eyebrow">Education &amp; Campus Architecture</span>
    <h2>Built for thousands of BYOD student devices, lean IT teams, and tight budgets</h2>
    <p>Schools, university campuses, dormitories, and public libraries manage some of the toughest networks anywhere: thousands of unmanaged personal phones and laptops, strict statutory child-safety filtering mandates, and zero tolerance for downtime during digital exams—all without per-student subscription budgets.</p>
  </div>

  <div class="card-grid grid-3">
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><polygon points="22 3 2 3 10 12.46 10 19 14 21 14 12.46 22 3"/></svg></div>
      <h3>Per-VLAN 55-Category Web Filter</h3>
      <p>Enforce distinct DNS RPZ content policies per network across 55 categories powered by <em>Université Toulouse Capitole (UT1)</em> and <em>Block List Project</em> catalogs—applying strict student policies on Wi-Fi while giving faculty broader research access.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/><path d="m9 12 2 2 4-4"/></svg></div>
      <h3>Mandatory Safe Search &amp; Anti-Bypass</h3>
      <p>Force <strong>Safe Search</strong> across Google (including regional country domains), Bing, and DuckDuckGo plus <strong>YouTube Restricted Mode</strong>, while redirecting hard-coded DNS (<code>8.8.8.8</code>) and blocking DoT (<code>853</code>) and DoH bypasses.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="m12 14 4-4"/><path d="M3.34 19a10 10 0 1 1 17.32 0"/></svg></div>
      <h3>Per-Student Fair Bandwidth (<code>dummynet</code>)</h3>
      <p>Prevent a handful of game downloads or streams from saturating campus uplinks: configure shared VLAN caps or automatic <strong>per-device download/upload limits</strong> (e.g., <code>15 Mbit/s</code> per student device) directly in kernel <code>pf</code> + <code>dummynet</code>.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><polygon points="12 2 2 7 12 12 22 7 12 2"/><polyline points="2 17 12 22 22 17"/><polyline points="2 12 12 17 22 12"/></svg></div>
      <h3>Admin, Lab, Dorm &amp; Guest VLAN Segmentation</h3>
      <p>Isolate administrative ERP servers, faculty offices, computer labs, smartboards, and student/guest Wi-Fi using 802.1Q VLANs and interface groups—ensuring student BYOD devices can never probe administrative subnets.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M3 3v18h18"/><path d="M18 17V9M13 17V5M8 17v-3"/></svg></div>
      <h3>Audited Block Reports &amp; DHCP History</h3>
      <p>Meet institutional accountability requirements with administrator-only (audit-logged) <strong>Unified Blocklist Reports</strong>, per-device <strong>Web Activity</strong>, randomized mobile MAC indicators, Wake-on-LAN for computer labs, and CSV-exportable DHCP lease history.</p>
    </div>
    <div class="card">
      <div class="card-icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M22 21v-2a4 4 0 0 0-3-3.87M16 3.13a4 4 0 0 1 0 7.75"/></svg></div>
      <h3>Exam Uptime &amp; Unlimited Student Headcount</h3>
      <p>Keep digital testing and LMS platforms on your primary fiber while steering student/dormitory Wi-Fi onto a secondary ISP via Policy Routing and 1-second Multi-WAN failover—with <strong>$0 per-student licensing</strong> whether you serve 200 or 20,000 learners.</p>
    </div>
  </div>
</div>

<div class="sol-block">
  <div class="sol-head">
    <span class="eyebrow">Reference Campus Blueprint</span>
    <h2>Sample multi-VLAN policy &amp; bandwidth architecture</h2>
  </div>

  <div class="sol-table-wrap">
    <table class="sol-table">
      <thead>
        <tr>
          <th>Campus Segment (VLAN)</th>
          <th>Web Filter &amp; Safe Search Policy</th>
          <th>Bandwidth Shaper (<code>dummynet</code>)</th>
          <th>WAN Uplink &amp; Access Control</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>VLAN 10 · Administration &amp; SIS</strong></td>
          <td>Malware, Phishing &amp; C2 Threat Blocking</td>
          <td>Unthrottled (Full Priority)</td>
          <td>Primary Fiber (<code>WAN1</code>) · Strictly Isolated</td>
        </tr>
        <tr>
          <td><strong>VLAN 20 · Faculty &amp; Staff</strong></td>
          <td>Threat Feeds + Ad/Tracker DNSBL</td>
          <td>High Shared Pool</td>
          <td>Primary Fiber (<code>WAN1</code>) + Remote WireGuard/OpenVPN</td>
        </tr>
        <tr>
          <td><strong>VLAN 30 · Computer Labs &amp; Exams</strong></td>
          <td>Academic Policy + Enforced Safe Search</td>
          <td><code>25 Mbit/s</code> per Workstation + Wake-on-LAN</td>
          <td>Primary Fiber (<code>WAN1</code> with Instant Failover)</td>
        </tr>
        <tr>
          <td><strong>VLAN 40 · Student &amp; Dorm Wi-Fi</strong></td>
          <td>55-Category Student Policy + YouTube Restricted + Forced DNS</td>
          <td><code>15 Mbit/s</code> per Device Fair Queue</td>
          <td>Steered to Secondary ISP (<code>WAN2</code>) · Blocked from Admin</td>
        </tr>
        <tr>
          <td><strong>VLAN 50 · Campus Visitors</strong></td>
          <td>Adult, Malware &amp; Proxy-Bypass Blocked</td>
          <td><code>10 Mbit/s</code> per Device</td>
          <td>Secondary ISP (<code>WAN2</code>) · Internet-Only Access</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

<div class="sol-cta-box">
  <div>
    <h3>Test-drive Stratum FW for your school or campus</h3>
    <p>Explore the 55-category Web Filter, DNSBL reports, and VLAN rules in our Live Demo, or deploy onto your own server.</p>
  </div>
  <div class="sol-cta-actions">
    <a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="btn btn-primary">Explore Live Demo</a>
    <a href="/download/" class="btn btn-secondary" style="background:#FFFFFF;color:#0F172A;">Download ISO / IMG</a>
  </div>
</div>

</div>
