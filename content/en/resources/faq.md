---
title: "FAQ"
description: "Frequently asked questions about Stratum Firewall v0.30.3, OPNsense & OpenVPN migration, Suricata IPS, GeoIP filtering, ZFS boot environments, and installers."
lead: "Answers to the most common questions about Stratum Firewall, OPNsense migration, security features, updates, and deployment."
full_width: true
---
<div class="faq-sections">

<section class="faq-section">
  <h2>General &amp; Platform</h2>
  <div class="faq">
    <details><summary>What is Stratum Firewall?</summary><div class="faq-body">Stratum Firewall (Stratum FW) is a self-hosted Network Security Operating System built on <strong>FreeBSD 15.1-RELEASE</strong> and the native <code>pf</code> packet filter, managed through a fast, asynchronous <strong>Python 3.12 / FastAPI</strong> web interface and REST API.</div></details>
    <details><summary>Can I try Stratum Firewall online before installing it?</summary><div class="faq-body">Yes! You can explore a real FreeBSD 15.1 virtual appliance running Stratum FW v0.30.3 directly in your browser via our <a href="https://demo.stratumfw.com" target="_blank" rel="noopener">Live Demo (demo.stratumfw.com)</a> in read-only sandbox mode (credentials <code>demo</code> / <code>Stratum2026</code> are auto-filled on the sign-in screen).</div></details>
    <details><summary>Why did you choose FreeBSD 15.1 instead of Linux?</summary><div class="faq-body">FreeBSD is engineered as a cohesive operating system rather than a patchwork of disparate components. Its battle-tested network stack, native <code>pf</code> firewall, high-speed <code>netmap</code> packet I/O, and built-in <strong>ZFS</strong> boot environments provide rock-solid predictability for network appliances.</div></details>
    <details><summary>Are there user limits, subscriptions, or telemetry?</summary><div class="faq-body">No. There are zero per-user fees, device limits, or mandatory subscriptions, and Stratum FW sends zero telemetry or analytics. It can run completely air-gapped using signed offline update packages (<code>.sfu</code>).</div></details>
  </div>
</section>

<section class="faq-section">
  <h2>OPNsense Migration, Architecture &amp; Updates</h2>
  <div class="faq">
    <details><summary>Can I migrate my existing OPNsense configuration and OpenVPN users?</summary><div class="faq-body">Yes (<code>v0.25.0</code> – <code>v0.30.3</code>). Under <strong>System &gt; Backup &gt; Import from OPNsense</strong>, upload your OPNsense <code>config.xml</code> to map hardware ports and convert interfaces, VLANs, nested aliases, firewall &amp; NAT rules, DHCP reservations, and DNS overrides. Enabling <em>"Also import OpenVPN"</em> transfers your CA, server &amp; user certificates, <code>tls-crypt</code>/<code>tls-auth</code> keys, and VPN user password hashes (including PHP <code>$2y$</code> bcrypt and <code>argon2id</code>) so existing client <code>.ovpn</code> profiles connect unchanged.</div></details>
    <details><summary>What are "Declarative Configuration" and the "Safe-Apply" workflow?</summary><div class="faq-body">All settings reside in a single auditable <code>config.json</code> source of truth that generates <code>pf.conf</code>, Unbound, dnsmasq, Suricata, OpenVPN, and WireGuard configs cleanly. Every change is pre-checked (<code>pfctl -n</code>, etc.) and applied with a <strong>60-second confirmation timer</strong>—if a rule cuts off your remote session, Stratum automatically rolls back to the previous working state.</div></details>
    <details><summary>How do system updates and ZFS Boot Environments work?</summary><div class="faq-body">Appliances update directly from our signed update server (<strong>System &gt; Update</strong>) or via offline <code>.sfu</code> files. Before applying any update, Stratum automatically snapshots the system into a new <strong>ZFS Boot Environment</strong>, allowing instant rollback at boot time if needed.</div></details>
    <details><summary>Does Stratum support Role-Based Access Control (RBAC) and API automation?</summary><div class="faq-body">Yes. Stratum provides <strong>Administrator</strong>, <strong>Operator</strong>, <strong>Read-Only</strong>, and <strong>VPN-only</strong> user roles, and its API-first FastAPI backend lets you automate configurations with Ansible, Terraform, or Python scripts.</div></details>
  </div>
</section>

<section class="faq-section">
  <h2>Security, VPN &amp; Networking Capabilities</h2>
  <div class="faq">
    <details><summary>How does per-interface Suricata IPS/IDS work?</summary><div class="faq-body">Under <strong>Services &gt; Intrusion Detection</strong>, each interface can independently run in <strong>Watch (IDS)</strong> or inline <strong>Block (IPS)</strong> mode. When mixed across interfaces, Stratum supervises dedicated <code>netmap</code> (blocking) and <code>pcap</code> (watch-only) Suricata processes and reloads rule category updates live via FreeBSD <code>SIGUSR2</code> without dropping traffic.</div></details>
    <details><summary>Do GeoIP country blocking, DNSBL, and IP blocklists require paid API keys?</summary><div class="faq-body">No—everything works out of the box with zero license keys. Stratum includes weekly <strong>Country (GeoIP) aliases</strong> pulled directly from all five Regional Internet Registries (ARIN, RIPE NCC, APNIC, LACNIC, AFRINIC), CDN-friendly <strong>Hostname aliases</strong>, 63 hourly-updated IPv4 threat feeds in <code>pf</code> tables, and Unbound RPZ <strong>DNSBL sinkholing</strong>.</div></details>
    <details><summary>Do you support WireGuard, Multi-WAN OpenVPN, Virtual IPs (VIPs), and Static Routes?</summary><div class="faq-body">Yes. Stratum supports kernel-native <strong>WireGuard (<code>if_wg</code>)</strong> with instant QR/config generation, <strong>OpenVPN</strong> with multi-WAN <code>multihome</code> reply routing and Windows <code>block-outside-dns</code>, <strong>Multi-WAN failover</strong>, <strong>Virtual IPs (VIPs)</strong> for routed public subnets (<code>/29</code>, etc.) with NAT reflection, and <strong>Static Routes</strong> reconciled cleanly via <code>stratum-netextra</code>.</div></details>
    <details><summary>What built-in diagnostic tools are included?</summary><div class="faq-body">Stratum includes downloadable Wireshark <strong>Packet Capture (<code>.pcap</code>)</strong> with smart filters, an <strong>ARP Table</strong> with randomized mobile MAC indicators and one-click <strong>Wake-on-LAN</strong>, live firewall rule hit counters, and a <strong>Remote-Safe Factory Reset</strong> that preserves active physical NIC assignments so remote appliances stay reachable.</div></details>
  </div>
</section>

<section class="faq-section">
  <h2>Installers &amp; Support</h2>
  <div class="faq">
    <details><summary>Which installer images are available?</summary><div class="faq-body">Stratum FW v0.30.3 (FreeBSD 15.1 amd64) ships with both a bootable <strong>VGA ISO (<code>.iso</code>)</strong> for standard displays and virtual machines (Proxmox, VMware, KVM) and a raw <strong>Serial Console USB (<code>.img</code>)</strong> disk image for headless hardware appliances on the <a href="/download/">Download</a> page.</div></details>
    <details><summary>How do I get help or report a bug?</summary><div class="faq-body">Reach us directly at <a href="mailto:info@stratumfw.com">info@stratumfw.com</a> or open an issue on <a href="https://github.com/kaanozkan-81/stratum-panel/issues" target="_blank" rel="noopener">GitHub Issues</a>.</div></details>
    <!-- STRATUM-V0300-FAQ-START -->
    <details><summary>Does Stratum Firewall support Two-Factor Authentication (2FA) for admin sign-in?</summary><div class="faq-body">Yes. Starting in <strong>v0.30.0</strong>, every panel account can enable <strong>TOTP two-factor sign-in</strong> under <strong>My Account</strong> using <em>Google Authenticator</em>, <em>Microsoft Authenticator</em>, <em>1Password</em>, <em>Bitwarden</em>, or any standard authenticator app. The QR code is drawn locally in the browser from a bundled library so your TOTP secret never leaves the firewall. Setup generates <strong>10 one-time recovery codes</strong>, enforces single-use code replay protection, and allows administrators (or physical/serial console access via <code>stratum-menu</code>) to reset 2FA if a phone is lost.</div></details>
    <details><summary>Can I prevent users from bypassing DNS blocklists and the Web Filter with hardcoded DNS?</summary><div class="faq-body">Yes. Enable <strong>Services &rarr; DNS Resolver &rarr; Force DNS through the firewall</strong> (<code>v0.29.2</code>) on your chosen LAN, Guest, or VLAN interfaces. Any device querying an external resolver like <code>8.8.8.8</code> or <code>1.1.1.1</code> is transparently redirected to Stratum&rsquo;s local resolver, and <strong>DNS over TLS (<code>TCP 853</code>)</strong> is blocked on those networks. If you run <strong>Active Directory</strong>, add your domain controllers to the <strong>Exception alias</strong> so internal AD lookups bypass redirection, and Force DNS records zero browsing history on its own.</div></details>
    <details><summary>Does Multi-WAN work with dynamic DHCP backup connections like 4G/5G LTE, Cable, or Starlink?</summary><div class="faq-body">Yes (<code>v0.29.1</code>). Secondary and tertiary WAN interfaces (<code>WAN2</code>, <code>WAN3</code>) support <strong>DHCP</strong> alongside Static IPv4 and PPPoE. An additional DHCP WAN never hijacks your default route or system DNS servers&mdash;failover stays governed by the 1-second gateway monitor, and whenever the backup ISP changes your lease gateway, <code>stratum-pfrefresh</code> updates failover, <code>reply-to</code>, and Policy Routing rules within seconds without needing an Apply.</div></details>
    <!-- STRATUM-V0300-FAQ-END -->
  </div>
</section>

</div>

<div style="text-align:center; margin-top:64px;">
  <h3 style="font-family:var(--font-display); font-size:22px; margin-bottom:12px;">Still have questions?</h3>
  <p style="color:var(--ash); margin-bottom:24px;">Reach out — we'd rather answer them than have you guess.</p>
  <a href="/company/contact/" class="btn btn-primary">Contact us</a>
</div>
