---
title: "Enterprise"
lead: "API-driven, highly available, and auditable. Built for modern infrastructure teams."
---

## What changes at enterprise scale

An enterprise network requires more than just a firewall. It requires **automation, visibility, and strict access control.**

- **Multiple sites** — branches, data centers, and cloud environments
- **Compliance** — ISO 27001, SOC 2, and strict audit trails
- **Infrastructure-as-Code** — Ansible, Terraform, and automated deployments
- **Zero Trust** — Strict Role-Based Access Control (RBAC) for administrators

**Stratum FW was built from the ground up to meet these demands.**

## How Stratum fits

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>API-First Architecture</h3>
    <p>Every configuration surface is exposed via a REST API. Automate your firewall deployments, integrate with CI/CD pipelines, and eliminate manual clicking.</p>
  </div>
  <div class="card">
    <h3>Strict RBAC</h3>
    <p>Differentiate between Administrators, Operators, and Read-Only users. Enforce the principle of least privilege across your entire team.</p>
  </div>
  <div class="card">
    <h3>Declarative Configuration</h3>
    <p>A single `config.json` is the source of truth. Easily version-control your firewall state, back it up, or deploy it to a new node in seconds.</p>
  </div>
  <div class="card">
    <h3>Auditable &amp; Transparent</h3>
    <p>No black boxes. The generated `pf.conf` and `wg0.conf` files are human-readable. Know exactly what the kernel is executing at all times.</p>
  </div>
</div>

## Operational advantages

<table class="table">
  <thead>
    <tr>
      <th>Concern</th>
      <th>Traditional stack</th>
      <th>With Stratum FW</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Automation</td>
      <td>Fragile screen-scraping scripts</td>
      <td>Clean, documented REST API</td>
    </tr>
    <tr>
      <td>Change management</td>
      <td>Manual, error-prone, hard to audit</td>
      <td>Safe-Apply, atomic rollback, strict audit logs</td>
    </tr>
    <tr>
      <td>Performance</td>
      <td>Userspace filtering (slow)</td>
      <td>Kernel-level pf (wire speed)</td>
    </tr>
    <tr>
      <td>Vendor lock-in</td>
      <td>Proprietary hardware and OS</td>
      <td>Standard FreeBSD, runs on any x86-64 hardware</td>
    </tr>
  </tbody>
</table>

<div style="margin-top:32px;">
  <a href="/product/architecture/" class="btn btn-primary">Read the Architecture</a>
  <a href="mailto:info@stratumfw.com" class="btn btn-secondary">Contact Sales</a>
</div>
