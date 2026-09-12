---
title: "Architecture"
lead: "A modern, standards-based platform — every layer chosen for reliability, auditability, and long-term maintainability."
---

## Design principles

Stratum Gateway is built on three principles that shape every decision in the codebase:

<div class="card-grid grid-3" style="margin-top:32px;">

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M10 2v16M4 6l6-4 6 4M4 14l6 4 6-4"/></svg></div>
    <h3>Standards first</h3>
    <p>Every component is a well-known open-source tool — Suricata, CrowdSec, Unbound, WireGuard, nftables. No proprietary black boxes.</p>
  </div>

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M3 10l5 5 9-11"/></svg></div>
    <h3>Safe by default</h3>
    <p>Every configuration change is validated, staged, and committed only after connectivity is confirmed. Failures roll back automatically.</p>
  </div>

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><circle cx="10" cy="10" r="7"/><path d="M10 3v7l5 3"/></svg></div>
    <h3>Operated, not babysat</h3>
    <p>Signed updates, built-in backup, module-level restore. Day-two operations are as considered as day-one deployment.</p>
  </div>

</div>

## System layers

Stratum is organised into four clean layers. Each layer is independently observable and can be audited without touching the one below.

<div class="arch-layers">

  <div class="arch-layer">
    <div class="arch-layer-num">04</div>
    <div class="arch-layer-body">
      <h3>Management Layer</h3>
      <p class="arch-layer-stack">Python · FastAPI · Jinja2 · HTTPS</p>
      <p>The web panel, REST API, and web terminal. Handles authentication, configuration staging, safe-apply orchestration, and audit logging. Runs as an isolated systemd service (<code>stratum-panel.service</code>) with its own virtual environment.</p>
    </div>
  </div>

  <div class="arch-layer">
    <div class="arch-layer-num">03</div>
    <div class="arch-layer-body">
      <h3>Service Layer</h3>
      <p class="arch-layer-stack">Suricata · CrowdSec · Unbound · WireGuard · Kea DHCP · RADIUS</p>
      <p>Individual daemons, each responsible for one function. They run independently — a failure in one does not cascade to the others. Each is monitored and controllable from the panel.</p>
    </div>
  </div>

  <div class="arch-layer">
    <div class="arch-layer-num">02</div>
    <div class="arch-layer-body">
      <h3>Network Layer</h3>
      <p class="arch-layer-stack">nftables · Linux routing · WireGuard kernel module · QoS (fq_codel)</p>
      <p>All packet handling happens in the Linux kernel via nftables. Policy decisions made by the service layer are enforced here at line rate.</p>
    </div>
  </div>

  <div class="arch-layer">
    <div class="arch-layer-num">01</div>
    <div class="arch-layer-body">
      <h3>Platform Layer</h3>
      <p class="arch-layer-stack">Debian 13 (trixie) · Linux kernel 6.12 LTS · systemd</p>
      <p>A standard, long-term-supported Linux foundation. No proprietary hypervisor, no custom kernel patches, no vendor lock-in.</p>
    </div>
  </div>

</div>

## Technology stack

<table class="table">
  <thead>
    <tr>
      <th>Function</th>
      <th>Component</th>
      <th>Role</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Operating system</td>
      <td>Debian 13 (trixie)</td>
      <td>Long-term support base with a standard package ecosystem</td>
    </tr>
    <tr>
      <td>Kernel</td>
      <td>Linux 6.12 LTS</td>
      <td>Modern networking features with extended maintenance</td>
    </tr>
    <tr>
      <td>Packet filtering</td>
      <td>nftables</td>
      <td>Kernel-level firewall and NAT engine</td>
    </tr>
    <tr>
      <td>IDS</td>
      <td>Suricata 7.x</td>
      <td>Signature-based intrusion detection on WAN interfaces</td>
    </tr>
    <tr>
      <td>Behavioral security</td>
      <td>CrowdSec + firewall bouncer</td>
      <td>Live behavioral analysis with kernel-level bans</td>
    </tr>
    <tr>
      <td>DNS resolver</td>
      <td>Unbound + RPZ</td>
      <td>DNSSEC-validating resolver with category filtering</td>
    </tr>
    <tr>
      <td>VPN</td>
      <td>WireGuard</td>
      <td>Modern, high-performance encrypted tunnels</td>
    </tr>
    <tr>
      <td>DHCP</td>
      <td>Kea DHCP</td>
      <td>Pool management and MAC-based reservations</td>
    </tr>
    <tr>
      <td>QoS</td>
      <td>fq_codel</td>
      <td>Fair-queuing traffic shaping at the kernel level</td>
    </tr>
    <tr>
      <td>Identity</td>
      <td>RADIUS</td>
      <td>Enterprise authentication for Wi-Fi and captive portal</td>
    </tr>
    <tr>
      <td>Panel backend</td>
      <td>Python 3 · FastAPI</td>
      <td>REST API and server-rendered web interface</td>
    </tr>
    <tr>
      <td>Panel frontend</td>
      <td>Jinja2 templates</td>
      <td>Server-rendered pages with no client-side framework dependency</td>
    </tr>
    <tr>
      <td>Web server</td>
      <td>Uvicorn (behind nginx)</td>
      <td>ASGI server exposing the panel over HTTPS</td>
    </tr>
    <tr>
      <td>Update delivery</td>
      <td>Signed manifest</td>
      <td>Cryptographically verified updates from a central server</td>
    </tr>
  </tbody>
</table>

## Deployment options

Stratum runs identically across bare metal and virtualised infrastructure. The same image boots on all of the following:

<div class="card-grid grid-2" style="margin-top:32px;">

  <div class="card">
    <h3>Bare metal</h3>
    <p>The recommended production deployment. Install directly on the appliance of your choice — from a small desktop-class box for branch offices to a 1U rack server for the data centre.</p>
  </div>

  <div class="card">
    <h3>Virtual machine</h3>
    <p>Runs on KVM, VMware, VirtualBox, Hyper-V, or any standards-compliant hypervisor. Ideal for labs, branch rollouts, and disaster recovery.</p>
  </div>

  <div class="card">
    <h3>Cloud instance</h3>
    <p>Deploy on any IaaS provider that supports Debian 13 images. Useful for hybrid setups, VPC edge protection, and remote-work gateways.</p>
  </div>

  <div class="card">
    <h3>Edge &amp; branch</h3>
    <p>Small footprint, low resource requirements. Suitable for branch offices, retail locations, and remote sites that need full functionality without a full rack.</p>
  </div>

</div>

## Safe operations model

The most common cause of network outages is a well-intentioned change that cuts off access. Stratum prevents this at the platform level.

<ol class="arch-steps">

  <li>
    <h4>Validate</h4>
    <p>Every configuration change is checked against syntax rules and dependency constraints before anything is applied.</p>
  </li>

  <li>
    <h4>Stage</h4>
    <p>Changes are written to a staging area. The running configuration is untouched until you explicitly commit.</p>
  </li>

  <li>
    <h4>Commit with confirmation</h4>
    <p>For changes that could affect connectivity, the panel asks you to confirm reachability within a timeout window. If you don't confirm, the change is rolled back automatically.</p>
  </li>

  <li>
    <h4>Rollback on failure</h4>
    <p>If a commit fails for any reason, the previous configuration is restored and the failure is logged with a clear diagnostic.</p>
  </li>

</ol>

## Security model

<div class="card-grid grid-2" style="margin-top:32px;">

  <div class="card">
    <h3>Local certificate authority</h3>
    <p>An integrated CA lets you issue and sign internal certificates without an external PKI. The panel itself uses HTTPS with certificates you control. The CA bundle is downloadable for distribution to clients.</p>
  </div>

  <div class="card">
    <h3>Session management</h3>
    <p>Panel sessions are signed with per-installation secrets and scoped to individual user accounts. No shared admin password, no credential sprawl.</p>
  </div>

  <div class="card">
    <h3>Signed updates</h3>
    <p>Update packages are verified against a signed manifest before application. A compromised mirror cannot deliver a modified payload — the signature check fails and the update is refused.</p>
  </div>

  <div class="card">
    <h3>Data stays local</h3>
    <p>No telemetry, no cloud dependency, no phoning home. The only outbound connection is the update check, and it is one-way and configurable.</p>
  </div>

</div>

## Extensibility

Because the platform is standard Debian, everything you already know applies:

- **Package management** — `apt` works as expected for any additional software.
- **Automation** — Standard tools (Ansible, Puppet, cloud-init) integrate without adapters.
- **Monitoring** — SNMP exposes the firewall to Zabbix, PRTG, LibreNMS, or any NMS.
- **Logs** — All services log to systemd journal and flat files, ready for shipping to a SIEM.
- **API** — A REST API exposes every configuration surface for programmatic management.

---

<div class="card-grid grid-2" style="margin-top:48px;">

  <div class="card">
    <h3>See the feature set</h3>
    <p>All 26 integrated modules, grouped by category, with technical detail on each.</p>
    <a href="/product/features/" class="card-link">Browse features →</a>
  </div>

  <div class="card">
    <h3>Try the live dashboard</h3>
    <p>Explore the actual panel — every module is there, running on a live instance.</p>
    <a href="/try-me/" class="card-link">Open the demo →</a>
  </div>

</div>
