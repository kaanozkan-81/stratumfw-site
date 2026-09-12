---
title: "About"
lead: "We build network security tools for the people who actually run the network."
---

## Why Stratum exists

Enterprise network security has a broken business model. The best products are priced for the Fortune 500. The affordable ones cut corners that matter. And almost all of them assume your data should live in someone else's cloud.

**Stratum Security** was founded to close that gap.

We build a firewall platform that is:

- **Serious enough for enterprises** — the same IDS, behavioral threat detection, and DNS filtering that large organizations deploy
- **Accessible to everyone else** — open source, no per-user licensing, no recurring fees
- **Yours to control** — 100% self-hosted. Your data never leaves your network.

The platform is called **Stratum Gateway**, and it does the job of five or six separate appliances from one coherent system.

## What we believe

<div class="card-grid grid-3" style="margin-top:32px;">

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M3 10l5 5 9-11"/></svg></div>
    <h3>Security shouldn't be a subscription</h3>
    <p>A firewall that stops working because a payment failed is not a firewall. We ship open source software, and we don't gate the essentials behind a paywall.</p>
  </div>

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><circle cx="10" cy="10" r="7"/><path d="M10 3v7l5 3"/></svg></div>
    <h3>Standards over reinvention</h3>
    <p>We use Suricata, CrowdSec, Unbound, WireGuard, and nftables — because these are the tools professionals already trust. Our job is to make them work together beautifully.</p>
  </div>

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><rect x="4" y="4" width="12" height="12" rx="2"/><path d="M8 2v2M12 2v2M8 16v2M12 16v2M2 8h2M2 12h2M16 8h2M16 12h2"/></svg></div>
    <h3>Local by default</h3>
    <p>The best place for your security data is your own infrastructure. We don't do telemetry, we don't do "phone home", and we don't do cloud lock-in.</p>
  </div>

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M10 2v16M4 6l6-4 6 4M4 14l6 4 6-4"/></svg></div>
    <h3>Safe by design</h3>
    <p>Network changes break networks. Every configuration in Stratum is validated, staged, and reversible. Nobody should lose connectivity because of a typo.</p>
  </div>

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M2 10c2-3.3 5-5 8-5s6 1.7 8 5c-2 3.3-5 5-8 5s-6-1.7-8-5Z"/></svg></div>
    <h3>Built for operators</h3>
    <p>Our users are sysadmins, MSPs, and small IT teams — not analysts in a SOC. Every feature is judged by how it helps someone who has twenty other things to do.</p>
  </div>

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M10 17.5S4 12.8 4 8a6 6 0 0 1 12 0c0 4.8-6 9.5-6 9.5Z"/></svg></div>
    <h3>Long-term thinking</h3>
    <p>We choose Debian LTS kernels and standardized components because we want deployments to run for a decade, not a product cycle.</p>
  </div>

</div>

## The platform

Stratum Gateway is currently at **version 1.3.0**, running on **Debian 13 (trixie)**. It ships with twenty-six integrated modules and is deployed across three broad categories of environment: small businesses, enterprises, and educational institutions.

<div class="stats" style="margin-top:32px; border:1px solid var(--line); border-radius:14px;">
  <div class="wrap">
    <div class="stat"><b>v1.3.0</b><span>Current release</span></div>
    <div class="stat"><b>26</b><span>Integrated modules</span></div>
    <div class="stat"><b>Debian 13</b><span>Platform foundation</span></div>
    <div class="stat"><b>Open Source</b><span>Free to use and modify</span></div>
  </div>
</div>

## Where we're going

We are actively building the platform in the open, with a roadmap that prioritizes operational quality over feature count. Priorities for the coming releases:

- **Multi-site management** — coordinated configuration across many gateways from one console.
- **API-first expansion** — every capability available programmatically, for automation and Infrastructure-as-Code.
- **Enhanced reporting** — exportable, scheduled reports for compliance and audit.
- **Community contributions** — an ecosystem of third-party modules, feeds, and integrations.

## Talk to us

If you're evaluating Stratum Gateway for your organization, or if you're building something with it that others should know about — we want to hear from you.

<div style="margin-top:32px;">
  <a href="/company/contact/" class="btn btn-primary">Contact us</a>
  <a href="/try-me/" class="btn btn-secondary">Try the live dashboard</a>
</div>
