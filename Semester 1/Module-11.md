# 🧮 Module 11: IPv4 Addressing

## 🎯 Objectives

Understand how IPv4 addressing works, including the role of subnetting, network/broadcast addresses, and VLSM. Apply subnetting techniques using both fixed and variable-length methods.

---

## 📘 11.1 IPv4 Network, Host & Broadcast Addresses

### 🔍 Key Address Types

* **Network Address**: Identifies the network segment. All host bits are 0.
* **Broadcast Address**: Used to communicate with all devices in a network. All host bits are 1.
* **Host Addresses**: All addresses between the network and broadcast addresses.

### 🎯 How to Get:

1. **Network Address**: AND the IP with the subnet mask.
2. **Broadcast Address**: Set all host bits to 1 (based on subnet mask).
3. **First Usable Host**: Network address +1
4. **Last Usable Host**: Broadcast address -1

---

## 📘 11.2 Subnetting Overview

### 🔧 What is a Subnet?

* A **subnet** is a smaller network created from a larger network by borrowing host bits.
* IPv4 address: **32 bits** (divided into 4 octets)
* Subnet mask: Divides the IP into **network** and **host** portions

### 🌐 Representations:

* **CIDR Notation**: e.g., /24 = 255.255.255.0
* **Dotted Decimal**: 255.255.255.0
* **Binary**: 11111111.11111111.11111111.00000000

### 🧠 IP Address Classes

| Class | Range (First Octet) | Default Subnet Mask | Usage                 |
| ----- | ------------------- | ------------------- | --------------------- |
| **A** | 1–126               | 255.0.0.0 (/8)      | Very large networks   |
| **B** | 128–191             | 255.255.0.0 (/16)   | Medium-sized networks |
| **C** | 192–223             | 255.255.255.0 (/24) | Small networks        |
| **D** | 224–239             | N/A                 | Multicast only        |
| **E** | 240–255             | N/A                 | Experimental use      |

> Note: 127.0.0.0/8 is reserved for loopback testing.

### 🏠 Private IP Address Ranges

| Class | Private Range                 |
| ----- | ----------------------------- |
| A     | 10.0.0.0 – 10.255.255.255     |
| B     | 172.16.0.0 – 172.31.255.255   |
| C     | 192.168.0.0 – 192.168.255.255 |

These addresses **cannot be routed** on the public Internet. They're used **inside local networks** and require **NAT (Network Address Translation)** for external access.

### 🌍 Public vs. Private IPs

| Private IPs                  | Public IPs               |
| ---------------------------- | ------------------------ |
| Used within local networks   | Routable on the internet |
| Require NAT for internet use | Assigned by ISPs         |
| Cost-free and reusable       | Costly and limited       |

### 🔗 Why Subnet?

* Reduce broadcast domains
* Improve performance and security
* Allow more efficient IP utilization
* One reason is to increase the number of broadcast domains

### 📏 Rules

* **Number of Hosts**: 2^n - 2 (n = number of host bits)
* **Number of Subnets**: 2^s (s = number of borrowed bits)
* **Maximum Subnet**: /30 (only 2 usable IPs)

### 🎓 Classful vs Classless

* **Classful**: Default boundaries (A: /8, B: /16, C: /24)
* **Classless (CIDR)**: Allows breaking default octet boundaries (e.g., breaking into the 4th octet)

---

## 📘 11.3 Subnetting Using the Magic Number

### 🔮 Magic Number Method

1. **Decide required subnets**
2. **Borrow bits** to meet requirement
3. **New Subnet Mask**: Add borrowed bits to default mask
4. **Magic Number**: Weight of first 1 in new subnet octet (256 - subnet increment)
5. **List Subnets** by adding the magic number

### 🧪 Example: Subnet 192.168.1.0/24 into 4 Subnets

1. Need 4 subnets → 2 bits (2^2 = 4)
2. New subnet mask: /26 (24 + 2)
3. Magic number: 64
4. Subnets:

   * 192.168.1.0/26 → Hosts: 1-62, Broadcast: .63
   * 192.168.1.64/26 → Hosts: 65-126, Broadcast: .127
   * 192.168.1.128/26 → Hosts: 129-190, Broadcast: .191
   * 192.168.1.192/26 → Hosts: 193-254, Broadcast: .255

### 🔄 Larger Prefix Examples

#### Subnetting 10.0.0.0/8 to /20:

* Borrow 12 bits → /20
* Magic number in 3rd octet: 16
* Subnets:

  * 10.0.0.0/20
  * 10.0.16.0/20
  * 10.0.32.0/20
  * ...
  * 10.1.0.0/20
  * 10.1.16.0/20
  * 10.1.32.0/20
  * ...
  * 10.255.160.0/20
  * 10.255.192.0/20
  * 10.255.224.0/20


#### Subnetting 172.16.0.0/16 into /20:

* Magic number in 3rd octet = 16
* Subnets:

  * 172.16.0.0/20
  * 172.16.16.0/20
  * 172.16.32.0/20 ...

### 🤔 What if I want 5 subnets?

* You need 3 bits (2^3 = 8)
* Always round up to next power of 2 to ensure enough subnets

---

## 📘 11.4 Network Segmentation

Dividing large IP blocks into smaller segments helps:

* Improve performance
* Control traffic
* Enhance security
* Increase the number of broadcast domains

---

## 📘 11.5 VLSM (Variable Length Subnet Mask)

### 🧩 Why VLSM?

* Avoids IP waste by assigning subnet masks based on host requirements

### 🔢 Steps for VLSM

1. List subnet needs by **descending host count**
2. Calculate required bits → subnet mask
3. Assign from largest to smallest within main network ID
4. For each, calculate:

   * Network Address
   * First/Last Usable Host
   * Broadcast Address

### 🔍 Example:

Given: 192.168.10.0/24

| Subnet | Hosts Needed | Mask | Subnet ID      | Host Range  | Broadcast |
| ------ | ------------ | ---- | -------------- | ----------- | --------- |
| A      | 100          | /25  | 192.168.10.0   | .1 – .126   | .127      |
| B      | 50           | /26  | 192.168.10.128 | .129 – .190 | .191      |
| C      | 20           | /27  | 192.168.10.192 | .193 – .222 | .223      |
| D      | 10           | /28  | 192.168.10.224 | .225 – .238 | .239      |
| E      | 2            | /30  | 192.168.10.240 | .241 – .242 | .243      |

---

## 📘 11.6 APIPA, Unicast, Broadcast, and Multicast

* **APIPA (169.254.0.0/16)**: Can communicate within same LAN
* **Unicast**: One-to-one communication (single sender → single receiver)
* **Broadcast**: One-to-all on the local network (e.g., 255.255.255.255)
* **Multicast (224.0.0.0 – 239.255.255.255)**: One-to-many targeted group communication

---




## 📘 11.7 Structured Design

### 🧠 IPv4 Network Address Planning

* Determine total subnets and hosts required
* Identify where to use public vs. private IPs
* Address conservation is more crucial in DMZs
* Use private IPs in intranet (e.g., 10.0.0.0/8)
* Include static vs. dynamic assignment
* Helps avoid duplication and simplifies management

### 📌 Device Address Assignment

* **Clients**: Use DHCP, leases are temporary
* **Servers & Peripherals**: Static IPs for consistency
* **Public-Facing Servers**: Use public IPs or NAT + VPN
* **Intermediary Devices**: Static IPs for management
* **Gateways**: Typically use first or last usable address in subnet
* Have a consistent strategy for ease of configuration and documentation

---

## 🔗 Calculators and Practice Tools

* [IP Subnet Calculator](https://www.calculator.net/ip-subnet-calculator.html)
* [VLSM Calculator](https://vlsmcalc.vercel.app/)
* [Subnetting Practice Tool](https://www.subnetting.net/Subnetting.aspx?mode=practice)

## 🧠 Additional Notes for NetAcad Questions

### 🔹 Private & Public IPs

* ✅ Private IPs are used inside local networks (e.g., 10.0.0.0/8, 192.168.0.0/16).
* ✅ Any organization can use them internally.
* 🚫 Public routers do not forward private IP packets.
* ✅ Public IPs are required for internet communication and are assigned by ISPs.
* ✅ Exhaustion of public IPv4 is a driver for IPv6 adoption.

### 🔹 Address Allocation Authorities

* ✅ RIRs (Regional Internet Registries) get IPs from IANA and assign to ISPs/orgs.
* ✅ ICANN oversees global IP address management and delegates to RIRs.
* ✅ AFRINIC handles IP allocation for Africa region.

### 🔹 Broadcast Behavior

* ✅ Routers do **not forward** IPv4 broadcast packets by default.
* Excessive broadcasts can lead to:

  * ✅ Slow network performance
  * ✅ Slow device performance

---

## 🏁 Final Words

Mastering IPv4 subnetting builds a strong foundation for advanced networking. Understand the theory, apply the magic number, practice VLSM, and use tools to boost confidence.
