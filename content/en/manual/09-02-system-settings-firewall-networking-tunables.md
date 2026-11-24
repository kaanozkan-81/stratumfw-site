---
title: "System > Settings: Firewall & NAT, Networking and Tunables"
seo_title: "09.2 System > Settings: Firewall & NAT, Networking and Tunables — Stratum FW User Manual"
description: "How blocked connections are answered and logged, bogon and private networks, WAN ping, the state table and timeouts, SYN flood protection, packet normalisation, NAT reflection, static port, hardware offload, CPU power, ARP logging and sysctl tunables."
weight: 902
chapter_num: "09.2"
url: "/manual/system-settings-firewall-networking-tunables/"
aliases:
  - "/manual/09-02-system-settings-firewall-networking-tunables/"
translationKey: "manual-09-02-system-settings-firewall-networking-tunables"
---

These three tabs hold the firewall's advanced settings. The defaults are safe for most networks: **change one
setting at a time** and watch the firewall log and the dashboard before the next.

## Firewall & NAT

![System > Settings > Firewall & NAT](/images/manual/set-firewall.png)

### Filtering

| Setting | Default | Meaning |
| :--- | :--- | :--- |
| **Blocked connections** | Drop silently | **Drop silently**: nothing is sent back; scanners see no answer at all. **Reject**: the sender gets a TCP reset or ICMP unreachable at once - devices in your own networks then fail fast instead of waiting. Rules with the action *Reject* always reject. |
| **Log packets blocked by the default rule** | on | Packets that no rule allowed appear in the firewall log. |
| **Also on the WAN** | on | Off: internet scans and late replies of closed connections arriving on the WAN are blocked without a log line - a much quieter log. Blocks on internal interfaces and VPNs are still logged. |
| **Log blocked broadcast and multicast noise** | off | Broadcasts of the local networks (discovery protocols and the like) are normally blocked without a log line. |
| **Block bogon networks (from the internet)** | on | Packets from reserved and unassigned addresses arriving on the WAN. |
| **Block private networks (from the internet)** | off | 10/8, 172.16/12, 192.168/16, 100.64/10 arriving on the WAN. **Leave off when the WAN is behind another router or modem with a private address.** |
| **Answer ping on the WAN address** | on | Useful for monitoring and troubleshooting; off makes the firewall invisible to ping. |

### State table and performance

| Setting | Meaning |
| :--- | :--- |
| **Maximum connections (states)** | How many connections the firewall can track at once. Empty or 0: the pf default (100,000). The page suggests a value for the installed memory and shows how many are in use now. Raise it for large guest networks. |
| **Timeouts for idle connections** | **Normal** (default); **High latency** for satellite links; **Conservative** keeps idle connections (SSH, VPN) alive longer; **Aggressive** frees idle connections sooner - helps when many guests open many connections. |
| **SYN flood protection** | SYN cookies when the state table fills up. Off by default. |
| **Randomise the IP ID of outgoing packets** | Hides the operating system of devices behind the firewall. Off by default. |
| **Clear the don't-fragment bit** | Only for devices that fail with fragmented packets, for example some NFS clients. Off by default. |

### NAT

| Setting | Meaning |
| :--- | :--- |
| **NAT reflection** | Local devices can use the public address of a port forward - `https://<public IP>` from the LAN reaches the internal server. Off by default. A host override on [**Services > DNS Resolver**](/manual/services-dns-resolver/) is usually the cleaner solution. |
| **Keep source ports in outbound NAT (static port)** | Outbound NAT keeps the source port unchanged. Only for applications that need it (some VoIP and game consoles); off by default. |

Everything on this tab takes effect with **Apply changes** - pf is reloaded and open connections stay up.

## Networking

![System > Settings > Networking](/images/manual/set-networking.png)

### Hardware offload

Network cards can take work off the CPU. On a firewall some of it does more harm than good: LRO merges packets
that are then forwarded, and some drivers mishandle TSO or checksums with VLANs and pf.

| Setting | Meaning |
| :--- | :--- |
| **Switch off LRO (large receive offload)** | Recommended on a router. |
| **Switch off TSO (TCP segmentation offload)** | When throughput through the firewall is poor or connections stall. |
| **Switch off checksum offload** | Only if a card corrupts packets. |

They apply to every physical port (VLANs use their parent port); the ports are not restarted. All off by default
(the card's settings are kept).

### CPU power

**CPU frequency**: **Not managed** (system default), **Adaptive** (fast under load, lower when idle),
**Maximum performance**, or **Economy** (lowest power).

### ARP and IPv6

| Setting | Meaning |
| :--- | :--- |
| **Log when an address moves to another MAC address** | A hint of ARP spoofing - or of a device replaced. On by default. |
| **Log ARP replies on the wrong interface** | A hint of a cabling or VLAN mistake. On by default. |
| **Forget ARP entries after ... seconds** | Default 1200. |
| **IPv6** | Not supported yet: all IPv6 traffic is blocked and IPv6 is off on every interface. Support arrives in phases. |

Changes take effect with **Apply changes** and are reverted automatically unless you keep them.

## Tunables

![System > Settings > Tunables](/images/manual/set-tunables.png)

System values (sysctl) that matter on a firewall, each with the value pfSense uses as a recommendation:

| Tunable | Recommended | What it does |
| :--- | :--- | :--- |
| `net.inet.tcp.blackhole` | 2 | No reset for TCP to closed ports of the firewall itself (hides it from scans) |
| `net.inet.udp.blackhole` | 1 | No ICMP port-unreachable for UDP to closed ports of the firewall itself |
| `net.inet.ip.random_id` | 1 | Random IP ID for packets the firewall itself sends |
| `net.inet.tcp.drop_synfin` | 1 | Drop TCP packets with SYN and FIN set (a scanner trick) |
| `net.inet.icmp.drop_redirect` | 1 | Ignore ICMP redirects |
| `net.inet.ip.redirect` | 0 | Do not send ICMP redirects |
| `net.inet.tcp.syncookies` | 1 | SYN cookies for the firewall's own TCP services |
| `net.inet.icmp.icmplim` | 200 | ICMP error replies per second |
| `net.inet.ip.intr_queue_maxlen` | 1000 | Input queue for routed packets (bursts of small packets) |
| `kern.ipc.soacceptqueue` | 1024 | Pending connections per listening socket (panel, DNS) |
| `kern.ipc.maxsockbuf` | 4262144 | Largest socket buffer (fast VPN and large transfers) |

For each: **Default** leaves the system value; a value set here is applied at once with **Apply** and kept after a
reboot. Switched back to default, the value it had before is restored. **Use all recommended values** sets them
all.

**Own tunables** - **Add tunable** with a name, value and description. Only run-time sysctls are accepted (loader
tunables need a reboot and are not offered), and values that Stratum FW manages itself (forwarding, pf, security
levels ...) are refused.
