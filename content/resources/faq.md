---
title: "FAQ"
lead: "Answers to the questions we're asked most often — about the platform, licensing, deployment, and support."
full_width: true
---

<div class="faq-sections">

<section class="faq-section">
  <h2>General</h2>

  <div class="faq">
    <details>
      <summary>What is Stratum Gateway?</summary>
      <div class="faq-body">
        Stratum Gateway is an open-source, self-hosted firewall and network security platform. It consolidates more than twenty network functions — routing, failover, VPN, intrusion detection, content filtering, and traffic shaping — into one system managed from a single web panel.
      </div>
    </details>

    <details>
      <summary>Who builds it?</summary>
      <div class="faq-body">
        Stratum Gateway is developed by Stratum Security. You can reach the team at <a href="mailto:info@stratumfw.com">info@stratumfw.com</a>.
      </div>
    </details>

    <details>
      <summary>Is it really free?</summary>
      <div class="faq-body">
        Yes. The platform is open source and free to use, modify, and deploy — including in commercial environments. There are no per-user licenses, no per-device fees, and no subscription requirement. Optional paid services (managed support, professional services) are available but not required.
      </div>
    </details>

    <details>
      <summary>What version is the current release?</summary>
      <div class="faq-body">
        The current release is <strong>v1.3.0</strong>. Version information is available in the panel under <em>System → About</em>, and updates are delivered through the built-in update mechanism.
      </div>
    </details>

    <details>
      <summary>Does the platform "phone home" or send telemetry?</summary>
      <div class="faq-body">
        No. There is no telemetry, no analytics, and no cloud dependency. The only outbound connection the platform makes on its own is a version check against the update server — and this is one-way, verifies signatures, and does not transmit any data about your deployment.
      </div>
    </details>
  </div>
</section>

<section class="faq-section">
  <h2>Deployment</h2>

  <div class="faq">
    <details>
      <summary>What hardware do I need?</summary>
      <div class="faq-body">
        Stratum Gateway runs on any modern x86-64 platform. For a small business or branch office, a small-form-factor PC or mini-desktop with two network interfaces is enough. For larger deployments, a 1U rack server sized to your throughput requirements is typical. There is no proprietary hardware.
      </div>
    </details>

    <details>
      <summary>Can I run it as a virtual machine?</summary>
      <div class="faq-body">
        Yes. Stratum Gateway runs on KVM, VMware, VirtualBox, Hyper-V, Proxmox, and any standards-compliant hypervisor. It also runs on cloud instances that support Debian 13 images.
      </div>
    </details>

    <details>
      <summary>What operating system is underneath?</summary>
      <div class="faq-body">
        Debian 13 (trixie) with the Linux 6.12 LTS kernel. This is a deliberate choice: long-term support, standard tooling, and a package ecosystem your team already knows.
      </div>
    </details>

    <details>
      <summary>How long does installation take?</summary>
      <div class="faq-body">
        A basic deployment — install, network configuration, initial firewall rules — typically takes an afternoon. A more complete rollout with VPN, filtering, and IDS tuning is measured in days, not weeks.
      </div>
    </details>

    <details>
      <summary>Can I migrate an existing firewall configuration?</summary>
      <div class="faq-body">
        There is no automated import from other firewall vendors. However, Stratum's configuration model is visual and rule-based, so translating existing rules is a matter of re-entering them rather than writing scripts. Support is available for complex migrations.
      </div>
    </details>
  </section>

<section class="faq-section">
  <h2>Capabilities</h2>

  <div class="faq">
    <details>
      <summary>How many modules does the platform include?</summary>
      <div class="faq-body">
        Twenty-six integrated modules, organized into six categories: Network &amp; Connectivity, Security &amp; Threat Protection, Access &amp; Identity, Traffic Control &amp; Visibility, Management &amp; Operations, and Platform &amp; Foundation. A complete list is on the <a href="/product/features/">features page</a>.
      </div>
    </details>

    <details>
      <summary>Which VPN technologies are supported?</summary>
      <div class="faq-body">
        WireGuard is the primary VPN, with per-peer management, QR code provisioning for mobile clients, and downloadable client configurations. WireGuard is also used for site-to-site tunnels where appropriate.
      </div>
    </details>

    <details>
      <summary>What does "self-hosted" mean in practice?</summary>
      <div class="faq-body">
        It means the entire platform — panel, database, logs, threat feeds, configuration — runs on hardware you control. Nothing is sent to Stratum's servers. Nothing is stored in a third-party cloud. If you unplug the internet, the firewall keeps working.
      </div>
    </details>

    <details>
      <summary>Can it replace my existing router, firewall, and IDS?</summary>
      <div class="faq-body">
        In most cases, yes. Stratum covers routing, NAT, DHCP, DNS, VPN, IDS, content filtering, QoS, and more. Organizations typically keep whatever they already have for specialized needs (e.g., a dedicated WAF for web applications), but the core network security functions consolidate onto one platform.
      </div>
    </details>

    <details>
      <summary>Does it support multiple sites?</summary>
      <div class="faq-body">
        Yes. Each site runs its own Stratum Gateway instance, all using the same image and configuration model. Consistency across sites is the default, not an achievement. Centralized multi-site management is on the near-term roadmap.
      </div>
    </details>

    <details>
      <summary>Is there an API?</summary>
      <div class="faq-body">
        Yes. Every configuration surface is exposed through a REST API, so orchestration tools (Ansible, Terraform, custom scripts) can manage the platform programmatically.
      </div>
    </details>

    <details>
      <summary>Does it integrate with existing monitoring?</summary>
      <div class="faq-body">
        Yes. SNMP v2c and v3 let the firewall appear in Zabbix, PRTG, LibreNMS, or any standard NMS. NetFlow exports for flow analysis. Logs are available in standard formats for ingestion by Splunk, Elastic, Wazuh, or similar.
      </div>
    </details>
  </section>

<section class="faq-section">
  <h2>Security model</h2>

  <div class="faq">
    <details>
      <summary>What happens if a configuration change breaks connectivity?</summary>
      <div class="faq-body">
        Stratum uses a safe-apply model. Changes are validated, then staged, then applied with a confirmation step. If you lose connectivity after applying a change, the system rolls back automatically to the previous configuration. You cannot permanently lock yourself out with a bad change.
      </div>
    </details>

    <details>
      <summary>How are updates delivered?</summary>
      <div class="faq-body">
        Updates are distributed as signed packages with a cryptographic manifest. The panel verifies the signature before applying anything. A compromised mirror cannot deliver a modified payload — the signature check fails and the update is refused.
      </div>
    </details>

    <details>
      <summary>Are there audit logs?</summary>
      <div class="faq-body">
        Yes. Every configuration change is captured with a timestamp, the user account that made it, and what was changed. Nothing in the panel is done anonymously, which supports audit and compliance requirements.
      </div>
    </details>

    <details>
      <summary>Does the platform include a certificate authority?</summary>
      <div class="faq-body">
        Yes. A built-in CA lets you create a local root, sign internal service certificates, and distribute the CA bundle to clients you control — no external PKI dependency.
      </div>
    </details>
  </section>

<section class="faq-section">
  <h2>Support</h2>

  <div class="faq">
    <details>
      <summary>How do I get help?</summary>
      <div class="faq-body">
        Email <a href="mailto:support@stratumfw.com">support@stratumfw.com</a>. Include the platform version (visible under <em>System → About</em>) and as much detail as you can about the situation.
      </div>
    </details>

    <details>
      <summary>Is there a community?</summary>
      <div class="faq-body">
        Yes — a community forum and discussion channels are being rolled out. Details will be published on this site as they become available.
      </div>
    </details>

    <details>
      <summary>Do you offer commercial support?</summary>
      <div class="faq-body">
        Optional paid support and professional services are available for organizations that want them. They are entirely optional — the platform is designed to be self-supported.
      </div>
    </details>

    <details>
      <summary>How do I report a security vulnerability?</summary>
      <div class="faq-body">
        Please email <a href="mailto:support@stratumfw.com?subject=Security%20Disclosure">support@stratumfw.com</a> with reproduction details and the affected version. Disclosures are taken seriously and handled responsibly.
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
