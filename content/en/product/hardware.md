---
title: "Hardware & Appliance Profiles"
seo_title: "Stratum FW Hardware Profiles — 4-Port, 8-Port SFP+ & 12-Port 1U FreeBSD Firewalls"
description: "Explore Stratum FW 4-port fanless desktop, 8-port (2x SFP+ + 6x RJ45) branch, and 12-port (4x SFP+ + 8x RJ45) 1U rackmount firewall profiles with zero hardware lock-in."
lead: "From silent 4-port desktop gateways to 8-port SFP+ branch hubs and 12-port 1U rack appliances — engineered for FreeBSD 15.1 with zero vendor lock-in."
translationKey: "product-hardware"
full_width: true
images: ["/images/hardware-stratum-family.jpg"]
---

<style>
main:has(.hw-layout) .container,
.container:has(.hw-layout) {
max-width: 1240px !important;
width: 100% !important;
margin-left: auto !important;
margin-right: auto !important;
}
.container *:has(.hw-layout),
.prose:has(.hw-layout),
article:has(.hw-layout),
.single-content:has(.hw-layout),
.page-content:has(.hw-layout) {
max-width: 100% !important;
width: 100% !important;
}
.hw-layout {
width: 100%;
font-family: var(--font-sans, 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif);
color: #1E293B;
}
.hw-layout code {
background: #F1F5F9 !important;
color: #0F172A !important;
border: 1px solid #CBD5E1 !important;
padding: 2px 7px !important;
border-radius: 5px !important;
font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace !important;
font-size: 0.84em !important;
font-weight: 600 !important;
}
.hw-showcase-frame {
background: #FFFFFF;
border: 1px solid #E2E8F0;
border-radius: 18px;
overflow: hidden;
box-shadow: 0 10px 30px rgba(15, 23, 42, 0.06);
margin-bottom: 56px;
}
.hw-showcase-img {
width: 100%;
display: block;
border-bottom: 1px solid #E2E8F0;
}
.hw-kpi-strip {
display: grid;
grid-template-columns: repeat(4, minmax(0, 1fr));
background: #0F172A;
}
@media (max-width: 960px) {
.hw-kpi-strip { grid-template-columns: repeat(2, minmax(0, 1fr)); }
}
@media (max-width: 560px) {
.hw-kpi-strip { grid-template-columns: 1fr; }
}
.hw-kpi-cell {
padding: 18px 22px;
border-right: 1px solid #1E293B;
}
.hw-kpi-cell:last-child {
border-right: none;
}
.hw-kpi-label {
display: block;
font-size: 11.5px;
font-weight: 700;
text-transform: uppercase;
letter-spacing: 0.07em;
color: #FBBF24 !important;
margin-bottom: 4px;
}
.hw-kpi-val {
display: block;
font-size: 14.5px;
font-weight: 600;
color: #F8FAFC !important;
line-height: 1.4;
}
.hw-sec-head {
max-width: 820px;
margin-bottom: 28px;
}
.hw-sec-eyebrow {
display: block;
color: #D97706 !important;
font-weight: 700;
font-size: 12.5px;
text-transform: uppercase;
letter-spacing: 0.08em;
margin-bottom: 6px;
}
.hw-sec-head h2 {
font-family: var(--font-display, 'Inter', system-ui, sans-serif);
font-size: 28px !important;
font-weight: 700 !important;
color: #090D16 !important;
letter-spacing: -0.015em;
line-height: 1.25 !important;
margin: 0 0 8px 0 !important;
}
.hw-sec-head p {
font-size: 16px !important;
color: #475569 !important;
line-height: 1.62 !important;
margin: 0 !important;
}
.hw-grid {
display: grid;
grid-template-columns: repeat(3, minmax(0, 1fr));
gap: 24px;
margin-bottom: 60px;
}
@media (max-width: 1024px) {
.hw-grid { grid-template-columns: 1fr; }
}
.hw-card {
background: #FFFFFF !important;
border: 1px solid #E2E8F0 !important;
border-radius: 16px;
overflow: hidden;
display: flex;
flex-direction: column;
box-shadow: 0 6px 22px rgba(15, 23, 42, 0.05);
transition: transform 0.18s ease, box-shadow 0.18s ease;
}
.hw-card-featured {
border: 2px solid #F59E0B !important;
box-shadow: 0 10px 28px rgba(245, 158, 11, 0.10);
}
.hw-card img {
width: 100%;
aspect-ratio: 16 / 9;
object-fit: cover;
display: block;
border-bottom: 1px solid #E2E8F0;
}
.hw-card-body {
padding: 24px;
display: flex;
flex-direction: column;
flex: 1;
}
.hw-tier {
font-size: 12px;
font-weight: 700;
letter-spacing: 0.06em;
text-transform: uppercase;
color: #B45309 !important;
margin-bottom: 6px;
}
.hw-card h3 {
font-family: var(--font-display, 'Inter', system-ui, sans-serif);
font-size: 20px !important;
font-weight: 700 !important;
color: #090D16 !important;
letter-spacing: -0.01em;
margin: 0 0 12px 0 !important;
}
.hw-badges {
display: flex;
flex-wrap: wrap;
gap: 8px;
margin-bottom: 14px;
}
.hw-badge-amber {
background: #FEF3C7 !important;
color: #92400E !important;
border: 1px solid #FDE68A !important;
border-radius: 999px;
padding: 4px 11px;
font-size: 12.5px;
font-weight: 700;
}
.hw-badge-slate {
background: #E0F2FE !important;
color: #075985 !important;
border: 1px solid #BAE6FD !important;
border-radius: 999px;
padding: 4px 11px;
font-size: 12.5px;
font-weight: 700;
}
.hw-card p {
font-size: 14.8px !important;
line-height: 1.6 !important;
color: #334155 !important;
margin: 0 0 16px 0 !important;
}
.hw-list {
list-style: none !important;
padding: 14px 0 0 0 !important;
margin: auto 0 0 0 !important;
border-top: 1px solid #F1F5F9;
}
.hw-list li {
font-size: 14px !important;
line-height: 1.55 !important;
color: #334155 !important;
margin-bottom: 8px !important;
padding-left: 16px !important;
position: relative;
}
.hw-list li:last-child {
margin-bottom: 0 !important;
}
.hw-list li strong {
color: #0F172A !important;
}
.hw-list li::before {
content: "•";
color: #D97706 !important;
font-weight: 800;
position: absolute;
left: 0;
}
.hw-table-wrap {
overflow-x: auto;
border: 1px solid #E2E8F0;
border-radius: 16px;
box-shadow: 0 8px 24px rgba(15, 23, 42, 0.05);
background: #FFFFFF;
margin-bottom: 56px;
}
.hw-table {
width: 100%;
border-collapse: collapse;
font-size: 14.5px;
min-width: 780px;
}
.hw-table thead th {
background: linear-gradient(135deg, #0F172A 0%, #1E293B 100%) !important;
color: #F8FAFC !important;
font-weight: 700 !important;
text-align: left;
padding: 16px 18px;
border-bottom: 3px solid #F59E0B;
white-space: nowrap;
}
.hw-table tbody td {
padding: 14px 18px;
border-bottom: 1px solid #E2E8F0;
color: #1E293B !important;
vertical-align: middle;
line-height: 1.5;
}
.hw-table tbody tr:nth-child(even) td {
background: #F8FAFC;
}
.hw-table tbody tr:last-child td {
border-bottom: none;
}
.hw-table tbody td strong {
color: #090D16 !important;
}
.hw-price-pill {
display: inline-block;
background: #ECFDF5 !important;
color: #047857 !important;
border: 1px solid #A7F3D0 !important;
font-weight: 700;
font-size: 12.5px;
padding: 3px 10px;
border-radius: 999px;
}
.hw-cta-dark {
background: linear-gradient(135deg, #0F172A 0%, #1E293B 100%) !important;
border: 1px solid rgba(245, 158, 11, 0.4) !important;
border-radius: 18px;
padding: 32px 36px;
display: flex;
flex-wrap: wrap;
align-items: center;
justify-content: space-between;
gap: 24px;
}
.hw-cta-dark h3 {
font-family: var(--font-display, 'Inter', system-ui, sans-serif);
color: #F8FAFC !important;
font-size: 22px !important;
font-weight: 700 !important;
margin: 0 0 6px 0 !important;
}
.hw-cta-dark p {
color: #CBD5E1 !important;
font-size: 15px !important;
line-height: 1.6 !important;
margin: 0 !important;
max-width: 680px;
}
.hw-cta-dark code {
background: rgba(255, 255, 255, 0.12) !important;
color: #FDE68A !important;
border: 1px solid rgba(251, 191, 36, 0.35) !important;
}
.hw-cta-actions {
display: flex;
flex-wrap: wrap;
gap: 12px;
}
.hw-btn-primary {
background: #F59E0B !important;
color: #090D16 !important;
font-weight: 700 !important;
font-size: 14.5px;
padding: 11px 22px;
border-radius: 10px;
text-decoration: none !important;
display: inline-block;
}
.hw-btn-secondary {
background: #FFFFFF !important;
color: #0F172A !important;
font-weight: 600 !important;
font-size: 14.5px;
padding: 11px 20px;
border-radius: 10px;
text-decoration: none !important;
display: inline-block;
}
</style>
<div class="hw-layout">
<div class="hw-showcase-frame">
<img src="/images/hardware-stratum-family.jpg" alt="Stratum FW 4-Port, 8-Port, and 12-Port Hardware Appliance Family" class="hw-showcase-img">
<div class="hw-kpi-strip">
<div class="hw-kpi-cell">
<span class="hw-kpi-label">SFW-400 • Compact Edge</span>
<span class="hw-kpi-val">4x RJ45 Fanless Desktop</span>
</div>
<div class="hw-kpi-cell">
<span class="hw-kpi-label">SFW-800 • Fiber Branch</span>
<span class="hw-kpi-val">2x SFP+ &amp; 6x RJ45 (8 Ports)</span>
</div>
<div class="hw-kpi-cell">
<span class="hw-kpi-label">SFW-1200 • 1U Enterprise</span>
<span class="hw-kpi-val">4x SFP+ &amp; 8x RJ45 (12 Ports)</span>
</div>
<div class="hw-kpi-cell">
<span class="hw-kpi-label">Zero Hardware Lock-In</span>
<span class="hw-kpi-val">Any amd64 Server or VM • $0/yr</span>
</div>
</div>
</div>
<div class="hw-sec-head">
<span class="hw-sec-eyebrow">Reference Appliance Profiles</span>
<h2>Three Scalable Port Profiles Built Around FreeBSD 15.1</h2>
<p>Every Stratum FW deployment runs the exact same asynchronous FastAPI control plane, stateful <code>pf</code> packet filter, OpenZFS Boot Environments, and <code>115200 8N1</code> serial console recovery — whether deployed on a turnkey appliance or your own <code>amd64</code> hardware.</p>
</div>
<div class="hw-grid">
<div class="hw-card">
<img src="/images/hardware-stratum-4port.jpg" alt="Stratum FW 4-Port Fanless Desktop Appliance">
<div class="hw-card-body">
<div class="hw-tier">Compact Fanless Desktop • 4 Ports</div>
<h3>Stratum FW 4-Port Profile</h3>
<div class="hw-badges">
<span class="hw-badge-amber">4x RJ45 Ethernet</span>
<span class="hw-badge-slate">USB-C Console + USB 3.0</span>
</div>
<p>Silent passive-cooled CNC aluminum box tailored for home labs, micro-branches, clinics, and retail counters needing reliable Dual-WAN failover and VPN access.</p>
<ul class="hw-list">
<li><strong>Port Layout:</strong> Ports <code>0 – 3</code> RJ45 for WAN1, 4G/5G DHCP WAN2, LAN, and Guest Wi-Fi</li>
<li><strong>Security &amp; Access:</strong> 5-layer Security Guard, Guest Captive Portal, WireGuard &amp; OpenVPN</li>
<li><strong>Install Media:</strong> Serial <code>.img</code> or VGA <code>.iso</code> with OpenZFS rollback</li>
</ul>
</div>
</div>
<div class="hw-card hw-card-featured">
<img src="/images/hardware-stratum-8port.jpg" alt="Stratum FW 8-Port Fiber Branch Appliance">
<div class="hw-card-body">
<div class="hw-tier">Fiber Branch &amp; SMB Hub • 8 Ports</div>
<h3>Stratum FW 8-Port Profile</h3>
<div class="hw-badges">
<span class="hw-badge-amber">2x SFP+ Fiber</span>
<span class="hw-badge-slate">6x RJ45 Ethernet</span>
</div>
<p>Wide fanless heatsink chassis pairing two SFP+ fiber cages (<code>1 – 2</code>) with six copper RJ45 ports (<code>3 – 8</code>) for multi-VLAN offices and multi-vendor S2S VPN hubs.</p>
<ul class="hw-list">
<li><strong>Fiber Uplinks:</strong> Direct ISP fiber transceivers or 10G <code>802.1Q</code> VLAN trunks to switches</li>
<li><strong>Site-to-Site VPN:</strong> Policy-based IPsec (<code>IKEv2/IKEv1</code>) &amp; kernel WireGuard S2S (<code>wg1+</code>)</li>
<li><strong>Inline Defense:</strong> Suricata 8 <code>netmap</code> IPS + <code>dummynet</code> per-device bandwidth limits</li>
</ul>
</div>
</div>
<div class="hw-card">
<img src="/images/hardware-stratum-12port-1u.jpg" alt="Stratum FW 12-Port 1U Rackmount Appliance">
<div class="hw-card-body">
<div class="hw-tier">19-Inch 1U Rackmount • 12 Ports</div>
<h3>Stratum FW 12-Port 1U Profile</h3>
<div class="hw-badges">
<span class="hw-badge-amber">4x SFP+ Fiber</span>
<span class="hw-badge-slate">8x RJ45 Ethernet</span>
</div>
<p>High-density 1U rack appliance combining four SFP+ fiber cages (<code>1 – 4</code>), eight copper RJ45 ports (<code>5 – 12</code>), and front RJ45 serial console for campuses, hotels, and headquarters.</p>
<ul class="hw-list">
<li><strong>Core Routing:</strong> Multi-ISP fiber WANs, <code>/29</code> Virtual IPs, <code>1:1 NAT</code> &amp; 30-day VLAN flow matrix</li>
<li><strong>Campus &amp; Guest Wi-Fi:</strong> Multi-zone Captive Portal + 56-category UT1 Web Filter</li>
<li><strong>AI Diagnostics:</strong> Read-only 15-tool MCP AI Assistant with LAN <code>Ollama</code> or <code>PUB-1</code> masking</li>
</ul>
</div>
</div>
</div>
<div class="hw-sec-head">
<span class="hw-sec-eyebrow">Technical Comparison</span>
<h2>Appliance Port &amp; Deployment Matrix</h2>
<p>Compare the 4-port, 8-port, and 12-port reference profiles alongside custom x86-64 servers and virtual machines.</p>
</div>
<div class="hw-table-wrap">
<table class="hw-table">
<thead>
<tr>
<th>Specification</th>
<th>4-Port Desktop</th>
<th>8-Port Branch</th>
<th>12-Port 1U Rack</th>
<th>Custom amd64 / VM</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Form Factor</strong></td>
<td>Compact Fanless Desktop</td>
<td>Wide Fanless Desktop</td>
<td>19-inch 1U Rackmount</td>
<td>Mini-PC, Rack Server, Proxmox, KVM</td>
</tr>
<tr>
<td><strong>Total Network Ports</strong></td>
<td><strong>4 Ports</strong></td>
<td><strong>8 Ports</strong></td>
<td><strong>12 Ports</strong></td>
<td>Up to hardware / NIC capacity</td>
</tr>
<tr>
<td><strong>SFP+ Fiber Cages</strong></td>
<td>None</td>
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
<td><strong>Console &amp; Installer</strong></td>
<td>USB-C Serial + USB 3.0 (<code>.img</code>)</td>
<td>USB-C Serial + USB 3.0 (<code>.img</code>)</td>
<td>RJ45 Serial + USB 3.0 (<code>.img</code> / <code>.iso</code>)</td>
<td>VGA <code>.iso</code> or Serial <code>.img</code></td>
</tr>
<tr>
<td><strong>Rollback &amp; Updates</strong></td>
<td>120 s Safe-Apply + OpenZFS BE</td>
<td>120 s Safe-Apply + OpenZFS BE</td>
<td>120 s Safe-Apply + OpenZFS BE</td>
<td>120 s Safe-Apply + OpenZFS BE</td>
</tr>
<tr>
<td><strong>Software &amp; UTM License</strong></td>
<td><span class="hw-price-pill">$0 / Year — All Unlocked</span></td>
<td><span class="hw-price-pill">$0 / Year — All Unlocked</span></td>
<td><span class="hw-price-pill">$0 / Year — All Unlocked</span></td>
<td><span class="hw-price-pill">$0 / Year — All Unlocked</span></td>
</tr>
</tbody>
</table>
</div>
<div class="hw-cta-dark">
<div>
<h3>Deploy on Your Existing Hardware Today or Contact Us for Sizing</h3>
<p>All Stratum FW capabilities — Security Guard, Multi-Vendor S2S VPN, Captive Portal, and the Read-Only AI Assistant — ship unlocked in our free <code>amd64</code> installers.</p>
</div>
<div class="hw-cta-actions">
<a href="/download/" class="hw-btn-primary">Download ISO &amp; Serial IMG →</a>
<a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="hw-btn-secondary">Try Live Demo</a>
<a href="/company/contact/" class="hw-btn-secondary">Hardware &amp; OEM Inquiry</a>
</div>
</div>
</div>
