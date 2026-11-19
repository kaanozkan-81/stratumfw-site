---
title: "Diagnostics > Packet Capture"
seo_title: "10.6 Diagnostics > Packet Capture — Stratum FW User Manual"
description: "Recording the packets of an interface into a .pcap file for Wireshark: filters, limits, leaving out your own session, downloading and privacy."
weight: 1006
chapter_num: "10.6"
url: "/manual/diagnostics-packet-capture/"
aliases:
  - "/manual/10-06-diagnostics-packet-capture/"
translationKey: "manual-10-06-diagnostics-packet-capture"
---

Packet capture records the packets of one interface into a file that you open in **Wireshark** on your computer.
It is the last word when other tools do not explain a problem: you see exactly what is sent and received - for
example whether a VoIP phone gets an answer from its provider, or whether a server replies at all.

![Diagnostics > Packet Capture](/images/manual/pcap.png)

## Capture

| Field | Meaning |
| :--- | :--- |
| **Interface** | The interface to capture on. |
| **Protocol** | **Any**, **TCP**, **UDP** or **ICMP**. |
| **Host or network** | Optional: only packets from or to this address or network, for example `192.168.1.20` or `10.0.0.0/24`. |
| **Port** | Optional: only packets from or to this port, for example `443`. |
| **Stop after (seconds)** | 10-600, default 60. |
| **or this size (MB)** | 1-100, default 20 - whichever comes first. A capture also stops at one million packets. |
| **Leave out my own panel and SSH traffic** | On by default, so the capture is not filled with your own session. |

**Start capture**. The page shows the progress; **Stop** ends it early.

Use a host or a port whenever you can: a busy WAN fills 20 MB in seconds.

**Which interface?** To see what a device sends, capture on its internal interface with the device's address as
host. To see what leaves the firewall for the internet - after NAT - capture on the WAN.

## Last capture

**Last capture** shows the interface, size, duration, result (why it ended), who started it and the filter. **Download .pcap** saves it; **Delete**
removes it.

One capture at a time. The file is kept until it is deleted, the next capture starts, or after 24 hours.

## Privacy

A capture contains **the traffic itself**: unencrypted content and the passwords of old protocols (FTP, Telnet,
plain HTTP). Only administrators can capture and download, every start, stop and download is written to the system
log, and the file is stored in a folder only root can read. Delete a capture when you are done, and do not send it
to others without need.
