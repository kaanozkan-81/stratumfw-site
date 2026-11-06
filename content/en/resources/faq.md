---
title: "FAQ"
lead: "Answers to the most common questions about Stratum Firewall, its architecture, and licensing."
full_width: true
---

<div class="faq-sections">

<section class="faq-section">
  <h2>General &amp; Platform</h2>

  <div class="faq">
    <details>
      <summary>What is Stratum Firewall?</summary>
      <div class="faq-body">
        Stratum Firewall is an open-source, enterprise-grade Network Security Operating System. It combines the raw power of the <strong>FreeBSD kernel</strong> and the <strong>pf packet filter</strong> with a modern, lightweight <strong>FastAPI management panel</strong>, providing a perfect balance of high performance and easy management.
      </div>
    </details>

    <details>
      <summary>Why did you choose FreeBSD instead of Linux?</summary>
      <div class="faq-body">
        We chose FreeBSD because it is designed to be a cohesive operating system rather than a collection of packages. It features the world's most battle-tested network stack, native support for <code>pf</code>, and ZFS for data integrity. This allows us to build a highly secure and predictable firewall appliance without worrying about upstream Linux distribution changes breaking the system overnight.
      </div>
    </details>

    <details>
      <summary>Is it open source?</summary>
      <div class="faq-body">
        Yes. The core configuration engine and panel will be fully open-source. There are no per-user licenses, no per-device fees, and no subscription requirements. You own the hardware and the software.
      </div>
    </details>

    <details>
      <summary>Does the platform "phone home" or send telemetry?</summary>
      <div class="faq-body">
        Absolutely not. There is no telemetry, no analytics, and no cloud dependency. Your firewall runs completely air-gapped if you want it to. You have full control over your data.
      </div>
    </details>
  </div>
</section>

<section class="faq-section">
  <h2>Architecture &amp; Management</h2>

  <div class="faq">
    <details>
      <summary>What does "Declarative Configuration" mean?</summary>
      <div class="faq-body">
        It means you never edit complex firewall rules by hand. You configure the system via the web UI or API, which saves your intent to a single <code>config.json</code> file. A Python engine then translates this JSON into native FreeBSD configuration files (like <code>pf.conf</code>). This ensures your firewall state is always auditable and easy to back up.
      </div>
    </details>

    <details>
      <summary>What is the "Safe-Apply" workflow?</summary>
      <div class="faq-body">
        Safe-Apply prevents you from locking yourself out. When you make a critical change (like changing the WAN IP or a firewall rule), Stratum validates it using <code>pfctl -nf</code>. It then applies the rule but starts a countdown. If you don't confirm that you still have connectivity, it automatically rolls back to the previous working configuration.
      </div>
    </details>

    <details>
      <summary>Can I automate Stratum using scripts?</summary>
      <div class="faq-body">
        Yes. The entire platform is API-first. The web panel is just a client that talks to the FastAPI backend. You can use Ansible, Terraform, or custom Python scripts to automate every aspect of the firewall configuration.
      </div>
    </details>

    <details>
      <summary>How does Role-Based Access Control (RBAC) work?</summary>
      <div class="faq-body">
        Stratum supports three distinct roles: <strong>Admin</strong> (full access), <strong>Operator</strong> (can run diagnostics and manage states, but cannot change firewall rules), and <strong>Read-Only</strong> (can only view dashboards). This enforces the principle of least privilege for your team.
      </div>
    </details>
  </div>
</section>

<section class="faq-section">
  <h2>Features &amp; Capabilities</h2>

  <div class="faq">
    <details>
      <summary>How does WireGuard integration work?</summary>
      <div class="faq-body">
        We didn't just wrap an external library. Stratum implements the X25519 cryptography internally, meaning zero external dependencies. You can generate peer configurations and QR codes in seconds directly from the panel, ensuring maximum security and performance.
      </div>
    </details>

    <details>
      <summary>Do you support Multi-WAN Failover?</summary>
      <div class="faq-body">
        Yes. You can configure multiple WAN interfaces (DHCP, Static, PPPoE). Stratum monitors the health of your primary connection and automatically fails over to the backup if it drops, ensuring business continuity.
      </div>
    </details>

    <details>
      <summary>Can I block malicious IPs and Domains?</summary>
      <div class="faq-body">
        Yes, at both layers. You can ingest public threat feeds to block malicious IPs at the <code>pf</code> level (using pf tables), and you can enable DNSBL to block malicious domains at the Unbound resolver level.
      </div>
    </details>
  </div>
</section>

<section class="faq-section">
  <h2>Support &amp; Releases</h2>

  <div class="faq">
    <details>
      <summary>When will the FreeBSD ISO be available?</summary>
      <div class="faq-body">
        We are currently in <strong>Developer Preview (v0.16.0)</strong>. The pre-built FreeBSD installer ISO is under active development and is released as v0.16.0.
      </div>
    </details>

    <details>
      <summary>Can I use it in production today?</summary>
      <div class="faq-body">
        While the core engine (pf, WireGuard) is highly stable, the panel and configuration engine are still being finalized. We recommend using it in homelabs and testing environments for now.
      </div>
    </details>

    <details>
      <summary>How do I get help or report a bug?</summary>
      <div class="faq-body">
        You can reach the development team directly at <a href="mailto:info@stratumfw.com">info@stratumfw.com</a>, or open an issue on our GitHub repository.
      </div>
    </details>
  </div>
</section>

</div>

---

<div style="text-align:center; margin-top:64px;">
  <h3 style="font-family:var(--font-display); font-size:22px; margin-bottom:12px;">Still have questions?</h3>
  <p style="color:var(--ash); margin-bottom:24px;">Reach out — we'd rather answer them than have you guess.</p>
  <a href="/company/contact/" class="btn btn-primary">Contact us</a>
</div>
