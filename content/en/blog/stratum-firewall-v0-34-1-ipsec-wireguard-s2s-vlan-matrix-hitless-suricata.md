---
title: "Stratum Firewall: Site-to-Site IPsec, Dedicated WireGuard S2S, 30-Day VLAN Traffic Matrix, and Hitless Suricata Updates"
seoTitle: "Stratum FW: IPsec & WireGuard S2S VPN, VLAN Matrix"
date: 2026-11-17
author: "Stratum Security"
description: "Stratum Firewall delivers multi-vendor Site-to-Site IPsec (IKEv2 & IKEv1), dedicated kernel WireGuard S2S interfaces, a 30-day Inter-VLAN traffic matrix, and hitless Suricata 8 updates."
seoDescription: "Deploy multi-vendor IKEv2/IKEv1 IPsec, dedicated kernel WireGuard S2S (wg1+), a 30-day Inter-VLAN traffic matrix, and hitless Suricata 8 IPS on FreeBSD 15.1."
translationKey: "blog-v0-34-1"
tags: ["stratum firewall", "ipsec", "wireguard", "freebsd 15.1", "suricata"]
---

Between and, [Stratum Firewall](/) evolved from a single-site perimeter appliance into a multi-branch security and routing hub. Built on **FreeBSD 15.1-RELEASE**, stateful `pf`, OpenZFS, and an asynchronous Python 3.12 / FastAPI control plane, this release cycle introduces multi-vendor **Site-to-Site IPsec**, isolated **WireGuard Site-to-Site (`wg1+`)** interfaces, a **30-day Inter-VLAN Traffic Matrix**, and zero-flap **Suricata 8** engine updates. Full version-by-version notes are available in the [Official Changelog](/download/#changelog).

## 1. Multi-Vendor Site-to-Site IPsec (`IKEv2` & `IKEv1`)

Enterprise branch networks rarely run a single vendor end to end. Under **VPN -> IPsec**, Stratum Firewall combines `strongSwan` key negotiation with native FreeBSD kernel ESP encryption (`enc0`) so headquarters and remote branches can interconnect directly with **FortiGate**, **Cisco IOS/ASA**, **OPNsense**, **pfSense**, and cloud VPC gateways:

* **Modern & Legacy Cipher Profiles:** Supports `IKEv2` as well as legacy `IKEv1` (Main and Aggressive modes) with `AES-256-GCM`, `AES-CBC`, `SHA-2`, Diffie-Hellman groups `14` through `21`, `Curve25519`, Perfect Forward Secrecy (PFS), Dead Peer Detection (DPD), and automatic UDP `4500` `NAT-T` encapsulation.
* **Multi-Subnet Phase 2 Selectors & `enc0` Visibility:** Define multiple local and remote CIDR pairs per tunnel while filtering decrypted branch traffic cleanly on FreeBSD's `enc0` virtual interface.
* **Automatic NAT Exemption, MSS Clamping & Peer Lockdown:** Private branch traffic is automatically exempted from WAN outbound NAT, TCP `SYN` segments are clamped to `1360` bytes to prevent PMTUD blackholes, pre-shared keys stay isolated in `0600` root-only secret files, and WAN `UDP 500/4500` + `ESP` ports open strictly to configured peer endpoints.

<figure style="margin:1.75rem 0;background:#08101f;border:1px solid #1e293b;border-radius:14px;overflow:hidden;box-shadow:0 18px 40px -12px rgba(15,23,42,0.35);">
  <img src="/images/features/s2s-vpn-topology.jpg" alt="Stratum Firewall Multi-Vendor IPsec and WireGuard Site-to-Site Hub Architecture" loading="lazy" style="display:block;width:100%;height:auto;">
  <figcaption style="padding:0.75rem 1rem;font-size:0.84rem;color:#cbd5e1;text-align:center;border-top:1px solid #1e293b;">Multi-vendor Site-to-Site hub interconnecting FortiGate, Cisco, cloud VPCs, MikroTik RouterOS 7, and remote Stratum / OPNsense gateways.</figcaption>
</figure>

## 2. Dedicated Kernel WireGuard Site-to-Site (`wg1, wg2...`)

Prior to, remote road-warrior clients shared `wg0`. Branch-to-branch links now run on dedicated FreeBSD kernel `if_wg` interfaces (`wg1`, `wg2`, and beyond) under **VPN -> WireGuard -> Site-to-Site**, keeping mobile user VPN pools completely untouched:

* **Verified Behind NAT & Against MikroTik RouterOS 7:** Tested in production topologies behind carrier/upstream NAT against MikroTik RouterOS 7 (`RB4011` and CCR series) with persistent keepalives, optional pingable `/30` transit tunnel addresses, and hitless `wg syncconf` configuration reloads.
* **Strict `AllowedIPs` & Route-Overlap Guards:** Each peer enforces exact remote subnet `AllowedIPs` selectors at the kernel cryptokey routing layer, while pre-apply validation blocks overlapping CIDR claims that could otherwise hijack local VLANs or road-warrior VPN pools.

<figure style="margin:1.75rem 0;background:#08101f;border:1px solid #1e293b;border-radius:14px;overflow:hidden;box-shadow:0 18px 40px -12px rgba(15,23,42,0.35);">
  <img src="/images/features/s2s-vpn-panel.jpg" alt="Stratum Firewall Live IPsec and WireGuard Site-to-Site Tunnel Status Panel" loading="lazy" style="display:block;width:100%;height:auto;">
  <figcaption style="padding:0.75rem 1rem;font-size:0.84rem;color:#cbd5e1;text-align:center;border-top:1px solid #1e293b;">Unified tunnel telemetry displaying live IKEv2, IKEv1, and WireGuard S2S handshakes alongside kernel crypto status.</figcaption>
</figure>

## 3. 30-Day Inter-VLAN & VPN Traffic Matrix

Designing internal micro-segmentation rules is risky when you only see short real-time log tails. Under **Diagnostics -> Firewall Log -> Between networks**, Stratum Firewall aggregates a rolling **30-day Inter-VLAN Traffic Matrix** across local VLANs, `enc0` IPsec subnets, and `wg1+` WireGuard branch interfaces:

* **Observed East-West Dependency Mapping:** Inspect exact protocol, port, and hit counts between internal network pairs over a full monthly cycle before tightening `pf` block rules.
* **Zero External Collector Required:** Stored locally on OpenZFS with bounded retention so branch appliances maintain full visibility without shipping gigabytes of syslog data offsite.

## 4. Hitless Suricata 8 Updates, Rule Separators & Refined Light Theme

Day-to-day operational polish received equal focus across this release series:

* **Zero WAN Port Flaps on Updates:** System package upgrades and Suricata 8 IPS/IDS reloads preserve `igb` and `netmap` NIC hardware offload states, eliminating link renegotiation drops on active WAN/LAN trunks and adding a clear banner whenever pending `pf` changes await **Apply**.
* **Colored Rule Separators & Scoped Views:** Organize large firewall and NAT policies with pfSense-style colored visual separators and switch instantly between **All**, **Default**, and **Custom** rule tabs.
* **Softer Off-White Light Theme & Hardened Sessions:** A low-glare daylight palette makes long policy audits comfortable alongside dark mode, while HTTPS sessions enforce `Secure` cookie attributes.
* **Richer OPNsense & Multi-WAN OpenVPN Tooling:** Direct OPNsense `config.xml` migration imports Unbound DNSBL blocklists and full-span DHCP pools, and Multi-WAN OpenVPN client exports include ordered backup WAN `remote` entries automatically.

## Download or Explore the Live Demo

Fresh offline installers (`.iso` for VGA consoles/hypervisors and `.img` for `115200 8N1` headless serial appliances) are ready on the [Download page](/download/), while existing deployments can upgrade in place from **System -> Firmware** with automatic ZFS Boot Environment rollback protection. You can also explore the interface right now in your browser via the [Interactive Live Demo](https://demo.stratumfw.com) or review full architecture specs on the [Features page](/product/features/#s2s-vpn).
