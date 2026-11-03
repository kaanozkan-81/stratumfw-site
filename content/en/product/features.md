---
title: "Features"
lead: "Every capability you need to secure, manage, and monitor a modern network — without the bloat."
full_width: true
---

<div class="features-layout">

<aside class="features-sidebar">
  <nav class="features-nav">
    <h4>Categories</h4>
    <ul>
      <li><a href="#networking">Network &amp; Routing</a></li>
      <li><a href="#security">Security &amp; Threat Intel</a></li>
      <li><a href="#vpn">VPN &amp; Access Control</a></li>
      <li><a href="#management">Management &amp; API</a></li>
    </ul>
  </nav>
</aside>

<div class="features-content">

<section id="networking" class="feature-category">
  <span class="eyebrow">Category 01</span>
  <h2>Network &amp; Routing</h2>
  <p class="category-lead">Powered by FreeBSD and pf, the most battle-tested network stack in the world.</p>

  <article class="feature-item">
    <h3>pf Packet Filter</h3>
    <p class="feature-problem"><strong>The problem:</strong> Userspace firewalls can't handle high throughput and add unnecessary latency to packets.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> We use FreeBSD's native pf. All stateful inspection, NAT, and routing happen directly inside the kernel, ensuring wire-speed performance even under heavy load.</p>
    <ul class="feature-tech">
      <li>Stateful Packet Inspection (SPI)</li>
      <li>Destination NAT (Port Forwarding) and 1:1 NAT</li>
      <li>Traffic shaping and prioritization (ALTQ)</li>
      <li>Real-time state and rule logging</li>
    </ul>
  </article>

  <article class="feature-item">
    <h3>Multi-WAN &amp; Failover</h3>
    <p class="feature-problem"><strong>The problem:</strong> A single internet link is a single point of failure. Downtime costs money.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Configure multiple WAN interfaces and let Stratum handle the rest. Health-checked failover automatically redirects traffic if your primary link drops.</p>
    <ul class="feature-tech">
      <li>Multiple WAN interfaces (DHCP, Static, PPPoE)</li>
      <li>Configurable gateway groups and failover policies</li>
      <li>Automatic recovery when the primary link returns</li>
    </ul>
  </article>

  <article class="feature-item">
    <h3>VLANs &amp; Bridges</h3>
    <p class="feature-problem"><strong>The problem:</strong> You need to segment traffic (Guest, IoT, Staff) without buying more hardware.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Native support for 802.1Q VLAN tagging and software bridging. Segment your network logically and assign firewall rules per VLAN.</p>
    <ul class="feature-tech">
      <li>802.1Q VLAN support on any physical interface</li>
      <li>Link aggregation (LAGG) support</li>
      <li>Software bridging (if_bridge)</li>
    </ul>
  </article>

  <article class="feature-item">
    <h3>DNS &amp; DHCP Services</h3>
    <p class="feature-problem"><strong>The problem:</strong> Managing separate DHCP and DNS servers for a small network is overkill and prone to errors.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Integrated DHCP (dnsmasq) and DNS resolver (Unbound). Manage static mappings, DNS overrides, and DHCP pools directly from the panel.</p>
    <ul class="feature-tech">
      <li>Unbound with DNSSEC validation</li>
      <li>dnsmasq for DHCP and local DNS</li>
      <li>Custom DNS overrides and host records</li>
    </ul>
  </article>
</section>

<section id="security" class="feature-category">
  <span class="eyebrow">Category 02</span>
  <h2>Security &amp; Threat Intel</h2>
  <p class="category-lead">Layered defense, from the packet level to the DNS layer.</p>

  <article class="feature-item">
    <h3>IP Blocklists (IPv4/IPv6)</h3>
    <p class="feature-problem"><strong>The problem:</strong> Manually blocking malicious IPs is impossible. Threat feeds update every minute.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Ingest public threat feeds directly into pf tables. Stratum automatically downloads, parses, and applies blocklists so you drop malicious traffic at the perimeter.</p>
    <ul class="feature-tech">
      <li>Import standard CIDR and IP lists</li>
      <li>Dynamic pf table updates (no full reload)</li>
      <li>Separate inbound and outbound blocking rules</li>
      <li>Feed status and hit counters</li>
    </ul>
  </article>

  <article class="feature-item">
    <h3>DNSBL (DNS Blocking)</h3>
    <p class="feature-problem"><strong>The problem:</strong> Ads, trackers, and malware domains slow down your network and pose a security risk.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Enable DNSBL to block domains at the resolver level. If a device tries to resolve a malicious domain, it gets an NXDOMAIN response instantly.</p>
    <ul class="feature-tech">
      <li>RPZ (Response Policy Zones) via Unbound</li>
      <li>Custom allow and block lists</li>
      <li>Scheduled feed updates</li>
    </ul>
  </article>

  <article class="feature-item">
    <h3>Safe-Apply Workflow</h3>
    <p class="feature-problem"><strong>The problem:</strong> One bad firewall rule can lock you out of your own server.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Every change is validated. If you change a WAN IP or firewall rule, Stratum gives you a timeout to confirm connectivity. If you don't, it rolls back automatically.</p>
    <ul class="feature-tech">
      <li>Pre-flight syntax checking (<code>pfctl -nf</code>)</li>
      <li>Atomic configuration replacement</li>
      <li>Automatic rollback on connection loss</li>
    </ul>
  </article>
</section>

<section id="vpn" class="feature-category">
  <span class="eyebrow">Category 03</span>
  <h2>VPN &amp; Access Control</h2>
  <p class="category-lead">Secure remote access and strict authorization.</p>

  <article class="feature-item">
    <h3>WireGuard VPN</h3>
    <p class="feature-problem"><strong>The problem:</strong> Traditional VPNs are heavy, hard to configure, and slow.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Fully integrated WireGuard. Stratum implements X25519 cryptography internally, meaning zero external dependencies. Generate peer configurations and QR codes in seconds.</p>
    <ul class="feature-tech">
      <li>Zero-dependency X25519 cryptography</li>
      <li>QR code generation for mobile devices</li>
      <li>Preshared Keys (PSK) for extra security</li>
      <li>Per-peer allowed IPs and routing</li>
    </ul>
  </article>

  <article class="feature-item">
    <h3>Role-Based Access Control (RBAC)</h3>
    <p class="feature-problem"><strong>The problem:</strong> Handing out the admin password to everyone is a security risk.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> Define exactly who can do what. Every API endpoint and UI page enforces strict role permissions.</p>
    <ul class="feature-tech">
      <li><b>Admin:</b> Full access to all configuration and users</li>
      <li><b>Operator:</b> Can manage states, run diagnostics, view logs</li>
      <li><b>Read-Only:</b> Can only view dashboards and metrics</li>
    </ul>
  </article>
</section>

<section id="management" class="feature-category">
  <span class="eyebrow">Category 04</span>
  <h2>Management &amp; API</h2>
  <p class="category-lead">Automate everything. The UI is just a client.</p>

  <article class="feature-item">
    <h3>Declarative Config Engine</h3>
    <p class="feature-problem"><strong>The problem:</strong> Firewall configs are often messy and hard to audit.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> The entire system state lives in a single <code>config.json</code>. The Python engine translates this JSON into native FreeBSD files.</p>
    <ul class="feature-tech">
      <li>Single source of truth</li>
      <li>Human-readable generated files</li>
      <li>Easy to back up and restore</li>
    </ul>
  </article>

  <article class="feature-item">
    <h3>REST API</h3>
    <p class="feature-problem"><strong>The problem:</strong> Manual clicking doesn't scale.</p>
    <p class="feature-solution"><strong>How Stratum solves it:</strong> The FastAPI backend exposes every configuration surface via a REST API. Build custom dashboards, automate deployments, or integrate with your existing CI/CD.</p>
    <ul class="feature-tech">
      <li>JSON-based endpoints</li>
      <li>Session and Token-based authentication</li>
      <li>Perfect for Ansible / Terraform</li>
    </ul>
  </article>
</section>

</div>
</div>
