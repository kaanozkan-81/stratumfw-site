---
title: "Changelog"
lead: "Complete version history for Stratum Gateway — from the initial architecture redesign to the current Developer Preview."
---

All notable changes to the Stratum Gateway project.

---

## v0.8.0 — Developer Preview (Current)

The current milestone represents a complete architectural rewrite. We moved away from Debian and heavy external dependencies to build a modern, FreeBSD-based firewall OS.

- **Core Architecture:** Migrated to **FreeBSD 14+**. All packet filtering is now handled natively by the **pf** kernel module for wire-speed performance.
- **Management Plane:** Introduced a brand new **Python 3.12 + FastAPI** backend. The UI is fully API-driven and server-rendered via Jinja2.
- **Declarative Config:** Implemented the JSON-based configuration engine. The system now generates `pf.conf`, `wg0.conf`, and `unbound.conf` dynamically.
- **Safe-Apply Workflow:** Added pre-flight syntax checking (`pfctl -nf`) and automatic rollback mechanisms to prevent lockouts.
- **WireGuard VPN:** Implemented a zero-dependency WireGuard integration. X25519 cryptography is handled internally without external C libraries.
- **RBAC:** Added strict Role-Based Access Control (Admin, Operator, Read-Only) across all API endpoints.
- **Documentation:** Published the new Developer Guide for building the panel from source on FreeBSD.

## v0.5.0 — Early Prototyping (Legacy Linux Stack)

*Note: This version was part of the failed Linux/Debian prototype and is no longer supported.*

- Initial proof-of-concept for a web-managed firewall.
- Basic integration with Suricata and CrowdSec.
- Initial attempts at a JSON-to-pf configuration generator.

## v0.1.0 — Initial Concept

*Note: The original concept phase.*

- Basic network interface management.
- Proof of concept for an API-driven interface.

---

<p style="text-align:center; margin-top:48px; color:var(--ash); font-size:14.5px;">
  For detailed commit history and older prototypes, visit the <a href="https://github.com/kaanozkan-81/stratumfw-site" style="color:var(--accent);">GitHub repository</a>.
</p>
