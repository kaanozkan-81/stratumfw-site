---
title: "Enterprise"
lead: "Multi-site protection with the operational discipline large organizations require."
---

## What changes at enterprise scale

A single office has one network to protect. An enterprise has many — and they all need to behave the same way, report to the same people, and pass the same audits. Common pressures:

- **Multiple sites** — branches, warehouses, subsidiaries — each with its own internet connection and local quirks
- **Compliance** — ISO 27001, SOC 2, KVKK, GDPR, or sector-specific requirements that demand evidence, not promises
- **Central visibility** — security teams that can't afford to log into 40 different panels
- **Audit trails** — every configuration change needs to be attributable and reversible
- **Integration** — SIEM, NMS, and identity systems that already exist and aren't going away

**Stratum Gateway was built to slot into this environment, not replace it.**

## How Stratum fits

<div class="card-grid grid-2" style="margin-top:32px;">

  <div class="card">
    <h3>Consistent per-site deployment</h3>
    <p>The same image, the same configuration model, the same interface. A branch office in one city behaves like a branch office in another — because it is one.</p>
  </div>

  <div class="card">
    <h3>Standard integration paths</h3>
    <p>SNMP for monitoring. NetFlow for flow visibility. REST API for orchestration. Logs in standard formats ready for SIEM ingestion.</p>
  </div>

  <div class="card">
    <h3>Per-module backup &amp; restore</h3>
    <p>Full-system snapshots are available, but the real value is granular: restore just the firewall rules, just the VPN config, just the QoS policies — without touching anything else.</p>
  </div>

  <div class="card">
    <h3>Signed updates with rollback</h3>
    <p>Cryptographically verified update manifests, applied with automatic rollback on failure. Rolling update cadence across sites is under your control.</p>
  </div>

  <div class="card">
    <h3>Local certificate authority</h3>
    <p>Issue and sign internal certificates without depending on an external PKI. Distribute the CA bundle to workstations and services you control.</p>
  </div>

  <div class="card">
    <h3>Full audit surface</h3>
    <p>Every configuration change is captured, attributable to a panel user, and reversible. What changed, who changed it, when — all visible.</p>
  </div>

</div>

## Reference architecture

A typical enterprise deployment looks like this:

<div class="ent-arch">

  <div class="ent-arch-row">
    <div class="ent-arch-col">
      <span class="ent-arch-label">Headquarters</span>
      <p>Primary Stratum Gateway with the most complete module set — IDS, CrowdSec, full web filtering, central logging export.</p>
    </div>
    <div class="ent-arch-col">
      <span class="ent-arch-label">Data center</span>
      <p>A second gateway protecting server segments, often paired with the HQ deployment for redundancy.</p>
    </div>
  </div>

  <div class="ent-arch-row">
    <div class="ent-arch-col">
      <span class="ent-arch-label">Branch offices</span>
      <p>One Stratum Gateway per site — same image, site-appropriate module set. Everything reports to the same NMS and SIEM.</p>
    </div>
    <div class="ent-arch-col">
      <span class="ent-arch-label">Remote &amp; hybrid work</span>
      <p>WireGuard VPN peering back to HQ. No hardware at the user end — just a QR code and a laptop.</p>
    </div>
  </div>

  <div class="ent-arch-row">
    <div class="ent-arch-col">
      <span class="ent-arch-label">Monitoring &amp; SIEM</span>
      <p>Zabbix, PRTG, LibreNMS pull SNMP metrics. Splunk, Elastic, or Wazuh consume logs. NetFlow feeds flow analysis.</p>
    </div>
    <div class="ent-arch-col">
      <span class="ent-arch-label">Identity</span>
      <p>RADIUS authenticates Wi-Fi and captive portal users against existing directory services.</p>
    </div>
  </div>

</div>

## Operational advantages

<table class="table">
  <thead>
    <tr>
      <th>Concern</th>
      <th>Traditional stack</th>
      <th>With Stratum Gateway</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Site rollouts</td>
      <td>Per-vendor configuration, per-model quirks</td>
      <td>One image, one configuration model, everywhere</td>
    </tr>
    <tr>
      <td>Incident response</td>
      <td>Log into each site separately</td>
      <td>Uniform interface across all sites, consistent logs</td>
    </tr>
    <tr>
      <td>Change management</td>
      <td>Manual, error-prone, hard to audit</td>
      <td>Staged, confirmed, rolled back automatically if needed</td>
    </tr>
    <tr>
      <td>Patching</td>
      <td>Manual, per-site, outage-window dependent</td>
      <td>Signed updates, rollback-safe, scheduled</td>
    </tr>
    <tr>
      <td>Vendor lock-in</td>
      <td>Proprietary hardware, proprietary OS</td>
      <td>Standard Debian, portable across any x86-64 platform</td>
    </tr>
    <tr>
      <td>Data residency</td>
      <td>Cloud dashboards, unclear data flows</td>
      <td>100% self-hosted — nothing leaves your premises</td>
    </tr>
  </tbody>
</table>

## Feature highlights for enterprise

<div class="card-grid grid-2" style="margin-top:32px;">

  <div class="card">
    <h3>SNMP</h3>
    <p>Integrate with Zabbix, PRTG, LibreNMS, or any standard NMS.</p>
    <a href="/product/features/#snmp" class="card-link">Learn more →</a>
  </div>

  <div class="card">
    <h3>NetFlow</h3>
    <p>Flow-level visibility for top talkers and destination analysis.</p>
    <a href="/product/features/#netflow" class="card-link">Learn more →</a>
  </div>

  <div class="card">
    <h3>Backup &amp; Restore</h3>
    <p>Per-module snapshots and full-system recovery.</p>
    <a href="/product/features/#backup" class="card-link">Learn more →</a>
  </div>

  <div class="card">
    <h3>Certificate Authority</h3>
    <p>Internal PKI for service certificates — no external dependency.</p>
    <a href="/product/features/#ca" class="card-link">Learn more →</a>
  </div>

  <div class="card">
    <h3>RADIUS</h3>
    <p>Enterprise authentication for Wi-Fi and captive portal users.</p>
    <a href="/product/features/#radius" class="card-link">Learn more →</a>
  </div>

  <div class="card">
    <h3>CrowdSec</h3>
    <p>Community threat intelligence extends your detection surface.</p>
    <a href="/product/features/#crowdsec" class="card-link">Learn more →</a>
  </div>

</div>

## Compliance-friendly by design

The platform does not claim to make you compliant — that depends on your processes, not just your tools. But Stratum removes friction from the parts where tools usually get in the way:

- **Attributable changes** — every panel action is tied to a user account.
- **Reversible configuration** — nothing is destructive by default; changes are staged and confirmed.
- **Data residency** — everything runs on infrastructure you control.
- **Standard formats** — logs and metrics export in formats your existing compliance tooling already understands.
- **Open source auditability** — the components are open; nothing has to be taken on faith.

<div style="margin-top:32px;">
  <a href="/try-me/" class="btn btn-primary">Try the live dashboard</a>
  <a href="mailto:k.ozkan@ethnohotels.com" class="btn btn-secondary">Talk to us</a>
</div>
