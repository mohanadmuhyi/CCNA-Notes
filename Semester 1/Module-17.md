# 📦 Module 17: Build a Small Network

## 🎯 Objectives

Implement a network design for a small network including a router, switch, and end devices.



---

## 📘 17.1 Devices in a Small Network

### 🧱 Small Network Topologies

* Small networks are common in businesses.
* Typically have one WAN connection (DSL, Cable, Ethernet).
* Managed by local IT or contractors.

### 🧩 Device Selection Factors

* Cost
* Speed and types of ports/interfaces
* Expandability
* Operating system features/services

### 📑 IP Addressing Plan

Plan and document IP addresses by:

* End user devices
* Servers and peripherals (e.g., printers, cameras)
* Intermediary devices

### ♻️ Redundancy

* Eliminate single points of failure.
* Add duplicate equipment or network links.

### 🚦 Traffic Management

* Use QoS for real-time traffic (voice/video).
* Priority queuing: high-priority queues always emptied first.

---

## 📘 17.2 Small Network Applications and Protocols

### 🔌 Network Applications vs. Services

* **Network Applications**: Communicate with protocol stack directly (e.g., browser).
* **Application Layer Services**: Interface non-network-aware apps with the network.

### 🌐 Common Protocols

| Protocol           | Purpose                       |
| ------------------ | ----------------------------- |
| Telnet / SSH       | Remote access (SSH is secure) |
| HTTP / HTTPS       | Web access                    |
| SMTP / POP3 / IMAP | Email transfer/retrieval      |
| FTP / SFTP         | File transfer                 |
| DHCP               | Auto IP assignment            |
| DNS                | Domain name resolution        |



### 📞 Voice and Video Applications

* Use IP telephony or VoIP.
* VoIP is cheaper but lower quality.
* Real-time apps require QoS.
* Protocols: RTP and RTCP

---

## 📘 17.3 Scale to Larger Networks

### 📈 Requirements to Scale

* Network documentation (topologies)
* Device inventory
* Budget
* Traffic analysis

### 🧪 Protocol Analysis

* Capture traffic at peak times.
* Use tools to analyze type, flow, and volume.

### 👩‍💻 Monitoring Employee Utilization

* OS tools: show OS version, CPU, RAM, drive, applications.
* Useful for future scaling decisions.

---

## 📘 17.4 Verify Connectivity

### 📶 Ping

* Tests Layer 3 connectivity.
* Uses ICMP Echo (type 8) and Echo Reply (type 0).

#### 🧭 IOS Ping Indicators

| Symbol | Meaning                 |
| ------ | ----------------------- |
| `!`    | Echo reply received     |
| `.`    | Timeout                 |
| `U`    | Destination unreachable |

### 📌 Extended Ping

* `ping` (no address) to enter interactive mode
* `ping ipv6` for IPv6 extended mode

### 🛰️ Traceroute

* Shows path (hops) through network
* Windows: `tracert`, Cisco: `traceroute`
* Timeout: `*` indicates no response
* Extended version: `traceroute` (no IP)

### 🧾 Network Baseline

* Document results from `ping`, `traceroute`, etc.
* Timestamp and archive for troubleshooting

---

## 📘 17.5 Host and IOS Commands

### 💻 Windows Host

* `ipconfig` – Displays IP settings
* `ipconfig /all` – Detailed IP configuration
* `ipconfig /release` – Releases IP address
* `ipconfig /renew` – Renews IP address
* `ipconfig /displaydns` – Shows DNS cache

### 🐧 Linux Host

* `ifconfig` – Show or configure interfaces (deprecated)
* `ip address` – Show IP addresses (preferred over ifconfig)

### 🍏 macOS Host

* `ifconfig` – Show interface details
* `networksetup -listallnetworkservices` – List all services
* `networksetup -getinfo <service>` – Get info on specific service

### 🧠 ARP Commands

* `arp -a` – Displays ARP cache table
* `netsh interface ip delete arpcache` – Clears ARP cache

### 🔍 Common IOS `show` Commands

* `show running-config` – View current config in RAM
* `show interfaces` – Detailed interface statistics
* `show ip interface` – IPv4-specific interface status
* `show ipv6 interface` – IPv6-specific interface status
* `show ip interface brief` – Summary of interfaces (IPv4)
* `show ipv6 interface brief` – Summary of interfaces (IPv6)
* `show ip route` – IPv4 routing table
* `show ipv6 route` – IPv6 routing table
* `show version` – IOS version and hardware info
* `show arp` – ARP cache info
* `show protocols` – Active routed protocols and interface status
* `show cdp neighbors` – Summary of connected Cisco devices
* `show cdp neighbors detail` – Detailed info of connected Cisco devices

---

## 📘 17.6 Troubleshooting Methodologies

### 🧭 Six-Step Process

1. Identify the problem
2. Establish a theory of probable cause
3. Test the theory
4. Implement solution
5. Verify and prevent
6. Document findings

### 🚨 Escalate if:

* Higher expertise or permissions required

### 🧮 `debug` Command

* Used to monitor internal IOS events in real time
* Turn off: `undebug all`

### 🖥️ `terminal monitor`

* Show log/debug messages on remote terminals

---

## 📘 17.7 Troubleshooting Scenarios

### ⚙️ Common Issues & Mitigations:

* **Duplex mismatch**

  * 🔍 **Cause**: Incorrect settings or autonegotiation failure
  * ✅ **Mitigation**: Manually set correct duplex on both ends or ensure autonegotiation works

* **IP addressing**

  * 🔍 **Cause**: Manual input errors, invalid subnet, DHCP failure
  * ✅ **Mitigation**: Verify IP settings, renew DHCP lease, check subnet mask and gateway

* **Default gateway**

  * 🔍 **Cause**: Incorrect/missing gateway blocks remote access
  * ✅ **Mitigation**: Ensure correct gateway IP is configured and reachable

* **DNS**

  * 🔍 **Cause**: Wrong DNS settings or unreachable server
  * ✅ **Mitigation**: Use alternate DNS, verify connectivity to DNS server, check `/etc/resolv.conf` (Linux)

#### 🛠️ Useful Commands

* Windows: `ipconfig`, `ipconfig /all`, `nslookup`
* IOS: `show ip interface brief`, `show ip route`

### ⚠️ APIPA

* Windows assigns 169.254.0.0/16 when no DHCP is found

---

## 🧠 NetAcad Questions

### 📊 Small Network Fundamentals

* Most businesses are small, so network design should reflect simplicity and reliability.
* Cost is a major factor in device selection.
* A well-planned IP addressing scheme ensures efficient communication.
* Redundancy eliminates single points of failure.
* QoS classifies traffic based on priority for better service.

### 🌐 Applications and Protocols

* Network access is enabled by application layer services and network applications.
* Telnet and SSH allow remote connections; SSH is encrypted, Telnet is not.

### 📈 Scaling Networks

* Scaling requires documentation and budgeting.
* Tools like **Wireshark** help visualize traffic.
* Windows' **Data Usage** identifies apps using bandwidth.

### 🛠️ Troubleshooting Methodologies

* After establishing a theory of cause, always test it.
* Major hardware replacements should be escalated before implementation.
* Commands to stop debug: `undebug all` or `no debug ip icmp`
* `terminal monitor` enables debug output on remote sessions.

---

## 🏁 Final Words

This module emphasizes practical skills in network setup, command-line usage, connectivity tests, and layered security concepts—all crucial for building, securing, and troubleshooting small networks.
