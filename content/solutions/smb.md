---
title: "Small Business"
lead: "Enterprise-grade security without the enterprise-grade overhead."
---

## The reality of a small business network

If you run a small business, your network probably looks like this:

- One or two internet connections — one of them unreliable
- A consumer router doing its best, held together with duct tape
- Wi-Fi for staff and guests that you're not entirely sure is secure
- No dedicated IT security person — whoever is "good with computers" handles it
- Cloud services you can't fully audit, and can't fully trust

The threats are real, but the budget isn't. **Stratum Gateway was designed exactly for this gap.**

## What Stratum changes

<div class="card-grid grid-2" style="margin-top:32px;">

  <div class="card">
    <h3>Reliable internet</h3>
    <p>Two connections, one firewall. If your primary link drops, traffic fails over in seconds. Your staff keeps working; you notice nothing until the ISP confirms it.</p>
  </div>

  <div class="card">
    <h3>Real security</h3>
    <p>Suricata IDS and CrowdSec run on the WAN edge. Known attacks, brute-force attempts, and automated scans get blocked at the kernel level — before they reach your systems.</p>
  </div>

  <div class="card">
    <h3>Content filtering</h3>
    <p>DNS-based filtering keeps malware, phishing, and unwanted categories off your network — with a feed catalog updated continuously.</p>
  </div>

  <div class="card">
    <h3>Secure remote access</h3>
    <p>WireGuard VPN lets staff access files from home or on the road. One QR code, and their laptop or phone is connected.</p>
  </div>

  <div class="card">
    <h3>Guest Wi-Fi that's actually separate</h3>
    <p>Captive portal for visitors, isolated from your internal network. No more handing out the main Wi-Fi password.</p>
  </div>

  <div class="card">
    <h3>Backups you can trust</h3>
    <p>Every module has its own backup. If you break something, restore just the piece you changed — not the whole box.</p>
  </div>

</div>

## A typical small business setup

<div class="smb-scenario">

  <div class="smb-scenario-step">
    <span class="smb-scenario-num">01</span>
    <div>
      <h4>Hardware</h4>
      <p>A small-form-factor PC — often a mini-desktop with two network ports. Total cost: less than a consumer router plus a year of cloud subscriptions.</p>
    </div>
  </div>

  <div class="smb-scenario-step">
    <span class="smb-scenario-num">02</span>
    <div>
      <h4>Connections</h4>
      <p>Two ISPs (cable + fiber, or fiber + LTE backup) connect to Stratum. Multi-WAN balancing keeps both useful; automatic failover keeps you online.</p>
    </div>
  </div>

  <div class="smb-scenario-step">
    <span class="smb-scenario-num">03</span>
    <div>
      <h4>Networks</h4>
      <p>Three VLANs: staff, guest, and IoT. Each gets its own policies. Guest has captive portal; IoT is isolated from everything else.</p>
    </div>
  </div>

  <div class="smb-scenario-step">
    <span class="smb-scenario-num">04</span>
    <div>
      <h4>Security</h4>
      <p>IDS on the WAN, CrowdSec watching for brute-force, geoblocking for countries you don't do business with. Protection running from day one.</p>
    </div>
  </div>

  <div class="smb-scenario-step">
    <span class="smb-scenario-num">05</span>
    <div>
      <h4>Day-two</h4>
      <p>Updates arrive on a schedule you choose. Backups run weekly. Everything's visible from one panel — from anywhere.</p>
    </div>
  </div>

</div>

## Feature highlights for small business

<div class="card-grid grid-2" style="margin-top:32px;">

  <div class="card">
    <h3>Multi-WAN Failover</h3>
    <p>Health-checked failover keeps you online when your ISP isn't.</p>
    <a href="/product/features/#multi-wan" class="card-link">Learn more →</a>
  </div>

  <div class="card">
    <h3>WireGuard VPN</h3>
    <p>Remote access that works on phones and laptops without a support ticket.</p>
    <a href="/product/features/#vpn" class="card-link">Learn more →</a>
  </div>

  <div class="card">
    <h3>Web Filtering</h3>
    <p>Automatic category blocking with continuously updated threat feeds.</p>
    <a href="/product/features/#webfilter" class="card-link">Learn more →</a>
  </div>

  <div class="card">
    <h3>Captive Portal</h3>
    <p>A guest network you can safely hand out to anyone.</p>
    <a href="/product/features/#captive-portal" class="card-link">Learn more →</a>
  </div>

  <div class="card">
    <h3>CrowdSec</h3>
    <p>Behavioral threat detection running quietly in the background.</p>
    <a href="/product/features/#crowdsec" class="card-link">Learn more →</a>
  </div>

  <div class="card">
    <h3>Backup &amp; Restore</h3>
    <p>Snapshots at the module level — roll back just what you changed.</p>
    <a href="/product/features/#backup" class="card-link">Learn more →</a>
  </div>

</div>

## Cost of ownership

The platform itself is open source and free. What you pay for is:

- **Hardware** — a small PC (typically $200–$500)
- **Your time** — an afternoon to set up, a few minutes per month to maintain
- **Optional support** — if you want a partner to manage it

There are no per-user licenses, no per-device fees, no subscription that stops working if a payment fails. Deploy it, keep it, own it.

<div style="margin-top:32px;">
  <a href="/try-me/" class="btn btn-primary">Try the live dashboard</a>
  <a href="mailto:info@stratumfw.com" class="btn btn-secondary">Talk to us</a>
</div>
