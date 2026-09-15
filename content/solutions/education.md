---
title: "Education"
lead: "Content filtering, network segmentation, and guest access built for schools and campuses."
---

## Why education networks are different

Educational institutions run some of the most demanding networks in any sector — with the smallest teams and the tightest budgets. Unique pressures:

- **Content filtering is mandatory** — regulatory requirements and duty-of-care obligations mean certain categories must be blocked, and the evidence must be defensible.
- **Mixed populations** — students, staff, administrators, guests, and connected devices all share the same physical infrastructure but need very different access.
- **Byzantine calendars** — traffic patterns spike during exam season and drop during holidays. Capacity has to flex without overspending.
- **Small IT teams** — one or two people often run a network designed for thousands of users.
- **Public transparency** — budgets are scrutinized. Licensing that scales with headcount is a hard sell.

**Stratum Gateway is designed for exactly this kind of environment.**

## What Stratum changes

<div class="card-grid grid-2" style="margin-top:32px;">

  <div class="card">
    <h3>Defensible content filtering</h3>
    <p>DNS-based category filtering with a curated feed catalog. Enable the categories you're required to block, keep logs of what was filtered, and update feeds automatically. Reports are exportable — evidence when you need it.</p>
  </div>

  <div class="card">
    <h3>Network segmentation</h3>
    <p>VLANs for staff, students, guest, IoT, and administrative networks. Each gets its own firewall policies. Student devices never touch administrative resources — enforced at the kernel level.</p>
  </div>

  <div class="card">
    <h3>Guest access done right</h3>
    <p>A captive portal for visitors with optional RADIUS authentication. Guest devices get internet access and nothing else. No more posting the Wi-Fi password on the wall.</p>
  </div>

  <div class="card">
    <h3>Safe IDS on the WAN</h3>
    <p>Suricata watches inbound traffic. CrowdSec blocks brute-force and scanning automatically. Students notice nothing; staff sleep better.</p>
  </div>

  <div class="card">
    <h3>Time-based policies</h3>
    <p>Objects and time-windows let you apply different rules during school hours and after. QoS keeps critical services prioritized when bandwidth is tight.</p>
  </div>

  <div class="card">
    <h3>No per-user licensing</h3>
    <p>A school with 200 students pays the same as one with 2,000. Open source, no recurring fees, no "student headcount" surprises at budget time.</p>
  </div>

</div>

## Reference deployment

A typical K-12 or small campus deployment:

<div class="smb-scenario">

  <div class="smb-scenario-step">
    <span class="smb-scenario-num">01</span>
    <div>
      <h4>Hardware</h4>
      <p>A 1U rack server or high-spec small-form-factor PC, sized to the student count. Runs on your existing network closet infrastructure.</p>
    </div>
  </div>

  <div class="smb-scenario-step">
    <span class="smb-scenario-num">02</span>
    <div>
      <h4>Networks</h4>
      <p>Staff VLAN, student VLAN, guest VLAN, IoT VLAN (printers, cameras, HVAC), and a management VLAN. Five firewall zones, one box.</p>
    </div>
  </div>

  <div class="smb-scenario-step">
    <span class="smb-scenario-num">03</span>
    <div>
      <h4>Filtering</h4>
      <p>Adult content, malware, phishing, and gambling categories enabled by default. Custom blocklist for anything specific to your institution.</p>
    </div>
  </div>

  <div class="smb-scenario-step">
    <span class="smb-scenario-num">04</span>
    <div>
      <h4>Guest access</h4>
      <p>Captive portal with a click-through terms page for visitors. Optional RADIUS integration if you want per-user authentication for guest network access.</p>
    </div>
  </div>

  <div class="smb-scenario-step">
    <span class="smb-scenario-num">05</span>
    <div>
      <h4>Operations</h4>
      <p>Weekly automatic backups. Updates on a schedule that avoids class hours. SNMP integration with the campus NMS if you already run one.</p>
    </div>
  </div>

</div>

## Feature highlights for education

<div class="card-grid grid-2" style="margin-top:32px;">

  <div class="card">
    <h3>Web Filtering</h3>
    <p>Category-based filtering with automated feed updates.</p>
    <a href="/product/features/#webfilter" class="card-link">Learn more →</a>
  </div>

  <div class="card">
    <h3>Captive Portal</h3>
    <p>Guest Wi-Fi with a customizable authentication page.</p>
    <a href="/product/features/#captive-portal" class="card-link">Learn more →</a>
  </div>

  <div class="card">
    <h3>RADIUS</h3>
    <p>Per-user authentication for staff and guest network access.</p>
    <a href="/product/features/#radius" class="card-link">Learn more →</a>
  </div>

  <div class="card">
    <h3>Firewall Rules</h3>
    <p>Segment staff, student, guest, and IoT traffic with clean policy.</p>
    <a href="/product/features/#rules" class="card-link">Learn more →</a>
  </div>

  <div class="card">
    <h3>QoS</h3>
    <p>Keep VoIP and management traffic flowing during peak hours.</p>
    <a href="/product/features/#qos" class="card-link">Learn more →</a>
  </div>

  <div class="card">
    <h3>IDS &amp; CrowdSec</h3>
    <p>Automated threat detection without a dedicated security team.</p>
    <a href="/product/features/#ids" class="card-link">Learn more →</a>
  </div>

</div>

## On cost and sustainability

Educational budgets are scrutinized, and rightly so. The question isn't just "what does it cost today" but "what will it cost in five years."

Stratum is open source. There are no per-user licenses, no per-device fees, no support contracts that quietly renew. You buy the hardware once. You run the platform forever. Updates are signed and delivered continuously at no cost.

If your institution later wants managed support or professional services, those are available — but they are options, not requirements.

<div style="margin-top:32px;">
  <a href="/try-me/" class="btn btn-primary">Try the live dashboard</a>
  <a href="mailto:info@stratumfw.com" class="btn btn-secondary">Talk to us</a>
</div>
