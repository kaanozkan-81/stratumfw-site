---
title: "System > Certificates"
seo_title: "09.6 System > Certificates & Block Page CA — Stratum FW User Manual"
description: "The web panel's TLS certificate (downloading, creating self-signed, installing your own certificate, restoring) and the Block Page CA for Application Control HTTPS block pages (.cer for Group Policy and .crt PEM)."
weight: 906
chapter_num: "09.6"
url: "/manual/system-certificates/"
aliases:
  - "/manual/09-06-system-certificates/"
translationKey: "manual-09-06-system-certificates"
---

The web panel is always served over HTTPS. **System > Certificates** controls the certificate the browser sees for the panel and manages the **Block Page CA** used by [**Firewall > Application Control**](/manual/firewall-application-control/#block-page).

![System > Certificates](/images/manual/certs.png)

## The current certificate

**Web panel certificate** shows who it was issued to, the names and addresses it covers, the issuer, its validity
and the days left - marked when fewer than 21 days remain or the certificate has expired - and its **SHA-256
fingerprint**. Click **Download panel certificate** to download the active web panel certificate directly from the page.

The fingerprint lets you check that you really talk to your firewall: compare it with the one your browser shows
(the padlock in the address bar > certificate details).

A notification can warn you 21 days before the certificate expires ([**System > Settings > Notifications**](/manual/system-settings-logging-notifications/)).

## The self-signed certificate

At installation the firewall creates its own certificate, valid for 10 years. Browsers warn about any
self-signed certificate until it is accepted once (or trusted on the device) - this is expected and not a sign of
an attack, as long as the fingerprint matches.

**New self-signed certificate** creates a new one. Under **Host names and addresses the panel is opened with**,
list - one per line - every name and address you type into the browser, for example:

```
192.168.1.1
fw-belek.lan
fw-belek.example.com
```

A browser warns again when the address in the address bar is not in the certificate.

**Create and install** - the new certificate takes effect at once.

### Trusting it on your computers

To get rid of the warning without a commercial certificate, download the panel certificate right from **System > Certificates** and import it as a trusted root certificate on the computers that manage the firewall - on Windows into *Trusted Root Certification Authorities*, on macOS into the keychain with *Always trust*. MCP clients need the same (see [**System > Settings: SNMP and MCP**](/manual/system-settings-snmp-mcp/)).

## Use your own certificate

A certificate from your company's certificate authority, or from a commercial provider for a public name such as
`fw.example.com`, is trusted by browsers without a warning.

1. Paste the **Certificate (PEM)** - or **Load from file**. The file may contain the intermediate certificates
   after the server certificate.
2. Paste the **Private key (PEM)** - or **Load from file**. The key must not be protected with a passphrase.
3. **Check and install**. The panel checks that the key matches the certificate and that it has not expired
   before installing it.

The certificate takes effect at once (no Apply).

> **Note:** Let's Encrypt (ACME) certificates arrive in a later version.

## Block Page CA

When a rule in [**Firewall > Application Control**](/manual/firewall-application-control/#block-page) is set to show a **block page** for `https://` sites, the isolated block page server (`_stratumbp`) signs a short-lived certificate for the blocked host name on the spot using the **Block Page CA**:

1. Click **Create Block Page CA** once on **System > Certificates**. The private key never leaves the firewall (readable only by `root` and `_stratumbp`).
2. Download the public CA certificate as **`.cer`** (for Windows and Active Directory **Group Policy** — the page lists the exact GPO path under *Trusted Root Certification Authorities*) or **`.crt`** (PEM for macOS, Linux and browsers) and install it on your managed devices.
3. Without the CA installed, browsers still cannot reach the blocked site - they show a certificate warning instead of the custom block page.

You can **replace** or **delete** the Block Page CA here at any time; a CA that is currently in use by an active Application Control rule cannot be deleted until no rule uses it.

## If the browser can no longer connect

The previous web panel certificate is kept. On the console (menu **8) Shell**) run:

```
stratum-cert restore
```

and the panel is served with the previous certificate again.
