---
title: "Architecture"
lead: "A modern management plane built on top of the raw power of FreeBSD. Declarative, auditable, and secure by design."
---

## Design Principles

Stratum Firewall is not a collection of shell scripts. It is a cohesive system built on three core principles that shape every line of code in the platform:

<div class="card-grid grid-3" style="margin-top:32px;">

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M10 2v16M4 6l6-4 6 4M4 14l6 4 6-4"/></svg></div>
    <h3>Declarative State</h3>
    <p>There is only one source of truth: <code>config.json</code>. You never edit <code>pf.conf</code> directly. The system reads your intent and generates the native configuration files.</p>
  </div>

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M3 10l5 5 9-11"/></svg></div>
    <h3>Safe by Default</h3>
    <p>Every change is staged. Before a rule is applied to the kernel, the system uses <code>pfctl -nf</code> to validate syntax and uses a confirmation timeout to rollback if you lose connectivity.</p>
  </div>

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><circle cx="10" cy="10" r="7"/><path d="M10 3v7l5 3"/></svg></div>
    <h3>API-First</h3>
    <p>The web panel is just a client. Every action performed in the UI is executed through a REST API. This allows for full Infrastructure-as-Code automation.</p>
  </div>

</div>

## System Layers

Stratum operates in three strictly separated layers. A failure or change in one layer cannot cascade uncontrolled into another.

<div class="arch-layers">

  <div class="arch-layer">
    <div class="arch-layer-num">03</div>
    <div class="arch-layer-body">
      <h3>Management Plane</h3>
      <p class="arch-layer-stack">Python 3.12 · FastAPI · Uvicorn · Jinja2 · Vanilla JS</p>
      <p>The web interface and REST API. It handles authentication, Role-Based Access Control (RBAC), input validation, and stages configuration changes. It is stateless and reacts to the current state of the system.</p>
    </div>
  </div>

  <div class="arch-layer">
    <div class="arch-layer-num">02</div>
    <div class="arch-layer-body">
      <h3>Configuration Engine</h3>
      <p class="arch-layer-stack">Python · generators.py · safe_apply.py</p>
      <p>The brain of the operation. It reads <code>config.json</code> and dynamically generates native FreeBSD configurations: <code>pf.conf</code>, <code>wg0.conf</code>, <code>unbound.conf</code>, and <code>rc.conf</code>. It also manages atomic rollbacks if a change fails.</p>
    </div>
  </div>

  <div class="arch-layer">
    <div class="arch-layer-num">01</div>
    <div class="arch-layer-body">
      <h3>Data Plane (Kernel)</h3>
      <p class="arch-layer-stack">FreeBSD 15.1 · pf · ALTQ · WireGuard (Kernel module)</p>
      <p>All packet processing happens here. pf handles stateful inspection, NAT, and routing at wire speed. WireGuard runs entirely in the kernel for maximum VPN throughput.</p>
    </div>
  </div>

</div>

## The Safe-Apply Workflow

The most common cause of network outages is a well-intentioned configuration change that cuts off access. Stratum eliminates this by treating every change as a transaction.

<ol class="arch-steps">
  <li>
    <h4>Validate</h4>
    <p>The UI or API submits the change. The Python engine checks it against syntax rules and dependency constraints.</p>
  </li>
  <li>
    <h4>Render &amp; Test</h4>
    <p>The engine generates a new <code>pf.conf</code> and tests it using <code>pfctl -nf</code>. If the syntax is invalid, it is rejected immediately.</p>
  </li>
  <li>
    <h4>Commit with Confirmation</h4>
    <p>For changes that affect connectivity (like WAN IP changes), the panel applies the rule but starts a countdown. If you don't confirm connectivity, the change is rolled back automatically.</p>
  </li>
  <li>
    <h4>Atomic Rollback</h4>
    <p>If a commit fails or the system becomes unreachable, the previous configuration is restored, and the failure is logged with a clear diagnostic message.</p>
  </li>
</ol>

## Technology Stack

We use battle-tested, standard components. No black boxes.

<table class="table">
  <thead>
    <tr>
      <th>Layer</th>
      <th>Component</th>
      <th>Role</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Operating System</td>
      <td>FreeBSD 15.1+</td>
      <td>The foundation. Known for its robust network stack and security.</td>
    </tr>
    <tr>
      <td>Packet Filter</td>
      <td>pf (Packet Filter)</td>
      <td>Stateful firewall, NAT, and traffic shaping (ALTQ).</td>
    </tr>
    <tr>
      <td>VPN</td>
      <td>WireGuard</td>
      <td>In-kernel, high-speed cryptographic tunnels.</td>
    </tr>
    <tr>
      <td>Panel Backend</td>
      <td>Python 3.12 · FastAPI</td>
      <td>Asynchronous, high-performance REST API and web server.</td>
    </tr>
    <tr>
      <td>Panel Frontend</td>
      <td>Jinja2 · Vanilla JS</td>
      <td>Server-side rendering with zero heavy frontend frameworks.</td>
    </tr>
    <tr>
      <td>Config Engine</td>
      <td>Python 3 (Internal)</td>
      <td>Translates <code>config.json</code> into native system files.</td>
    </tr>
  </tbody>
</table>

## Security Model

<div class="card-grid grid-2" style="margin-top:32px;">

  <div class="card">
    <h3>Role-Based Access Control (RBAC)</h3>
    <p>The panel supports three distinct user roles. <b>Admin</b> can change anything. <b>Operator</b> can perform diagnostics and manage states. <b>Read-Only</b> can view the dashboard and logs but cannot make changes.</p>
  </div>

  <div class="card">
    <h3>Strict Secrets Management</h3>
    <p>Private keys (like WireGuard keys) are never stored in the main <code>config.json</code>. They are written to disk with <code>chmod 600</code> permissions and read only when the configuration is rendered.</p>
  </div>

  <div class="card">
    <h3>Zero External Dependencies</h3>
    <p>We don't rely on external package repositories for our critical UI. The panel runs on standard Python libraries and is served via Uvicorn, minimizing the attack surface.</p>
  </div>

  <div class="card">
    <h3>Tamper-Evident Logs</h3>
    <p>Every action is logged via syslog to <code>/var/log/auth.log</code> (facility: auth). Changes are tied to the specific user account that made them.</p>
  </div>

</div>

---

<div class="card-grid grid-2" style="margin-top:48px;">
  <div class="card">
    <h3>Explore Features</h3>
    <p>See the full list of capabilities built on top of this architecture.</p>
    <a href="/product/features/" class="card-link">Browse features →</a>
  </div>
  <div class="card">
    <h3>Read the Docs</h3>
    <p>Installation, configuration, and API reference for Stratum Firewall.</p>
    <a href="/resources/documentation/" class="card-link">Open documentation →</a>
  </div>
</div>
