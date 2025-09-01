# CCNA SRWE - Semester 2

## 🌐 Module 16: Troubleshoot Static and Default Routes

### 🎯 Objectives

The objective of this module is to troubleshoot static and default route configurations, ensuring reliable packet delivery across the network.

---

## 📦 16.1 Packet Processing with Static Routes

### 🔹 How Routers Process Packets

When a packet arrives at a router:

1. The router decapsulates the packet.
2. It searches the routing table for a matching destination network entry.

* **If the destination matches a static route** → the router uses the next-hop IP address or exit interface.
* **If no specific match exists but a default route is configured** → the router forwards the packet using the default static route.
* **If no match exists at all** → the router drops the packet and sends an ICMP unreachable message to the source.

### 🔹 Step-by-Step Forwarding Example

* **R1 receives packet** → Matches static route → Forwards via S0/1/0.
* **R2 receives packet** → Matches route entry → Forwards via S0/1/1.
* **R3 receives packet** → Destination matches directly connected G0/0/0 → Looks up ARP table for PC3.

  * If ARP entry exists → Encapsulates and forwards packet to PC3.
  * If ARP entry does not exist → Sends ARP request → Uses reply to forward packet.

---

## 🛠️ 16.2 Troubleshoot IPv4 Static and Default Route Configuration

### 🔹 Causes of Network Issues

* Interface failure
* Service provider connection drops
* Oversaturated links
* Wrong configurations entered by admins

### 🔹 Common Troubleshooting Commands

| Command                     | Description                                                                    |
| --------------------------- | ------------------------------------------------------------------------------ |
| **ping**                    | Tests Layer 3 connectivity to a destination. Extended pings give more options. |
| **traceroute**              | Verifies the path to a destination using ICMP echo replies.                    |
| **show ip route**           | Displays routing table entries to verify routes.                               |
| **show ip interface brief** | Shows interface status and IP addresses. Useful for Layer 1 & 2 checks.        |
| **show cdp neighbors**      | Displays directly connected Cisco devices to validate physical connectivity.   |

### 🔹 Example: Solving a Connectivity Problem

Problem: PC1 cannot reach PC3.

* Extended pings from R1 G0/0/0 to PC3 fail.
* Pings from R1 to R2 and R3 succeed.
* R2 routing table shows incorrect static route.

**Incorrect Route Example (R2):**

```
S    172.16.3.0/24 [1/0] via 192.168.1.1
```

**Corrected Static Route Command:**

```
R2(config)# ip route 172.16.3.0 255.255.255.0 172.16.2.1
```

After correction → PC1 can reach PC3 successfully.

---


## 📚 Key Takeaways (What I Learned)

1. A host sends packets to the default gateway, which processes and forwards based on routing table entries.
2. Static routes provide specific forwarding paths; default routes serve as a fallback when no match exists.
3. If no match is found, routers drop packets and notify the source using ICMP.
4. At the destination router, ARP resolves MAC addresses for final delivery.
5. Essential troubleshooting commands: **ping, traceroute, show ip route, show ip interface brief, show cdp neighbors**.
6. Properly configured static and default routes ensure correct end-to-end packet delivery.

---

## 📝 Final Word

This module emphasized how static and default routes impact packet forwarding and how misconfigurations can break connectivity. By mastering troubleshooting commands and understanding packet flow, a network administrator can quickly isolate and resolve routing problems, ensuring reliable communication across the network.
