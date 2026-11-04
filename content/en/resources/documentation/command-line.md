---
title: "Command Line"
lead: "Master the underlying FreeBSD tools that power Stratum Gateway."
---

While the Stratum panel provides a comprehensive interface for managing your firewall, advanced users will frequently interact with the underlying FreeBSD system via SSH. Stratum does not hide the operating system from you; it builds upon it.

Here are the essential command-line tools you should know when managing a Stratum Gateway.

## 1. pfctl (Packet Filter Control)

The `pfctl` command is the interface to the `pf` firewall. You can use it to check rules, view states, and see statistics.

- **View the active ruleset:**
  ~~~bash
  sudo pfctl -sr
  ~~~
  *(Because Stratum uses declarative config, you should see your rules here after clicking "Apply" in the UI).*

- **View active connections (states):**
  ~~~bash
  sudo pfctl -ss
  ~~~

- **Show firewall statistics:**
  ~~~bash
  sudo pfctl -si
  ~~~

- **Test a configuration file for syntax errors:**
  ~~~bash
  sudo pfctl -nf /etc/pf.conf
  ~~~

## 2. ifconfig (Interface Configuration)

Use `ifconfig` to view the status of your network interfaces, check IP addresses, and see MAC addresses.

- **List all interfaces:**
  ~~~bash
  ifconfig -a
  ~~~

- **Show details for a specific interface (e.g., em0):**
  ~~~bash
  ifconfig em0
  ~~~

## 3. netstat & sockstat

These tools are essential for troubleshooting network connections and seeing which services are listening on which ports.

- **Show listening TCP/UDP ports:**
  ~~~bash
  sockstat -4 -l
  ~~~

- **Show routing table:**
  ~~~bash
  netstat -rn
  ~~~

## 4. service (Service Management)

FreeBSD uses the `service` command to manage rc.d scripts.

- **Check the status of a service (e.g., WireGuard):**
  ~~~bash
  sudo service stratum_wireguard status
  ~~~

- **Restart the panel (if you need to apply manual backend changes):**
  ~~~bash
  sudo service stratum-panel restart
  ~~~

## 5. tcpdump (Packet Capture)

For deep packet analysis, `tcpdump` is built-in. You can also run this from the UI's Diagnostics page.

~~bash
sudo tcpdump -i em0 -n port 80
~~

## Next Steps

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>File &amp; Path Reference</h3>
    <p>Learn where Stratum stores its configuration and logs.</p>
    <a href="/resources/documentation/file-paths/" class="card-link">File Reference →</a>
  </div>
  <div class="card">
    <h3>Need help?</h3>
    <p>If you are stuck, reach out to the team.</p>
    <a href="/company/contact/" class="card-link">Contact Us →</a>
  </div>
</div>
