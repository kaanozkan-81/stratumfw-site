---
title: "Terms of Use"
lead: "Terms governing the use of stratumfw.com, the Stratum Firewall software, and related services."
---

## 1. Free Provision & Software Licensing

Stratum Firewall is provided **free of charge** (`$0/year`, with no recurring subscriptions or feature paywalls). Its control-plane and web panel source code is not yet published; the software license for those sources will be announced when they are published.

All open-source operating system, kernel, cryptographic, and networking components on which Stratum Firewall is built — including **FreeBSD**, **pf**, **dummynet**, **OpenZFS**, **Unbound**, **dnsmasq**, **WireGuard**, **strongSwan**, **OpenVPN**, **Suricata**, **OpenSSL**, **Nginx**, and **Python** — remain governed by their respective open-source licenses.

## 2. Permitted Use & Restrictions

You may download, install, and operate Stratum Firewall installer images and signed updates on physical or virtual network appliances for personal, educational, homelab, or commercial network infrastructure at no cost. Until the Stratum control-plane source code and its formal license are published, you may not redistribute modified Stratum Firewall binary packages as an official Stratum release or misrepresent proprietary third-party forks as official Stratum builds.

## 3. No Warranty ("As-Is")

Stratum Firewall is under active pre-`v1.0` engineering development and is provided **"AS IS"**, without warranty of any kind, express or implied, including but not limited to warranties of merchantability, fitness for a particular purpose, or uninterrupted operation. Network administrators remain responsible for maintaining configuration backups (`.stbk`), verifying firewall rules, and testing updates prior to deployment in critical environments.

## 4. Limitation of Liability

In no event shall the authors or maintainers of Stratum Firewall be liable for any direct, indirect, incidental, special, exemplary, or consequential damages (including network downtime, data loss, or business interruption) arising out of the use or inability to use the software.

## 5. Security & Contact

Vulnerability disclosures and legal inquiries may be directed to **security@stratumfw.com** (see [`/.well-known/security.txt`](/.well-known/security.txt) and [Security & Verification](/product/security/)).
