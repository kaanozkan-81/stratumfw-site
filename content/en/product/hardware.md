---
title: "Hardware & Appliance Profiles"
seo_title: "Stratum FW Hardware Profiles — 4-Port, 8-Port SFP+ & 12-Port 1U FreeBSD Firewalls"
description: "Explore Stratum FW 4-port fanless desktop, 8-port (2x SFP+ + 6x RJ45) branch, and 12-port (4x SFP+ + 8x RJ45) 1U rackmount firewall profiles with zero hardware lock-in."
lead: "From silent 4-port desktop gateways to 8-port SFP+ branch hubs and 12-port 1U rack appliances — engineered for FreeBSD 15.1 with zero vendor lock-in."
images: ["/images/hardware-stratum-family.jpg"]
---

<style>
.container *:has(.sg-wrap) { max-width: 100% !important; width: 100% !important; }
.hw-wrap { width: 100%; max-width: 1240px; margin: 0 auto; color: #f8fafc !important; }
.hw-hero { background: linear-gradient(145deg, #08101f 0%, #0f172a 100%) !important; border: 1px solid rgba(245, 158, 11, 0.32) !important; border-radius: 20px; padding: 34px; margin-bottom: 32px; box-shadow: 0 20px 45px rgba(2, 6, 23, 0.22); }
.hw-eyebrow { display: inline-block; background: rgba(245, 158, 11, 0.14) !important; color: #fbbf24 !important; border: 1px solid rgba(245, 158, 11, 0.38) !important; border-radius: 999px; padding: 5px 14px; font-size: 0.78rem; font-weight: 700; letter-spacing: 0.08em; text-transform: uppercase; margin-bottom: 14px; }
.hw-hero h2 { color: #f8fafc !important; font-size: clamp(1.75rem, 3vw, 2.4rem); line-height: 1.2; margin: 0 0 12px 0 !important; font-weight: 800; }
.hw-hero p { color: #cbd5e1 !important; font-size: 1.04rem; line-height: 1.68; max-width: 920px; margin: 0 0 24px 0 !important; }
.hw-family-img { width: 100%; border-radius: 14px; border: 1px solid rgba(148, 163, 184, 0.22); display: block; margin-bottom: 20px; }
.hw-pill-row { display: flex; flex-wrap: wrap; gap: 10px; }
.hw-pill { background: #08101f !important; border: 1px solid #1e293b !important; color: #e2e8f0 !important; border-radius: 999px; padding: 7px 15px; font-size: 0.86rem; font-weight: 600; }
.hw-pill strong { color: #fbbf24 !important; }
.hw-grid { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 24px; margin-bottom: 36px; }
@media (max-width: 980px) { .hw-grid { grid-template-columns: 1fr; } }
.hw-card { background: #0f172a !important; border: 1px solid #1e293b !important; border-radius: 18px; overflow: hidden; display: flex; flex-direction: column; box-shadow: 0 14px 32px rgba(2, 6, 23, 0.16); }
.hw-card-featured { border-color: rgba(245, 158, 11, 0.5) !important; }
.hw-card img { width: 100%; aspect-ratio: 16 / 9; object-fit: cover; display: block; border-bottom: 1px solid #1e293b; }
.hw-card-body { padding: 24px; display: flex; flex-direction: column; flex: 1; }
.hw-tier { color: #fbbf24 !important; font-size: 0.76rem; font-weight: 700; letter-spacing: 0.08em; text-transform: uppercase; margin-bottom: 6px; }
.hw-card h3 { color: #f8fafc !important; font-size: 1.32rem; margin: 0 0 10px 0 !important; font-weight: 700; }
.hw-port-badges { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 14px; }
.hw-badge-amber { background: rgba(245, 158, 11, 0.15) !important; color: #fbbf24 !important; border: 1px solid rgba(245, 158, 11, 0.35) !important; border-radius: 8px; padding: 4px 10px; font-size: 0.8rem; font-weight: 700; }
.hw-badge-cyan { background: rgba(56, 189, 248, 0.14) !important; color: #7dd3fc !important; border: 1px solid rgba(56, 189, 248, 0.32) !important; border-radius: 8px; padding: 4px 10px; font-size: 0.8rem; font-weight: 700; }
.hw-card p { color: #cbd5e1 !important; font-size: 0.94rem; line-height: 1.62; margin: 0 0 14px 0 !important; }
.hw-spec-list { list-style: none !important; padding: 0 !important; margin: auto 0 0 0 !important; border-top: 1px solid #1e293b; padding-top: 14px !important; }
.hw-spec-list li { color: #cbd5e1 !important; font-size: 0.88rem; line-height: 1.55; margin-bottom: 7px; padding-left: 16px; position: relative; }
.hw-spec-list li::before { content: "•"; color: #fbbf24 !important; position: absolute; left: 0; font-weight: 700; }
.hw-table-box { background: #0f172a !important; border: 1px solid #1e293b !important; border-radius: 18px; padding: 28px; margin-bottom: 32px; overflow-x: auto; }
.hw-table-box h3 { color: #f8fafc !important; margin: 0 0 16px 0 !important; font-size: 1.35rem; }
.hw-table { width: 100%; border-collapse: collapse; min-width: 680px; }
.hw-table th { text-align: left; padding: 12px 14px; background: #08101f !important; color: #fbbf24 !important; font-size: 0.83rem; text-transform: uppercase; letter-spacing: 0.05em; border-bottom: 1px solid #334155; }
.hw-table td { padding: 12px 14px; color: #e2e8f0 !important; font-size: 0.92rem; border-bottom: 1px solid #1e293b; }
.hw-table tr:last-child td { border-bottom: none; }
.hw-table code { background: #08101f !important; color: #7dd3fc !important; padding: 2px 6px; border-radius: 5px; font-size: 0.84rem; }
.hw-cta { background: linear-gradient(135deg, #08101f 0%, #0f172a 100%) !important; border: 1px solid rgba(56, 189, 248, 0.3) !important; border-radius: 18px; padding: 28px 32px; display: flex; flex-wrap: wrap; align-items: center; justify-content: space-between; gap: 20px; }
.hw-cta h3 { color: #f8fafc !important; margin: 0 0 6px 0 !important; font-size: 1.28rem; }
.hw-cta p { color: #cbd5e1 !important; margin: 0 !important; font-size: 0.95rem; max-width: 680px; }
.hw-cta-btns { display: flex; flex-wrap: wrap; gap: 12px; }
.hw-btn-amber { background: #f59e0b !important; color: #08101f !important; font-weight: 700; padding: 11px 20px; border-radius: 10px; text-decoration: none !important; font-size: 0.93rem; display: inline-block; }
.hw-btn-outline { background: rgba(255,255,255,0.06) !important; color: #f8fafc !important; border: 1px solid rgba(148,163,184,0.35) !important; font-weight: 600; padding: 11px 20px; border-radius: 10px; text-decoration: none !important; font-size: 0.93rem; display: inline-block; }
</style>
<div class="sg-wrap hw-wrap">
<div class="hw-hero">
<span class="hw-eyebrow">Stratum FW Appliance Lineup • Zero Hardware Lock-In</span>
<h2>4-Port Desktop, 8-Port Fiber Branch &amp; 12-Port 1U Enterprise Profiles</h2>
<p>Purpose-styled around FreeBSD 15.1-RELEASE, stateful <code style="color:#7dd3fc !important;">pf</code>, OpenZFS Boot Environments, and headless <code style="color:#7dd3fc !important;">115200 8N1</code> serial console recovery. Deploy Stratum FW on compact fanless aluminum boxes, SFP+ fiber branch gateways, 19-inch 1U rack appliances, or your own existing <code style="color:#7dd3fc !important;">amd64</code> servers and hypervisors at <strong style="color:#fbbf24 !important;">$0/year</strong>.</p>
<img src="/images/hardware-stratum-family.jpg" alt="Stratum FW 4-Port, 8-Port, and 12-Port Hardware Appliance Lineup" class="hw-family-img">
<div class="hw-pill-row">
<span class="hw-pill"><strong>SFW-400:</strong> 4x RJ45 Fanless Desktop</span>
<span class="hw-pill"><strong>SFW-800:</strong> 2x SFP+ &amp; 6x RJ45 Branch (8 Ports)</span>
<span class="hw-pill"><strong>SFW-1200:</strong> 4x SFP+ &amp; 8x RJ45 1U Rack (12 Ports)</span>
<span class="hw-pill"><strong>BYOD / VM:</strong> Any x86-64, Proxmox, KVM or VMware</span>
</div>
</div>
<div class="hw-grid">
<div class="hw-card">
<img src="/images/hardware-stratum-4port.jpg" alt="Stratum FW 4-Port Fanless Desktop Appliance">
<div class="hw-card-body">
<div class="hw-tier">Compact Fanless Desktop • 4 Ports</div>
<h3>Stratum FW 4-Port Profile</h3>
<div class="hw-port-badges">
<span class="hw-badge-amber">4x RJ45 Ethernet</span>
<span class="hw-badge-cyan">USB-C Console + USB 3.0</span>
</div>
<p>Silent passive-cooled CNC aluminum chassis engineered for home labs, micro-branches, clinics, and retail counters needing reliable Dual-WAN failover and VPN security.</p>
<ul class="hw-spec-list">
<li><strong>Interfaces:</strong> Ports <code style="color:#7dd3fc !important;">0 – 3</code> RJ45 for WAN1, WAN2 (4G/5G DHCP), LAN, and isolated Guest Wi-Fi</li>
<li><strong>Included Stack:</strong> 5-layer Security Guard, Guest Captive Portal, WireGuard &amp; OpenVPN</li>
<li><strong>Installer:</strong> Headless Serial <code style="color:#7dd3fc !important;">.img</code> or VGA <code style="color:#7dd3fc !important;">.iso</code> on OpenZFS</li>
</ul>
</div>
</div>
<div class="hw-card hw-card-featured">
<img src="/images/hardware-stratum-8port.jpg" alt="Stratum FW 8-Port Fiber Branch Appliance with 2 SFP+ and 6 RJ45">
<div class="hw-card-body">
<div class="hw-tier">Fiber Branch &amp; SMB Gateway • 8 Ports</div>
<h3>Stratum FW 8-Port Profile</h3>
<div class="hw-port-badges">
<span class="hw-badge-amber">2x SFP+ Fiber</span>
<span class="hw-badge-cyan">6x RJ45 Ethernet</span>
</div>
<p>Wide fanless heatsink enclosure combining dual SFP+ fiber cages (<code style="color:#7dd3fc !important;">1 – 2</code>) with six copper RJ45 ports (<code style="color:#7dd3fc !important;">3 – 8</code>) for multi-VLAN offices and multi-vendor S2S VPN hubs.</p>
<ul class="hw-spec-list">
<li><strong>Fiber Uplinks:</strong> Direct ISP fiber transceivers or 10G <code style="color:#7dd3fc !important;">802.1Q</code> VLAN trunks to core switches</li>
<li><strong>Site-to-Site VPN:</strong> Policy-based IPsec (<code style="color:#7dd3fc !important;">IKEv2/IKEv1</code>) &amp; dedicated kernel WireGuard S2S (<code style="color:#7dd3fc !important;">wg1+</code>)</li>
<li><strong>Inline Defense:</strong> Suricata 8 <code style="color:#7dd3fc !important;">netmap</code> IPS + <code style="color:#7dd3fc !important;">dummynet</code> per-device bandwidth shaping</li>
</ul>
</div>
</div>
<div class="hw-card">
<img src="/images/hardware-stratum-12port-1u.jpg" alt="Stratum FW 12-Port 1U Enterprise Rackmount Appliance with 4 SFP+ and 8 RJ45">
<div class="hw-card-body">
<div class="hw-tier">19-Inch 1U Rackmount • 12 Ports</div>
<h3>Stratum FW 12-Port 1U Profile</h3>
<div class="hw-port-badges">
<span class="hw-badge-amber">4x SFP+ Fiber</span>
<span class="hw-badge-cyan">8x RJ45 Ethernet</span>
</div>
<p>High-port-density 1U rack appliance with four SFP+ cages (<code style="color:#7dd3fc !important;">1 – 4</code>), eight copper RJ45 ports (<code style="color:#7dd3fc !important;">5 – 12</code>), and front RJ45 serial console for campuses, hotels, and headquarters.</p>
<ul class="hw-spec-list">
<li><strong>High-Density Routing:</strong> Multi-ISP fiber WANs, <code style="color:#7dd3fc !important;">/29</code> Virtual IPs, <code style="color:#7dd3fc !important;">1:1 NAT</code>, and 30-day inter-VLAN matrix</li>
<li><strong>Campus &amp; Hospitality:</strong> Multi-zone Guest Captive Portal + 56-category UT1 Web Filter</li>
<li><strong>AI Diagnostics:</strong> Read-only 15-tool MCP AI Assistant with local LAN <code style="color:#7dd3fc !important;">Ollama</code> or <code style="color:#7dd3fc !important;">PUB-1</code> masking</li>
</ul>
</div>
</div>
</div>
<div class="hw-table-box">
<h3>Appliance Port &amp; Capability Matrix</h3>
<table class="hw-table">
<thead>
<tr>
<th>Specification</th>
<th>4-Port Desktop</th>
<th>8-Port Branch</th>
<th>12-Port 1U Rack</th>
<th>Custom x86-64 / VM</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Form Factor</strong></td>
<td>Compact Fanless Desktop</td>
<td>Wide Fanless Desktop</td>
<td>19-inch 1U Rackmount</td>
<td>Mini-PC, Server, Proxmox, KVM</td>
</tr>
<tr>
<td><strong>Total Network Ports</strong></td>
<td><strong>4 Ports</strong></td>
<td><strong>8 Ports</strong></td>
<td><strong>12 Ports</strong></td>
<td>Up to hardware / NIC limit</td>
</tr>
<tr>
<td><strong>SFP+ Fiber Cages</strong></td>
<td>—</td>
<td><code>2x SFP+</code> (Ports 1–2)</td>
<td><code>4x SFP+</code> (Ports 1–4)</td>
<td>Supported (<code>ix</code>, <code>ixl</code>, etc.)</td>
</tr>
<tr>
<td><strong>RJ45 Copper Ports</strong></td>
<td><code>4x RJ45</code> (Ports 0–3)</td>
<td><code>6x RJ45</code> (Ports 3–8)</td>
<td><code>8x RJ45</code> (Ports 5–12)</td>
<td>Supported (<code>igb</code>, <code>em</code>, <code>vtnet</code>)</td>
</tr>
<tr>
<td><strong>Console &amp; Media</strong></td>
<td>USB-C Serial + USB 3.0 (<code>.img</code>)</td>
<td>USB-C Serial + USB 3.0 (<code>.img</code>)</td>
<td>RJ45 Serial + USB 3.0 (<code>.img</code> / <code>.iso</code>)</td>
<td>VGA <code>.iso</code> or Serial <code>.img</code></td>
</tr>
<tr>
<td><strong>Rollback &amp; Updates</strong></td>
<td>120s Safe-Apply + OpenZFS BE</td>
<td>120s Safe-Apply + OpenZFS BE</td>
<td>120s Safe-Apply + OpenZFS BE</td>
<td>120s Safe-Apply + OpenZFS BE</td>
</tr>
<tr>
<td><strong>Full Software Suite</strong></td>
<td><strong style="color:#fbbf24 !important;">$0 / Year</strong> (All features)</td>
<td><strong style="color:#fbbf24 !important;">$0 / Year</strong> (All features)</td>
<td><strong style="color:#fbbf24 !important;">$0 / Year</strong> (All features)</td>
<td><strong style="color:#fbbf24 !important;">$0 / Year</strong> (All features)</td>
</tr>
</tbody>
</table>
</div>
<div class="hw-cta">
<div>
<h3>Deploy on Your Own Hardware Today or Inquire About Turnkey Sizing</h3>
<p>Every Stratum FW feature — Security Guard, Multi-Vendor VPN, Captive Portal, and the Read-Only AI Assistant — ships unlocked in the free <code style="color:#7dd3fc !important;">amd64</code> installer with zero serial-number licensing.</p>
</div>
<div class="hw-cta-btns">
<a href="/download/" class="hw-btn-amber">Download ISO &amp; Serial IMG →</a>
<a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="hw-btn-outline">Try Live Demo</a>
<a href="/company/contact/" class="hw-btn-outline">Hardware &amp; OEM Inquiry</a>
</div>
</div>
</div>
