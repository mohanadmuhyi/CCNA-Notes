# CCNA ENSA - Semester 3
## 🧩 Module 12: Network Troubleshooting

### 🎯 Objectives

Troubleshoot enterprise networks effectively using documentation, systematic methods, and troubleshooting tools.

---

## 🗂️ 12.1 Network Documentation

### 📘 Overview

Accurate network documentation is essential for monitoring and troubleshooting. It should include:

* **Physical and Logical Topology Diagrams**
* **Device Documentation** (hardware, software, interfaces, configurations)
* **Network Performance Baseline** (normal network behavior)

> Keep documentation in one central location with a backup stored elsewhere.

### 🧭 Network Topology Types

* **Physical Topology:** Physical connections between devices (cables, ports).
* **Logical Topology:** Network flow and addressing structure.

### 🖥️ Network Device Documentation

Maintain up-to-date details for:

* **Routers**: Interfaces, IPs, routing protocols.
* **Switches**: VLANs, trunks, MAC tables.
* **End Devices**: IPs, hostnames, connected ports.

### 📊 Establish a Network Baseline

A baseline defines normal performance. It helps identify congestion and underused areas.

**Steps to establish a baseline:**

1. **Select Data to Collect:** Start small (e.g., interface utilization, CPU load).
2. **Identify Devices/Ports of Interest:** Focus on key servers, routers, and switches.
3. **Determine Duration:** 2–4 weeks is typical. Minimum seven days long. Repeat annually or periodically.

**🧠 Example:**
During baseline collection, you monitor `GigabitEthernet0/1` of the main router and observe 40% utilization during peak hours. Later, when utilization exceeds 80%, it indicates abnormal congestion, signaling a need for capacity upgrade.

### 🔍 Data Measurement Commands

| Command                                                 | Description                                  |
| ------------------------------------------------------- | -------------------------------------------- |
| `show version`                                          | Displays uptime, version, hardware info      |
| `show ip interface brief` / `show ipv6 interface brief` | Interface summary                            |
| `show interfaces`                                       | Detailed interface statistics                |
| `show ip route` / `show ipv6 route`                     | Routing tables                               |
| `show cdp neighbors detail`                             | Connected Cisco devices info                 |
| `show arp` / `show ipv6 neighbors`                      | ARP or IPv6 neighbor table                   |
| `show running-config`                                   | Current configuration                        |
| `show vlan`                                             | VLAN status                                  |
| `show port`                                             | Port status on a switch                      |
| `show tech-support`                                     | Collects multiple show outputs for reporting |

---

## ⚙️ 12.2 Troubleshooting Process

### 🧩 General Procedures

A structured method reduces time and increases accuracy.

### Seven-Step Troubleshooting Process

1. **Define the Problem**
2. **Gather Information** (use pings, show commands)
3. **Analyze Information** (compare to baseline)
4. **Eliminate Possible Causes**
5. **Propose Hypothesis** (potential solution)
6. **Test Hypothesis** (verify, rollback if needed)
7. **Solve and Document** (inform team, record fix)

### 🗣️ Question End Users

Ask questions like:

* What doesn’t work? When did it start?
* Who is affected?
* Is the issue constant or intermittent?
* Were there any error messages?

### 🔧 Gather Information Commands

| Command                   | Function                     |
| ------------------------- | ---------------------------- |
| `ping`                    | Test connectivity            |
| `traceroute`              | Shows packet path            |
| `telnet` / `ssh`          | Remote access testing        |
| `show ip interface brief` | Interface summary            |
| `show ip route`           | Routing info                 |
| `show protocols`          | Layer 3 status               |
| `debug`                   | Enable live event monitoring |

### 🧱 Layered Troubleshooting Models

Use **OSI** or **TCP/IP** models to isolate problems layer by layer.

### 🧭 Structured Methods

| Method                 | Use Case                            |
| ---------------------- | ----------------------------------- |
| **Bottom-Up**          | Suspect physical issue              |
| **Top-Down**           | Simpler problems or software issues |
| **Divide and Conquer** | Test mid-layer (e.g., Layer 3)      |
| **Follow-the-Path**    | Track actual packet route           |
| **Substitution**       | Replace faulty hardware             |
| **Comparison**         | Compare with working element        |
| **Educated Guess**     | Based on experience                 |

### 📏 Guidelines for Selecting a Troubleshooting Method

* **Physical Issues:** Start Bottom-Up.
* **Software or Application Issues:** Try Top-Down.
* **Complex Network Problems:** Use Divide and Conquer.
* **When Path Is Unknown:** Apply Follow-the-Path.
* **Intermittent Hardware Faults:** Use Substitution.
* **Two Networks with Similar Setup:** Use Comparison.

> 💡 *Troubleshooting improves with experience—every issue solved sharpens your diagnostic skill.*

---

## 🧰 12.3 Troubleshooting Tools

### 🧑‍💻 Software Tools

* **Network Management Systems (NMS):** Monitor devices, faults, and configurations.
* **Knowledge Bases:** Use Cisco or vendor documentation.
* **Baselining Tools:** Automate documentation and performance tracking.

### 📡 Protocol Analyzer

* **Example:** Wireshark.
* Captures and displays data from Layers 1–7 for deep inspection.

### 🔩 Hardware Tools

| Tool                          | Function                                  |
| ----------------------------- | ----------------------------------------- |
| **Digital Multimeter**        | Measures voltage/current/resistance       |
| **Cable Tester**              | Tests data cabling                        |
| **Cable Analyzer**            | Certifies copper/fiber cables             |
| **Portable Network Analyzer** | Troubleshoots VLANs and switched networks |
| **Cisco Prime NAM**           | Web-based network analysis tool           |

### 🧾 Syslog Server

* Syslog enables routers and switches to send system log messages to a **central server**.
* Logs can be directed to **console, buffer, VTY, or remote server**.
* Severity levels (0–7): **0=Emergencies**, **7=Debugging**.
* Default level shown on console: **Informational (6)**.

**Useful Commands:**

```bash
Router(config)# logging host 192.168.1.10      # Sets syslog server IP
Router(config)# logging trap warnings          # Sends warnings and above
Router(config)# service timestamps log datetime msec
Router# show logging                           # Displays log buffer
```

> 💡 Lower level number = higher severity.

---

## 🧠 12.4 Symptoms and Causes of Network Problems

### ⚡ Physical Layer

**Common Symptoms:**

* Performance lower than baseline.
* Packet drops, intermittent disconnections.
* Console errors and link down messages.
* High CPU utilization on devices.

**Common Causes:**

* Power or grounding issues.
* Faulty NICs, cables, or connectors.
* Excessive attenuation or interference (EMI, crosstalk).
* Incorrect clock rate or interface disabled.

**Useful Commands:**

```bash
show interfaces
show controllers
show environment all
```

### 🔁 Data Link Layer

**Symptoms:** Slow frame forwarding, STP instability, or excessive broadcasts.
**Causes:**

* Encapsulation or framing mismatch.
* STP loops or topology changes.
* Duplex mismatch or VLAN misconfiguration.
* Address mapping errors (ARP issues).

**Useful Commands:**

```bash
show interfaces trunk
show spanning-tree
show vlan brief
show mac address-table
```

### 🌐 Network Layer

**Symptoms:** Missing routes, unreachable subnets, routing loops.
**Causes:**

* Routing protocol misconfiguration (EIGRP/OSPF).
* Neighbor adjacency failure.
* Incorrect static routes or summarization.
* ISP or hardware link issues.

**Useful Commands:**

```bash
show ip route
show ip protocols
show ip ospf neighbor
show ip eigrp neighbors
```

### 🚪 Transport Layer

**ACL Troubleshooting:**

* Wrong direction/interface.
* ACEs ordered incorrectly.
* Implicit deny at end.
* Wrong ports or protocols.

**NAT Troubleshooting:**

* DHCP/DNS/SNMP failing due to translation.
* Use `ip helper-address` to assist protocols across subnets.

**Useful Commands:**

```bash
show access-lists
show ip nat translations
show ip nat statistics
```

### 💬 Application Layer

**Symptoms:** Applications unreachable, timeouts, failed authentication.
**Causes:** Service stopped, incorrect DNS resolution, or firewall blocks.

**Useful Commands:**

```bash
show tcp brief all
show control-plane host open-ports
nslookup [hostname]
```

| Protocol          | Purpose               |
| ----------------- | --------------------- |
| **SSH/Telnet**    | Remote device access  |
| **HTTP/FTP/TFTP** | Web and file transfer |
| **SMTP/POP**      | Email delivery        |
| **SNMP**          | Network monitoring    |
| **DNS**           | Hostname resolution   |
| **NFS**           | Remote file system    |

---

## 🌍 12.5 Troubleshooting IP Connectivity

### Step-by-Step Details

#### 1️⃣ Verify the Physical Layer

Check cables, interfaces, and hardware status.

```bash
show interfaces status
show interfaces
ping <neighbor IP>
```

Look for errors, collisions, or link down states.

#### 2️⃣ Check Duplex Mismatches

```bash
show interfaces
```

Compare duplex settings on both ends (Full/Half). If mismatched, manually configure:

```bash
interface g0/1
 speed 1000
 duplex full
```

#### 3️⃣ Verify Local Addressing

```bash
show ip interface brief
show arp
```

Ensure correct IP, subnet mask, and ARP entries.

#### 4️⃣ Verify Default Gateway

```bash
show ip route
ping <gateway>
```

If unreachable, reconfigure:

```bash
ip route 0.0.0.0 0.0.0.0 <next-hop>
```

#### 5️⃣ Verify Correct Path

Check routing table entries and prefix matches.

```bash
traceroute <destination>
show ip route <destination>
```

Look for unexpected routes or missing networks.

#### 6️⃣ Verify the Transport Layer

Use **Telnet** or **SSH** to test port-level connectivity.

```bash
telnet <IP> <port>
```

Check NAT or ACL restrictions if blocked.

#### 7️⃣ Verify ACLs

Ensure ACLs are applied correctly and not blocking traffic.

```bash
show access-lists
show running-config | include access-group
```

#### 8️⃣ Verify DNS

```bash
nslookup www.cisco.com
ip host router 192.168.1.1
```

Check that DNS servers are reachable and configured properly.

---

## 🧠 12.6 Summary

* Maintain accurate documentation and baselines.
* Apply structured 7-step troubleshooting method.
* Select the right troubleshooting approach for the issue type.
* Use both **software (Wireshark, Syslog)** and **hardware (multimeters, testers)** tools.
* Analyze layer-by-layer to pinpoint failure cause.
* Verify cables, duplex, IPs, routing, ACLs, and DNS for connectivity issues.

---

## 📝 Additional Notes for NetAcad Questions



* **Logical topology** diagrams display **IP addresses** and network flow.
* **End-system documentation** includes information such as the **operating system**, installed software, and hardware details for servers and PCs.
* A **network baseline** answers:

  * How the network performs during a normal day.
  * Which parts of the network are **least** or **most** utilized.
* A baseline is not a continuous process but rather repeated periodically (e.g., yearly or quarterly).
* Use `show cdp neighbors detail` for detailed info about **directly connected Cisco devices**.


* The **three main stages** of troubleshooting are:

  1. **Gather Symptoms**
  2. **Isolate the Problem**
  3. **Implement Corrective Action**
* During the **Test Hypothesis** step, always create a **rollback plan** to quickly undo changes if the solution fails.
* When communicating with users:

  * Be empathetic and considerate.
  * Listen carefully or read reports thoroughly.
  * Speak in terms the user understands.
* The command `show protocols` displays both **global and interface-specific Layer 3 protocol** information.
* When troubleshooting **routers and Layer 3 switches**, consider up to **Layer 4 (Transport)**.
* Use **Bottom-Up** for physical issues, **Top-Down** for software problems.


* **Knowledge Bases** (like Cisco Support) are online resources that provide vendor-specific troubleshooting guidance.
* **Protocol Analyzers** such as Wireshark inspect packet contents to identify errors at multiple layers.
* **Cable Analyzers** test and certify copper or fiber cables for compliance with network standards.
* **Syslog severity level 0** corresponds to the **highest severity (Emergencies)**.



* **Physical Layer:** Late collisions, short frames, and jabber indicate cabling or NIC issues.
* **Data Link Layer:** Spanning Tree Protocol (STP) loops are Layer 2 problems causing broadcast storms.
* **Network Layer:** Routing protocol loops (e.g., OSPF, EIGRP) occur when route updates create circular paths.
* **Transport Layer:** Extended ACLs operate here; incorrect configuration can block TCP/UDP traffic.
* **Application Layer:** DNS resolution issues are application-level problems affecting name-to-IP translation.
* To verify connectivity of **non-ICMP protocols**, use **Telnet** (e.g., `telnet <IP> 80`).

---

## ✅ **Final Word**

Troubleshooting is both a science and an art — combining systematic logic with experience and intuition. Stay calm, analyze layer by layer, and let the data guide you to the fix.
