---
title: "About"
lead: "We build network security tools for the people who actually run the network."
---

## Why Stratum exists

Enterprise firewalls have become complacent. The market is dominated by legacy appliances with slow, clunky interfaces, or by cloud-managed systems that demand you hand over control of your network data. 

**Stratum Security** was founded to break that mold.

We built Stratum Gateway because we wanted a firewall that is:
- **Serious enough for enterprises** — utilizing the raw, battle-tested power of the FreeBSD kernel and the `pf` packet filter.
- **Modern and API-driven** — managed via a lightweight FastAPI backend, allowing complete automation and Infrastructure-as-Code.
- **Yours to control** — 100% self-hosted, open-source, and free from vendor lock-in.

## What we believe

<div class="card-grid grid-3" style="margin-top:32px;">

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M3 10l5 5 9-11"/></svg></div>
    <h3>Standards over reinvention</h3>
    <p>We don't reinvent the wheel. We use FreeBSD, pf, and WireGuard — because these are the tools professionals already trust. Our job is to make them work together beautifully.</p>
  </div>

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M8 4H4v4M12 16h4v-4M4 16l5-5M16 4l-5 5"/></svg></div>
    <h3>Transparency by design</h3>
    <p>No black boxes. Every configuration you make in the UI is translated into human-readable `pf.conf` and `wg0.conf` files. What you see is exactly what the kernel runs.</p>
  </div>

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M10 2v16M4 6l6-4 6 4M4 14l6 4 6-4"/></svg></div>
    <h3>Safety as a feature</h3>
    <p>Network changes break networks. Every configuration in Stratum is validated, staged, and reversible. Nobody should lose connectivity because of a typo.</p>
  </div>

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><rect x="4" y="4" width="12" height="12" rx="2"/><path d="M8 2v2M12 2v2M8 16v2M12 16v2M2 8h2M2 12h2M16 8h2M16 12h2"/></svg></div>
    <h3>Local by default</h3>
    <p>The best place for your security data is your own infrastructure. We don't do telemetry, we don't do "phone home", and we don't do cloud lock-in.</p>
  </div>

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M2 10c2-3.3 5-5 8-5s6 1.7 8 5c-2 3.3-5 5-8 5s-6-1.7-8-5Z"/></svg></div>
    <h3>Built for operators</h3>
    <p>Our users are sysadmins, MSPs, and small IT teams — not analysts in a SOC. Every feature is judged by how it helps someone who has twenty other things to do.</p>
  </div>

  <div class="card">
    <div class="card-icon"><svg width="22" height="22" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M10 17.5S4 12.8 4 8a6 6 0 0 1 12 0c0 4.8-6 9.5-6 9.5Z"/></svg></div>
    <h3>Long-term thinking</h3>
    <p>We choose FreeBSD and Python because we want deployments to run for a decade, not a product cycle. Open standards mean a longer lifespan for your hardware.</p>
  </div>

</div>

## The platform

Stratum Gateway is currently at **version 0.8.0 (Developer Preview)**. It is a complete architectural rewrite built on **FreeBSD 14+** with a **FastAPI** management panel.

<div class="stats" style="margin-top:32px; border:1px solid var(--line); border-radius:14px;">
  <div class="wrap">
    <div class="stat"><b>v0.8.0</b><span>Developer Preview</span></div>
    <div class="stat"><b>FreeBSD</b><span>OS Foundation</span></div>
    <div class="stat"><b>pf</b><span>Firewall Engine</span></div>
    <div class="stat"><b>API-First</b><span>Management Plane</span></div>
  </div>
</div>

## Where we're going

We are actively building the platform in the open. Priorities for the coming releases:

- **v0.9.0:** FreeBSD installer ISO, IP/DNS Blocklists, Multi-WAN failover.
- **v1.0.0:** Production-ready image, automated updates, full audit logging.
- **Future:** Multi-site management, deeper API integrations, and community-driven modules.

## Talk to us

If you're evaluating Stratum Gateway for your organization, or if you're building something with it that others should know about — we want to hear from you.

<div style="margin-top:32px;">
  <a href="/company/contact/" class="btn btn-primary">Contact us</a>
</div>
