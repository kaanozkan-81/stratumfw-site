---
title: "Product Overview"
lead: "A modern, API-driven firewall OS that strips away bloat and gives you absolute control over your network."
---

## What Stratum Firewall is

Stratum Firewall is an enterprise-grade **Network Security Operating System** built on **FreeBSD**. It is designed for engineers who demand transparency, raw performance, and modern manageability.

Unlike traditional firewall appliances that hide behind proprietary CLIs or heavy, cloud-dependent dashboards, Stratum provides a **lightweight FastAPI web panel** that acts as a direct interface to the system. 

Every change you make in the UI is validated, declaratively written to a single `config.json` file, and instantly rendered into native kernel configurations (like `pf.conf` and `wg0.conf`).

## The Stratum Philosophy

<div class="card-grid grid-3" style="margin-top:32px;">
  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M10 3L3 6v6c0 4.4 3.3 6.5 7 8c3.7-1.5 7-3.6 7-8V6l-7-3z"/></svg></div>
    <h3>Zero Vendor Lock-in</h3>
    <p>Built entirely on open standards. No subscriptions, no hidden telemetry, and no proprietary cloud dependencies. You own the hardware and the software.</p>
  </div>
  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M8 4H4v4M12 16h4v-4M4 16l5-5M16 4l-5 5"/></svg></div>
    <h3>API-First Architecture</h3>
    <p>The entire system is driven by a REST API. The web panel is just a client. Automate your firewall using Ansible, Terraform, or custom Python scripts.</p>
  </div>
  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M12 3l-8 8 8 8 8-8-8-8z"/><path d="M8 12l4-4"/></svg></div>
    <h3>Kernel-Level Security</h3>
    <p>We don't do userspace filtering. Stratum leverages FreeBSD's native <strong>pf packet filter</strong> and ALTQ for stateful inspection and traffic shaping at wire speed.</p>
  </div>
</div>

## Core Capabilities

Stratum is not a collection of 30 loosely integrated scripts. It is a cohesive, purpose-built platform with deep integrations.

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>pf &amp; Routing Engine</h3>
    <p>Full stateful packet inspection (SPI), Network Address Translation (NAT), port forwarding, and VLAN support powered directly by FreeBSD's pf.</p>
    <a href="/product/features/" class="card-link">Learn more →</a>
  </div>
  <div class="card">
    <h3>WireGuard VPN</h3>
    <p>Native, high-performance remote access. Features built-in QR code generation and zero external dependencies (cryptography is handled internally).</p>
    <a href="/product/features/" class="card-link">Learn more →</a>
  </div>
  <div class="card">
    <h3>Threat Intelligence &amp; Blocklists</h3>
    <p>Protect at all layers. Block malicious IPs at the firewall level (IPv4/IPv6) and filter malicious domains at the DNS level (DNSBL) with automated feed updates.</p>
    <a href="/product/features/" class="card-link">Learn more →</a>
  </div>
  <div class="card">
    <h3>Safe-Apply Workflow</h3>
    <p>Never lock yourself out. Every configuration change is tested against <code>pfctl -nf</code> before applying. If you misconfigure the WAN, the system rolls back automatically.</p>
    <a href="/product/architecture/" class="card-link">Read about the architecture →</a>
  </div>
</div>

## Why FreeBSD?

We didn't choose Linux. We chose FreeBSD because it is an operating system designed to be a single, cohesive, and secure network appliance. 

- **The pf packet filter** is the gold standard for firewall engines (originating from OpenBSD, but deeply integrated into FreeBSD).
- **ZFS** offers enterprise-grade storage integrity for configurations and logs.
- **Jails** allow for secure isolation of services if needed.
- **Predictable release cycles** mean your firewall won't break due to upstream packaging changes overnight.

## Built for the Modern Enterprise

Whether you are running a homelab, a small business, or managing distributed enterprise infrastructure, Stratum provides the tools you need without the baggage of legacy enterprise firewalls.

<div class="stats" style="margin-top:32px; border:1px solid var(--line); border-radius:14px;">
  <div class="wrap">
    <div class="stat"><b>FreeBSD</b><span>OS Foundation</span></div>
    <div class="stat"><b>pf</b><span>Firewall Engine</span></div>
    <div class="stat"><b>FastAPI</b><span>Management Plane</span></div>
    <div class="stat"><b>JSON</b><span>Declarative Config</span></div>
  </div>
</div>
