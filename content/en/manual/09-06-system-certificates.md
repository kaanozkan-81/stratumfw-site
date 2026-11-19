---
title: "System > Certificates"
seo_title: "09.6 System > Certificates — Stratum FW User Manual"
description: "The web panel's TLS certificate: the self-signed certificate, installing your own certificate from a company CA or a provider, the browser warning, expiry and restoring the previous certificate."
weight: 906
chapter_num: "09.6"
url: "/manual/system-certificates/"
aliases:
  - "/manual/09-06-system-certificates/"
translationKey: "manual-09-06-system-certificates"
---

The web panel is always served over HTTPS. **System > Certificates** controls the certificate the browser sees.

![System > Certificates](/images/manual/certs.png)

## The current certificate

**Web panel certificate** shows who it was issued to, the names and addresses it covers, the issuer, its validity
and the days left - marked when fewer than 21 days remain or the certificate has expired - and its **SHA-256
fingerprint**.

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

To get rid of the warning without a commercial certificate, trust the firewall's certificate on the computers
that manage it: export it from the browser (certificate details > export) and import it as a trusted root
certificate - on Windows into *Trusted Root Certification Authorities*, on macOS into the keychain with *Always
trust*. MCP clients need the same (see [**System > Settings: SNMP and MCP**](/manual/system-settings-snmp-mcp/)).

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

## If the browser can no longer connect

The previous certificate is kept. On the console (menu **8) Shell**) run:

```
stratum-cert restore
```

and the panel is served with the previous certificate again.
