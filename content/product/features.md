---
title: "Features"
lead: "Twenty-six integrated modules covering every layer of your network — from physical interfaces to threat intelligence, all managed from one panel."
full_width: true
---

<div class="features-layout">

<aside class="features-sidebar">
  <nav class="features-nav">
    <h4>Categories</h4>
    <ul>
      <li><a href="#network">Network &amp; Connectivity</a></li>
      <li><a href="#security">Security &amp; Threat Protection</a></li>
      <li><a href="#access">Access &amp; Identity</a></li>
      <li><a href="#traffic">Traffic Control</a></li>
      <li><a href="#management">Management &amp; Operations</a></li>
      <li><a href="#platform">Platform</a></li>
    </ul>
  </nav>
</aside>

<div class="features-content">

<section id="network" class="feature-category">
  <span class="eyebrow">Category 01</span>
  <h2>Network &amp; Connectivity</h2>
  <p class="category-lead">Everything that keeps packets flowing — from physical interfaces to routing, addressing, and name resolution.</p>

  <article id="interfaces" class="feature-item">
    <h3>Interface Management</h3>
    <p class="feature-problem"><strong>The problem:</strong> Physical and virtual interfaces change constantly — new VLANs, bonding, MTU tuning — and each change risks breaking the network.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> A unified interface manager with safe-apply logic. Every change is validated, staged, and only committed after you confirm connectivity. If a change would cut you off, Stratum rolls it back automatically.</p>
    <ul class="feature-tech">
      <li>Physical, VLAN, bridge, bond, and PPPoE interfaces</li>
      <li>Per-interface MTU, MAC, and offload tuning</li>
      <li>Safe-apply with automatic rollback on connectivity loss</li>
      <li>Real-time link state and traffic counters</li>
    </ul>
  </article>

  <article id="multi-wan" class="feature-item">
    <h3>Multi-WAN Failover &amp; Load Balancing</h3>
    <p class="feature-problem"><strong>The problem:</strong> A single internet link is a single point of failure. Backup links that sit idle waste money; manual failover wastes time.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Health-checked failover and per-flow ECMP load balancing across multiple WAN connections. If a link degrades or drops, traffic shifts within seconds — sessions stay alive, users never notice.</p>
    <ul class="feature-tech">
      <li>Configurable health checks (ICMP, HTTP, DNS)</li>
      <li>Weighted and equal-cost load balancing</li>
      <li>Per-flow and per-packet distribution modes</li>
      <li>Automatic failback when the primary link recovers</li>
    </ul>
  </article>

  <article id="dhcp" class="feature-item">
    <h3>DHCP Server</h3>
    <p class="feature-problem"><strong>The problem:</strong> IP address management that lives in a spreadsheet becomes stale within weeks.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> A built-in DHCP server with pool management, static reservations, and one-click conversion of any active lease into a reservation.</p>
    <ul class="feature-tech">
      <li>Multiple pools per interface</li>
      <li>Static reservations keyed by MAC address</li>
      <li>Convert active lease → reservation in one click</li>
      <li>Live lease table with expiry tracking</li>
    </ul>
  </article>

  <article id="dns" class="feature-item">
    <h3>DNS Resolver with Feed Catalog</h3>
    <p class="feature-problem"><strong>The problem:</strong> DNS is the first place malware calls home — but blocking it without breaking legitimate traffic is a delicate balance.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> An Unbound-based resolver extended with RPZ (Response Policy Zones) and a curated feed catalog. Enable category-based blocking by country, threat type, or content category — feeds update automatically.</p>
    <ul class="feature-tech">
      <li>Unbound resolver with DNSSEC validation</li>
      <li>RPZ-based policy enforcement</li>
      <li>Feed catalog with dozens of curated sources</li>
      <li>Per-feed scope, toggling, and manual updates</li>
      <li>Custom feed support (upload your own blocklists)</li>
    </ul>
  </article>

  <article id="nat" class="feature-item">
    <h3>NAT — Port Forwarding &amp; 1:1</h3>
    <p class="feature-problem"><strong>The problem:</strong> Exposing internal services requires careful NAT rules — and a single mistake opens more than intended.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> A visual NAT manager covering both destination NAT (port forwarding) and static 1:1 NAT (full IP mapping). Rules can be toggled without deletion, making temporary exposure trivial.</p>
    <ul class="feature-tech">
      <li>Destination NAT (port forwarding) with protocol selection</li>
      <li>Static 1:1 NAT for full IP mapping</li>
      <li>Rule toggling without losing configuration</li>
      <li>Conflict detection between overlapping rules</li>
    </ul>
  </article>

</section>

</div>
</div>

<section id="security" class="feature-category">
  <span class="eyebrow">Category 02</span>
  <h2>Security &amp; Threat Protection</h2>
  <p class="category-lead">Layered defense that catches what a single tool would miss — from signature-based detection to behavioral analysis and geographic filtering.</p>

  <article id="ids" class="feature-item">
    <h3>Suricata IDS</h3>
    <p class="feature-problem"><strong>The problem:</strong> Signature-based intrusion detection generates noise. Too many alerts, and analysts stop looking.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Suricata IDS runs on your WAN interfaces with a tuned ruleset and per-signature suppression. You see what matters, silence what doesn't, and adjust monitoring scope per interface.</p>
    <ul class="feature-tech">
      <li>Suricata 7.x with ET Open + custom rules</li>
      <li>Per-interface monitoring selection</li>
      <li>Signature suppression without disabling entire rulesets</li>
      <li>Alert log with search and export</li>
      <li>Threshold tuning for noisy signatures</li>
    </ul>
  </article>

  <article id="crowdsec" class="feature-item">
    <h3>CrowdSec Behavioral Threat Engine</h3>
    <p class="feature-problem"><strong>The problem:</strong> Signatures only catch known attacks. Zero-day and novel behavior slip through.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> CrowdSec watches live behavior — SSH brute force, HTTP scanning, port sweeps — and applies decisions via a firewall bouncer. When a decision fires, the IP is banned at the kernel level instantly. Community threat intelligence extends coverage beyond what you'd see alone.</p>
    <ul class="feature-tech">
      <li>Behavioral scenarios (SSH, HTTP, probes, etc.)</li>
      <li>Firewall bouncer integration (nftables)</li>
      <li>Manual ban / unban from the panel</li>
      <li>Community blocklist integration (CrowdSec Cloud)</li>
      <li>Simulation mode for pre-deployment testing</li>
    </ul>
  </article>

  <article id="geoblock" class="feature-item">
    <h3>Geo &amp; Threat IP Blocking</h3>
    <p class="feature-problem"><strong>The problem:</strong> If you don't do business in Country X, why accept any traffic from there?</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Country-level blocking based on MaxMind GeoIP databases, plus a continuously refreshed threat IP list aggregated from public intelligence feeds. Both are managed from one panel with one-click updates.</p>
    <ul class="feature-tech">
      <li>Country blocking (all, or exception-based)</li>
      <li>Curated threat IP feed with scheduled updates</li>
      <li>Manual IP / CIDR add and remove</li>
      <li>Update status and last-refresh visibility</li>
    </ul>
  </article>

  <article id="webfilter" class="feature-item">
    <h3>Web Filtering (DNS-based)</h3>
    <p class="feature-problem"><strong>The problem:</strong> You can't inspect every packet, but you can control what names resolve.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Category-based filtering at the DNS layer. Ads, malware, adult content, phishing — enabled in bulk or per-category. Feeds update on a schedule you choose, and you can push a manual update any time.</p>
    <ul class="feature-tech">
      <li>Category-based blocklists (dozens of categories)</li>
      <li>Automatic and manual feed updates</li>
      <li>Configurable update frequency</li>
      <li>Per-feed scope and toggling</li>
      <li>Custom blocklist uploads</li>
    </ul>
  </article>

  <article id="rules" class="feature-item">
    <h3>Firewall Rules &amp; Objects</h3>
    <p class="feature-problem"><strong>The problem:</strong> Firewall rules scattered across scripts, iptables dumps, and documentation nobody maintains.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> A structured rule manager with reusable objects (address groups, service groups, time windows). Rules are visual, reorderable, toggleable, and applied atomically.</p>
    <ul class="feature-tech">
      <li>Custom rule editor with drag-reorder</li>
      <li>Reusable address, service, and time objects</li>
      <li>MAC-based rules for device-level control</li>
      <li>IP and MAC blocklists integrated</li>
      <li>Rule hit counters and match logs</li>
    </ul>
  </article>

</section>

<section id="access" class="feature-category">
  <span class="eyebrow">Category 03</span>
  <h2>Access &amp; Identity</h2>
  <p class="category-lead">How people and devices get onto your network — securely, and only when they should.</p>

  <article id="vpn" class="feature-item">
    <h3>WireGuard VPN</h3>
    <p class="feature-problem"><strong>The problem:</strong> Traditional VPNs are slow to set up and hard to distribute to end users.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> WireGuard peer management with one-click client provisioning. Every peer generates a QR code — scan it on a phone, import it on a laptop. The tunnel is up in seconds.</p>
    <ul class="feature-tech">
      <li>WireGuard peers with public/private key management</li>
      <li>QR code generation for mobile clients</li>
      <li>Downloadable client configs</li>
      <li>Per-peer allowed IPs and routing control</li>
      <li>Live peer status (handshake, traffic)</li>
    </ul>
  </article>

  <article id="captive-portal" class="feature-item">
    <h3>Captive Portal</h3>
    <p class="feature-problem"><strong>The problem:</strong> Guest networks shouldn't be open. But forcing guests through a VPN or a complex sign-up is a non-starter.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> A customizable captive portal that intercepts new devices, presents an authentication page (or click-through terms), and grants network access. Templates are customizable and can be swapped without touching code.</p>
    <ul class="feature-tech">
      <li>Authentication page with customizable templates</li>
      <li>Template upload / removal from the panel</li>
      <li>Preview mode before publishing</li>
      <li>Optional RADIUS backend for credentials</li>
      <li>Per-user session tracking</li>
    </ul>
  </article>

  <article id="radius" class="feature-item">
    <h3>RADIUS Authentication</h3>
    <p class="feature-problem"><strong>The problem:</strong> Enterprise Wi-Fi and VPN access need centralized identity — not shared passwords.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> RADIUS integration for 802.1X and captive portal authentication. Multiple servers, per-server toggling, and built-in connectivity testing.</p>
    <ul class="feature-tech">
      <li>Multiple RADIUS server support</li>
      <li>Per-server enable / disable</li>
      <li>Connection testing from the panel</li>
      <li>Integration with captive portal and Wi-Fi</li>
    </ul>
  </article>

</section>

<section id="traffic" class="feature-category">
  <span class="eyebrow">Category 04</span>
  <h2>Traffic Control &amp; Visibility</h2>
  <p class="category-lead">Know what's on the wire, and shape it so the important traffic always gets through.</p>

  <article id="qos" class="feature-item">
    <h3>QoS &amp; Traffic Shaping</h3>
    <p class="feature-problem"><strong>The problem:</strong> One user streaming 4K starves the VoIP calls and the CRM sync.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Fair-queuing bandwidth management with per-network allocation. Critical traffic is prioritized; everything else shares remaining bandwidth fairly. Changes apply atomically and can be disabled with one click.</p>
    <ul class="feature-tech">
      <li>Fair-queuing (fq_codel) and class-based shaping</li>
      <li>Per-network and per-interface policies</li>
      <li>Upload / download asymmetric handling</li>
      <li>One-click enable / disable</li>
      <li>Live traffic graphs per class</li>
    </ul>
  </article>

  <article id="netflow" class="feature-item">
    <h3>NetFlow Analysis</h3>
    <p class="feature-problem"><strong>The problem:</strong> "The internet is slow" — but which host, which protocol, which destination?</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> NetFlow export gives you flow-level visibility into every conversation on your network. Top talkers, top protocols, top destinations — all without packet capture overhead.</p>
    <ul class="feature-tech">
      <li>NetFlow v5/v9 export</li>
      <li>Per-interface flow collection</li>
      <li>Top talkers and protocol breakdowns</li>
      <li>Export to external collectors (nfdump, Elastic, etc.)</li>
    </ul>
  </article>

</section>

<section id="management" class="feature-category">
  <span class="eyebrow">Category 05</span>
  <h2>Management &amp; Operations</h2>
  <p class="category-lead">Day-to-day tools for the people who run the network — from a unified dashboard to backup, updates, and remote administration.</p>

  <article id="management" class="feature-item">
    <h3>Unified Web Dashboard</h3>
    <p class="feature-problem"><strong>The problem:</strong> Monitoring a dozen tools means a dozen browser tabs — and none of them talk to each other.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> A single web panel with a customizable widget-based dashboard. CPU, memory, interfaces, active threats, VPN peers, DHCP leases — arrange what you care about, hide what you don't.</p>
    <ul class="feature-tech">
      <li>Drag-and-arrange dashboard widgets</li>
      <li>Per-user dashboard layouts</li>
      <li>Real-time system metrics</li>
      <li>Consistent UI across all 26 modules</li>
    </ul>
  </article>

  <article id="backup" class="feature-item">
    <h3>Backup &amp; Restore</h3>
    <p class="feature-problem"><strong>The problem:</strong> Full-system backups take hours. Restoring one wrong change shouldn't mean restoring everything.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Module-level backup and restore. Snapshot just the firewall rules, just the VPN config, just the QoS policies — and roll back that one piece without touching the rest. Full-system backup is still there when you need it.</p>
    <ul class="feature-tech">
      <li>Per-module backup snapshots</li>
      <li>Full-system backup</li>
      <li>Downloadable backups from the panel</li>
      <li>Restore with progress tracking</li>
      <li>Scheduled backup rotation</li>
    </ul>
  </article>

  <article id="update" class="feature-item">
    <h3>Built-in Update Mechanism</h3>
    <p class="feature-problem"><strong>The problem:</strong> Security patches arrive weekly. Manually updating a firewall means downtime and risk.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> A signed update channel that checks for new versions, validates the manifest, and applies updates with automatic rollback on failure. You control when — the system handles how.</p>
    <ul class="feature-tech">
      <li>Signed manifest from a central update server</li>
      <li>Version checks on demand or on schedule</li>
      <li>Apply updates from the panel with progress tracking</li>
      <li>Automatic rollback on failure</li>
      <li>Current version visibility (v1.3.0 and beyond)</li>
    </ul>
  </article>

  <article id="notifications" class="feature-item">
    <h3>Notifications &amp; Alerts</h3>
    <p class="feature-problem"><strong>The problem:</strong> You shouldn't have to log in to learn that something's wrong.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Configurable notifications for critical events — failed updates, new bans, interface down, high resource use. Delivered through your choice of channels.</p>
    <ul class="feature-tech">
      <li>Event-driven notifications</li>
      <li>Test notification from the panel</li>
      <li>Per-event subscription control</li>
      <li>Configurable delivery channels</li>
    </ul>
  </article>

  <article id="snmp" class="feature-item">
    <h3>SNMP Monitoring</h3>
    <p class="feature-problem"><strong>The problem:</strong> Your NMS (Zabbix, PRTG, LibreNMS) needs to see the firewall as just another device.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> SNMP v2c/v3 support with per-user credentials, so the firewall slots cleanly into existing monitoring infrastructure.</p>
    <ul class="feature-tech">
      <li>SNMP v2c and v3</li>
      <li>Multiple SNMP users with different access levels</li>
      <li>Standard MIBs (interfaces, system, resources)</li>
      <li>Per-user add / delete from the panel</li>
    </ul>
  </article>

  <article id="terminal" class="feature-item">
    <h3>Web Terminal</h3>
    <p class="feature-problem"><strong>The problem:</strong> Sometimes the fastest fix is a command — but SSH isn't always available.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> A browser-based terminal for full shell access, authenticated through the same panel login. Useful for advanced troubleshooting when you're nowhere near an SSH client.</p>
    <ul class="feature-tech">
      <li>Full shell access from the browser</li>
      <li>Session-scoped, authenticated via panel login</li>
      <li>Useful for recovery and advanced debugging</li>
    </ul>
  </article>

</section>

<section id="platform" class="feature-category">
  <span class="eyebrow">Category 06</span>
  <h2>Platform &amp; Foundation</h2>
  <p class="category-lead">The base layer that everything else stands on — operating system, cryptography, identity, and lifecycle.</p>

  <article id="platform-os" class="feature-item">
    <h3>Debian-based Platform</h3>
    <p class="feature-problem"><strong>The problem:</strong> Proprietary firewall OSes lock you into vendor hardware and closed upgrade cycles.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Debian 13 (trixie) with a modern LTS kernel. Standard tooling, standard packages, standard knowledge — your team already knows how to administer it. Runs on bare metal, VMs, or cloud instances.</p>
    <ul class="feature-tech">
      <li>Debian 13 (trixie) with Linux kernel 6.12 LTS</li>
      <li>Runs on bare metal, VM (KVM, VirtualBox, VMware), or cloud</li>
      <li>Standard Debian package management for extensions</li>
      <li>Open source — inspect, modify, self-support</li>
    </ul>
  </article>

  <article id="ca" class="feature-item">
    <h3>Built-in Certificate Authority</h3>
    <p class="feature-problem"><strong>The problem:</strong> Internal services need TLS — but running your own PKI used to mean a dedicated project.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> A built-in CA for issuing and signing internal certificates. Generate a root, sign service certificates, download the CA bundle for distribution. No external PKI required.</p>
    <ul class="feature-tech">
      <li>Create a local root CA</li>
      <li>Sign certificates with the CA</li>
      <li>Self-signed certificate generation</li>
      <li>CA bundle download for client distribution</li>
    </ul>
  </article>

  <article id="users" class="feature-item">
    <h3>Panel User Management</h3>
    <p class="feature-problem"><strong>The problem:</strong> A shared admin password is an audit nightmare.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Per-user panel accounts with password management, reset flows, and the ability to hand off administration without sharing credentials.</p>
    <ul class="feature-tech">
      <li>Multiple panel users</li>
      <li>Password change and reset from the panel</li>
      <li>Session management with signed cookies</li>
      <li>Audit-friendly separation of accounts</li>
    </ul>
  </article>

  <article id="boot" class="feature-item">
    <h3>BIOS &amp; UEFI Boot</h3>
    <p class="feature-problem"><strong>The problem:</strong> Modern servers often ship with UEFI-only firmware, while older hardware requires legacy BIOS. An installer tied to one mode locks out half the deployment targets.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> The installer supports both legacy BIOS and UEFI boot out of the box. No firmware reconfiguration, no manual CSM adjustments — insert the media, boot, and the installation proceeds.</p>
    <ul class="feature-tech">
      <li>Legacy BIOS and UEFI boot both supported</li>
      <li>Automatic detection of firmware mode during installation</li>
      <li>Compatible with bare metal, virtual machines, and cloud instances</li>
      <li>No Secure Boot configuration changes required</li>
    </ul>
  </article>

  <article id="console" class="feature-item">
    <h3>VGA &amp; Serial Console Variants</h3>
    <p class="feature-problem"><strong>The problem:</strong> Headless servers in data centers are reached through serial-over-LAN (IPMI, iDRAC, iLO), while workstations and small offices use standard displays. One installer can't serve both cleanly.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Two separate installer images ship with every release — one that outputs to a VGA console, one that outputs to a serial console. Both install the identical gateway. You pick the one that matches how you'll reach the machine.</p>
    <ul class="feature-tech">
      <li>VGA installer for physical displays and desktops</li>
      <li>Serial installer for IPMI, iDRAC, iLO, and other SOL environments</li>
      <li>Identical gateway after installation — only the console output differs</li>
      <li>Both images published with matching SHA-256 hashes</li>
    </ul>
  </article>

  <article id="first-login" class="feature-item">
    <h3>Mandatory Password Change on First Login</h3>
    <p class="feature-problem"><strong>The problem:</strong> Default credentials that ship with an installer are a known attack vector. Most products warn users to change them — very few enforce it.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> The initial admin credentials follow a documented, predictable convention. On first login, the panel enforces a password change before any other function becomes available — no configuration, no dashboard, no routing changes until the new password is set.</p>
    <ul class="feature-tech">
      <li>Predictable first-login credentials, documented per release</li>
      <li>Panel locked until password change is completed</li>
      <li>Enforced client-side and server-side</li>
      <li>Same policy applied to the SSH account</li>
    </ul>
  </article>

  <article id="settings" class="feature-item">
    <h3>System Settings</h3>
    <p class="feature-problem"><strong>The problem:</strong> Basic system administration shouldn't require a separate SSH session.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Hostname, timezone, power controls (reboot, shutdown), and factory reset — all from the panel. Factory reset wipes configuration back to defaults, but data is retained through a staged, confirmable process.</p>
    <ul class="feature-tech">
      <li>Hostname and timezone configuration</li>
      <li>Reboot / shutdown from the panel</li>
      <li>Factory reset with progress tracking</li>
      <li>Safe operations — every destructive action is staged and confirmed</li>
    </ul>
  </article>

</section>

</div>
</div>
