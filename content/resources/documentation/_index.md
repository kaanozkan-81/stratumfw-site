---
title: "Documentation"
lead: "Everything you need to deploy, configure, and operate Stratum Gateway — organized by task."
full_width: true
---

<div class="docs-layout">

<div class="docs-intro">
  <p>Documentation covers the platform from first install to day-two operations. Content is organized by what you're trying to accomplish, not by which module it belongs to.</p>
</div>

<section id="getting-started" class="docs-section">
  <div class="docs-section-head">
    <span class="eyebrow">01</span>
    <h2>Getting Started</h2>
    <p>From bare hardware to a working firewall.</p>
  </div>

  <div class="card-grid grid-3">
    <div class="card">
      <h3>Installation</h3>
      <p>Prepare hardware, boot the installer, and complete the initial setup.</p>
      <a href="/resources/documentation/installation/" class="card-link">Read guide →</a>
    </div>
    <div class="card">
      <h3>Initial Configuration</h3>
      <p>Set hostname, timezone, admin account, and first network interfaces.</p>
      <a href="/resources/documentation/initial-configuration/" class="card-link">Read guide →</a>
    </div>
    <div class="card">
      <h3>Network Basics</h3>
      <p>WAN, LAN, VLANs, and how Stratum models your network topology.</p>
      <a href="/resources/documentation/network-basics/" class="card-link">Read guide →</a>
    </div>
    <div class="card">
      <h3>Panel Tour</h3>
      <p>Navigate the interface, understand the layout, and find what you need.</p>
      <a href="/resources/documentation/panel-tour/" class="card-link">Read guide →</a>
    </div>
  </div>
</section>

<section id="configuration" class="docs-section">
  <div class="docs-section-head">
    <span class="eyebrow">02</span>
    <h2>Configuration</h2>
    <p>Set up the modules that make the platform do its job.</p>
  </div>

  <div class="card-grid grid-3">
    <div class="card">
      <h3>Firewall Rules</h3>
      <p>Create, order, and apply rules. Understand objects and time windows.</p>
      <span class="card-link" style="color:var(--ash-dark);">Coming soon</span>
    </div>
    <div class="card">
      <h3>Multi-WAN</h3>
      <p>Configure failover and load balancing across multiple internet links.</p>
      <span class="card-link" style="color:var(--ash-dark);">Coming soon</span>
    </div>
    <div class="card">
      <h3>VPN (WireGuard)</h3>
      <p>Set up peers, generate configs, and provision mobile clients.</p>
      <span class="card-link" style="color:var(--ash-dark);">Coming soon</span>
    </div>
    <div class="card">
      <h3>IDS (Suricata)</h3>
      <p>Select interfaces, tune rule sets, and manage suppressions.</p>
      <span class="card-link" style="color:var(--ash-dark);">Coming soon</span>
    </div>
    <div class="card">
      <h3>Web Filtering</h3>
      <p>Enable categories, manage feeds, and configure update frequency.</p>
      <span class="card-link" style="color:var(--ash-dark);">Coming soon</span>
    </div>
    <div class="card">
      <h3>Captive Portal</h3>
      <p>Customize templates, integrate with RADIUS, and control guest access.</p>
      <span class="card-link" style="color:var(--ash-dark);">Coming soon</span>
    </div>
    <div class="card">
      <h3>QoS &amp; Traffic Shaping</h3>
      <p>Allocate bandwidth by network and prioritize critical traffic.</p>
      <span class="card-link" style="color:var(--ash-dark);">Coming soon</span>
    </div>
    <div class="card">
      <h3>NAT &amp; Port Forwarding</h3>
      <p>Expose internal services safely with destination and 1:1 NAT.</p>
      <span class="card-link" style="color:var(--ash-dark);">Coming soon</span>
    </div>
    <div class="card">
      <h3>DNS &amp; Feeds</h3>
      <p>Configure the resolver, enable RPZ feeds, and add custom sources.</p>
      <span class="card-link" style="color:var(--ash-dark);">Coming soon</span>
    </div>
  </div>
</section>

<section id="operations" class="docs-section">
  <div class="docs-section-head">
    <span class="eyebrow">03</span>
    <h2>Operations</h2>
    <p>Day-two tasks: keeping the system healthy, current, and recoverable.</p>
  </div>

  <div class="card-grid grid-3">
    <div class="card">
      <h3>Backup &amp; Restore</h3>
      <p>Create module-level backups, schedule rotation, and restore safely.</p>
      <span class="card-link" style="color:var(--ash-dark);">Coming soon</span>
    </div>
    <div class="card">
      <h3>Updates</h3>
      <p>Check for versions, apply updates, and understand the rollback behavior.</p>
      <span class="card-link" style="color:var(--ash-dark);">Coming soon</span>
    </div>
    <div class="card">
      <h3>Monitoring</h3>
      <p>SNMP, NetFlow, notifications, and how to integrate with your NMS.</p>
      <span class="card-link" style="color:var(--ash-dark);">Coming soon</span>
    </div>
    <div class="card">
      <h3>Troubleshooting</h3>
      <p>Common issues, log locations, and diagnostic commands.</p>
      <span class="card-link" style="color:var(--ash-dark);">Coming soon</span>
    </div>
  </div>
</section>

<section id="reference" class="docs-section">
  <div class="docs-section-head">
    <span class="eyebrow">04</span>
    <h2>Reference</h2>
    <p>For automation, integration, and debugging.</p>
  </div>

  <div class="card-grid grid-3">
    <div class="card">
      <h3>REST API</h3>
      <p>Every configuration surface exposed programmatically for orchestration.</p>
      <span class="card-link" style="color:var(--ash-dark);">Coming soon</span>
    </div>
    <div class="card">
      <h3>Command Line</h3>
      <p>CLI tools and utilities available on the underlying system.</p>
      <span class="card-link" style="color:var(--ash-dark);">Coming soon</span>
    </div>
    <div class="card">
      <h3>File &amp; Path Reference</h3>
      <p>Where configuration lives, where logs are stored, and what each file does.</p>
      <span class="card-link" style="color:var(--ash-dark);">Coming soon</span>
    </div>
  </div>
</section>

<section id="release-notes" class="docs-section">
  <div class="docs-section-head">
    <span class="eyebrow">05</span>
    <h2>Release Notes</h2>
    <p>Version history and what changed in each release.</p>
  </div>

  <div class="release-card release-latest">
    <div class="release-head">
      <div>
        <span class="release-version">v1.3.0</span>
        <span class="release-badge">Latest</span>
      </div>
      <span class="release-date">2026-09-12</span>
    </div>
    <div class="release-body">
      <p>A guided progress screen for Factory Reset, replacing the previous silent reboot. The interface now polls the gateway's health endpoint and automatically redirects to the login screen once the system is back online, giving operators clear visibility throughout the reset sequence.</p>
    </div>
    <div class="release-meta">
      <div class="release-meta-item">
        <strong>SHA-256</strong>
        <code>dcdb29ec365776116dd26db54f7cfc44f5ec66b64d0806f6d9b95a1263e5eefc</code>
      </div>
      <a href="https://update.stratumfw.com/packages/stratum-panel-1.3.0.tar.gz" class="release-download">
        Download package →
      </a>
    </div>
  </div>

  <h3 class="release-history-title">Earlier releases</h3>

  <div class="release-timeline">

    <div class="release-entry">
      <div class="release-entry-head">
        <span class="release-entry-version">v1.2.0</span>
        <span class="release-entry-date">Update Mechanism: First Live Validation</span>
      </div>
      <ul class="release-entry-list">
        <li>First end-to-end real-world test of the download-and-apply update pipeline (v1.1.0 → v1.2.0)</li>
        <li>Verified via a visible on-screen marker confirming the new code was actually running after the update</li>
        <li>Completed successfully with no errors</li>
      </ul>
    </div>

    <div class="release-entry">
      <div class="release-entry-head">
        <span class="release-entry-version">v1.1.0</span>
        <span class="release-entry-date">Critical WAN Fix &amp; Factory Reset</span>
      </div>
      <ul class="release-entry-list">
        <li>Root-caused and fixed the WAN assignment "hang": applying a WAN role restarted <code>systemd-networkd</code> wholesale, briefly cutting the panel's own connection. Fixed by switching to a targeted <code>networkctl reload</code>.</li>
        <li>Fixed CrowdSec firewall bouncer never completing real registration on first boot</li>
        <li>Added the Factory Reset feature: restores nftables, DNS, Suricata, and SNMP to clean templates; wipes generated secrets, VPN config, and interface roles; reboots into first-boot</li>
        <li>Built the first real download-and-apply Software Update mechanism: HTTPS-only manifest fetch, mandatory SHA-256 verification, automatic backup of the current panel before applying, atomic file replacement with rollback on failure, and a self-restart sequence designed to avoid self-inflicted deadlocks</li>
      </ul>
    </div>

    <div class="release-entry">
      <div class="release-entry-head">
        <span class="release-entry-version">v1.0.0</span>
        <span class="release-entry-date">Installable Product: ISO, Website &amp; Backup/Restore</span>
      </div>
      <ul class="release-entry-list">
        <li>Preseed-based Debian installer ISO with a first-boot provisioning pipeline: network bootstrap, secrets/CA/TLS generation, service startup</li>
        <li>Public corporate website and software-update server migrated to a permanent host (<code>stratumfw.com</code> / <code>update.stratumfw.com</code>) with real TLS certificates</li>
        <li>Interactive product demo pages (Dashboard, VPN, Web Filter, Geo-blocking)</li>
        <li>Backup/restore system with fixes for a <code>pipefail</code> bug that rejected valid backups, an audit-middleware bug that corrupted large file uploads, and a systemd self-kill deadlock during restore</li>
        <li>Restore progress page with live status polling</li>
        <li>Extensive VirtualBox ISO testing, surfacing and fixing over a dozen first-boot and provisioning issues: PPPoE/DHCP network file naming, VPN peer file format, empty nftables set syntax, CrowdSec/networkd-dispatcher/NetFlow services not starting on first boot</li>
      </ul>
    </div>

    <div class="release-entry">
      <div class="release-entry-head">
        <span class="release-entry-version">v0.7.0</span>
        <span class="release-entry-date">VPN, Governance &amp; Update Groundwork</span>
      </div>
      <ul class="release-entry-list">
        <li>QoS v2: per-interface CAKE architecture rework</li>
        <li>User management, audit logging, backup/restore, and general settings pages</li>
        <li>Country-based IP blocking</li>
        <li>WireGuard VPN server</li>
        <li>Safe Search enforcement for web filtering</li>
        <li>First version of the Software Update server (<code>update.stratumfw.com</code>) and update-check UI</li>
        <li>Web-based terminal (xterm.js)</li>
        <li>Customizable dashboard widgets</li>
        <li>Fixed a Grafana crash-loop</li>
        <li>Configuration migration analysis for common firewall platforms (106 aliases, 103 rules, 31 NAT rules mapped)</li>
        <li>Initial ISO / unattended-installer planning</li>
      </ul>
    </div>

    <div class="release-entry">
      <div class="release-entry-head">
        <span class="release-entry-version">v0.6.0</span>
        <span class="release-entry-date">Threat Detection &amp; Traffic Control</span>
      </div>
      <ul class="release-entry-list">
        <li>CrowdSec panel (collaborative IPS)</li>
        <li>Suricata IDS panel, including crash-loop fix and a suppress/whitelist workflow</li>
        <li>SNMP panel and NetFlow panel</li>
        <li>Multi-WAN Load Balancing (ECMP)</li>
        <li>Traffic Shaping / QoS built on HTB + CAKE</li>
        <li>Fixed persistence issues in <code>nic-configure</code>, <code>nat-rules-apply</code>, <code>wan-monitor.sh</code>, and <code>filter.nft</code></li>
      </ul>
    </div>

    <div class="release-entry">
      <div class="release-entry-head">
        <span class="release-entry-version">v0.5.0</span>
        <span class="release-entry-date">Multi-WAN &amp; Physical Interface Management</span>
      </div>
      <ul class="release-entry-list">
        <li>Physical interface configuration system: OPT ports, free custom naming</li>
        <li>WAN connection modes: DHCP, Static IP, PPPoE, and Reserved Networks</li>
        <li>Default-deny security policy hardening</li>
        <li>Interface role tracking (<code>interface-roles.conf</code>)</li>
        <li>Multi-WAN failover monitoring (<code>wan-monitor.sh</code>)</li>
        <li>Services management page</li>
        <li>Fixed nftables.conf structural corruption, a CrowdSec crash-loop, and a dashboard performance regression</li>
      </ul>
    </div>

    <div class="release-entry">
      <div class="release-entry-head">
        <span class="release-entry-version">v0.4.0</span>
        <span class="release-entry-date">Security Hardening &amp; VLANs</span>
      </div>
      <ul class="release-entry-list">
        <li>DNSBL feed system with per-VLAN scoping</li>
        <li>Fixed a delimiter-injection vulnerability in rule and object handling</li>
        <li>VLAN creation and management UI</li>
        <li>Interface configuration improvements</li>
      </ul>
    </div>

    <div class="release-entry">
      <div class="release-entry-head">
        <span class="release-entry-version">v0.3.0</span>
        <span class="release-entry-date">Panel Core Features</span>
      </div>
      <ul class="release-entry-list">
        <li>Dashboard widgets</li>
        <li>NAT, Rules, Interfaces, Web Filter, and Objects pages</li>
        <li>Custom firewall rule engine (<code>fw-rule</code> CLI tool) with live nftables generation</li>
        <li>Aliases / Objects system (<code>fw-object</code> CLI tool) for reusable address and port groups</li>
      </ul>
    </div>

    <div class="release-entry">
      <div class="release-entry-head">
        <span class="release-entry-version">v0.2.0</span>
        <span class="release-entry-date">Web Panel Foundation</span>
      </div>
      <ul class="release-entry-list">
        <li>FastAPI + Jinja2 + vanilla JS admin panel scaffolded</li>
        <li>Authentication with bcrypt-hashed credentials</li>
        <li>Initial dashboard layout and design system</li>
        <li>SNMP, NetFlow, and Loki/Grafana monitoring stack wired in</li>
        <li>Granular NAT rule control</li>
      </ul>
    </div>

    <div class="release-entry">
      <div class="release-entry-head">
        <span class="release-entry-version">v0.1.0</span>
        <span class="release-entry-date">Core Infrastructure</span>
      </div>
      <ul class="release-entry-list">
        <li>Network infrastructure: physical interface roles (WAN/LAN/OPT), VLAN trunking, <code>systemd-networkd</code> configuration</li>
        <li>nftables firewall core: sets, objects, filter and NAT tables built from scratch</li>
        <li>DHCP (Kea) and DNS (Unbound) services</li>
        <li>QoS foundation (traffic shaping groundwork)</li>
        <li>IDS/IPS integration (Suricata)</li>
        <li>Category-based web filtering foundation (RPZ-style DNS blocklists)</li>
      </ul>
    </div>

  </div>

  <div class="docs-update-box">
    <div>
      <h4>Update mechanism</h4>
      <p>Stratum Gateway checks for updates against a signed manifest. Every package is cryptographically verified before it is applied, and a failed update rolls back automatically.</p>
    </div>
  </div>

  <h3 class="release-history-title" style="margin-top:48px;">Current manifest</h3>
  <p style="color:var(--ash); font-size:14.5px; margin-bottom:20px;">The update server publishes a signed manifest describing the latest available release. The panel fetches this on demand and verifies every field before applying an update.</p>

  <div class="manifest-card">
    <div class="manifest-head">
      <div class="manifest-title">
        <span class="manifest-dot"></span>
        <code>manifest.json</code>
      </div>
      <span class="manifest-source">update.stratumfw.com</span>
    </div>
    <dl class="manifest-fields">
      <div class="manifest-row">
        <dt>latest_version</dt>
        <dd><span class="manifest-tag">1.3.0</span></dd>
      </div>
      <div class="manifest-row">
        <dt>released_at</dt>
        <dd>2026-09-12</dd>
      </div>
      <div class="manifest-row">
        <dt>package_url</dt>
        <dd><a href="https://update.stratumfw.com/packages/stratum-panel-1.3.0.tar.gz">packages/stratum-panel-1.3.0.tar.gz</a></dd>
      </div>
      <div class="manifest-row">
        <dt>sha256</dt>
        <dd><code class="manifest-hash">dcdb29ec365776116dd26db54f7cfc44f5ec66b64d0806f6d9b95a1263e5eefc</code></dd>
      </div>
      <div class="manifest-row">
        <dt>changelog</dt>
        <dd>Guided progress screen for Factory Reset with automatic redirect to login once the gateway is back online.</dd>
      </div>
    </dl>
  </div>

</section>

<section class="docs-contact">
  <h3>Can't find what you need?</h3>
  <p>Documentation is a work in progress. If there's a topic you'd like to see covered, tell us — it directly shapes what gets written next.</p>
  <a href="/company/contact/" class="btn btn-primary">Contact us</a>
</section>

</div>
