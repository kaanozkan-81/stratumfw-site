---
title: "REST API"
description: "Automate firewall rules, WireGuard peers, aliases, and Safe-Apply workflows using the Stratum Firewall REST API."
lead: "Automate everything. The Stratum web panel is backed by a clean asynchronous FastAPI REST architecture."
---

Stratum Firewall is built with an **API-first** architecture on FreeBSD 15.1. Every action available in the web interface executes through structured REST endpoints, allowing you to integrate Stratum FW into your monitoring and Infrastructure-as-Code (Ansible, Terraform, Python) workflows.

## Base URL

All API endpoints are served over HTTPS relative to your firewall's management address:

```text
https://<firewall-ip>/api/
```

## Authentication

### 1. Sign In (Obtain Session Cookie)

```bash
curl -k -X POST https://192.168.1.1/login \
  -d "username=admin&password=your_secure_password" \
  -c cookies.txt
```

### 2. Authenticated Requests

Include the session cookie and the `X-Requested-With: stratum` CSRF protection header:

```bash
curl -k -X GET https://192.168.1.1/api/status \
  -b cookies.txt \
  -H "X-Requested-With: stratum"
```

## Role-Based Access Control (RBAC)

| Role | Permissions |
| :--- | :--- |
| **Administrator** | Full read/write access across all configuration and system endpoints. |
| **Operator** | Read access across configuration endpoints plus diagnostic actions (`/api/diag/*`). |
| **Read-Only** | Read-only inspection access (used in the [Live Demo](https://demo.stratumfw.com)); write requests return `403 Forbidden`. |

## Safe-Apply Rollback Workflow via API

1. Submit your configuration update; Stratum validates the configuration (`pfctl -n`, `unbound-checkconf`, etc.) and returns `apply_needed: true`.
2. Apply changes and call `/api/apply/confirm` within **60 seconds**; otherwise Stratum automatically rolls back to the previous working snapshot to prevent remote lockouts.

## Next Steps

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>Command Line Utilities</h3>
    <p>Explore Stratum FW's built-in FreeBSD CLI tools.</p>
    <a href="/resources/documentation/command-line/" class="card-link">CLI Reference →</a>
  </div>
  <div class="card">
    <h3>File &amp; Path Reference</h3>
    <p>See where configuration files, keys, and ZFS snapshots live.</p>
    <a href="/resources/documentation/file-paths/" class="card-link">File Reference →</a>
  </div>
</div>
