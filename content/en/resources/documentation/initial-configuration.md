---
title: "Initial Configuration"
lead: "Set up your firewall after first boot — interfaces, routing, and administrator access."
---

Once you have successfully deployed the Stratum panel and logged in for the first time, the system is running but not yet ready to route traffic. This guide walks through the essential first steps to bring your firewall online securely.

## 1. Change the Default Credentials

For security, the first time you log into the panel (using the default `admin` credentials printed in the console), you will be forced to change your password. 

- Use a strong, unique password (minimum 12 characters).
- Once changed, the system will unlock the rest of the dashboard.

## 2. Interface Assignments (WAN & LAN)

Navigate to **Interfaces → Assignments**. Stratum uses the FreeBSD `pf` engine, so interfaces must be assigned specific roles before rules can be applied.

- **WAN:** Select the physical interface connected to your modem or ISP.
- **LAN:** Select the physical interface connected to your internal network switch.
- **OPT:** (Optional) Assign any additional interfaces for DMZ, VLANs, or a secondary WAN.

Click **Save**. Because this is a critical change, the **Safe-Apply** mechanism will trigger. You must confirm connectivity within 60 seconds, or the system will revert the changes automatically to prevent a lockout.

## 3. WAN Configuration

Navigate to **Interfaces → WAN**.

You must configure how the firewall obtains its public IP address:
- **DHCP:** (Most common) The firewall will automatically request an IP from your ISP.
- **Static IP:** Enter the IP, Subnet Mask, and Gateway provided by your ISP.
- **PPPoE:** Enter the username and password provided by your ISP (common for DSL/Fiber).

Apply the changes. Check the **Dashboard** to verify that the WAN interface shows an "Up" status and has received an IP address.

## 4. LAN & DHCP Setup

Navigate to **Services → DHCP**.

By default, Stratum will run a DHCP server on the LAN interface so that devices (laptops, phones) can automatically get an IP address and access the internet.

- Define your LAN subnet (e.g., `192.168.1.1/24`).
- Define the DHCP pool (e.g., `192.168.1.100` to `192.168.1.200`).
- Set your DNS servers (usually the firewall's LAN IP).

## 5. Role-Based Access Control (RBAC)

Stratum is built for teams. Navigate to **System → Users** to add other administrators. You should never share the `admin` account.

Create individual users and assign them one of three roles:
- **Admin:** Full access to configure firewall rules, VPNs, and system settings.
- **Operator:** Can view dashboards, run diagnostics (ping/traceroute), and manage active states, but cannot change firewall rules.
- **Read-Only:** Can view the dashboard and logs but cannot make any changes.

## 6. Enable Threat Intelligence

Now that the network is live, protect it. Navigate to **Security → Blocklists**.

- Enable **IP Blocklists** to automatically download known malicious IPs into `pf` tables.
- Enable **DNSBL** to block ads, trackers, and malware domains at the resolver level.

## Next Steps

Your firewall is now routing traffic and protecting your network. From here, you can explore:

<div class="card-grid grid-2" style="margin-top:32px;">
  <div class="card">
    <h3>Set up VPN</h3>
    <p>Configure WireGuard for secure remote access.</p>
    <a href="/resources/documentation/wireguard-vpn/" class="card-link">WireGuard Guide →</a>
  </div>
  <div class="card">
    <h3>Master the Firewall</h3>
    <p>Learn how to write complex pf rules using Safe-Apply.</p>
    <a href="/resources/documentation/pf-rules/" class="card-link">pf Rules Guide →</a>
  </div>
</div>
