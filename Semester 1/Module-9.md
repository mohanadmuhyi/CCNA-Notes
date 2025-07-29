# 📘 CCNA - Module 9: Address Resolution

## 🎯 Objective

Explain how ARP and ND (Neighbor Discovery) enable communication on a network, including resolving MAC addresses from IP addresses in IPv4 and IPv6 networks.

---

## 9.1 🗭 MAC and IP Addresses

### 🧩 Layered Addressing

* **MAC Address (Layer 2)**: Physical address, unique per NIC, used to deliver frames on the same network.
* **IP Address (Layer 3)**: Logical address used to identify devices across networks.

### 🔄 Communication Behavior

| Situation      | MAC Destination          | IP Destination          |
| -------------- | ------------------------ | ----------------------- |
| Same network   | Destination device's MAC | Destination device's IP |
| Remote network | Default gateway's MAC    | Destination device's IP |

> 🤠 IP addresses guide the packet. MAC addresses guide the frame.

### 🔀 Media Independent Delivery

* IP is **media independent**, so it doesn't rely on the type of medium (wired, wireless).
* The network layer is responsible for addressing and routing, regardless of physical medium.

---

## 9.2 🧠 Address Resolution Protocol (ARP)

### 📖 What is ARP?

* Used in **IPv4** networks.
* Maps IP addresses to MAC addresses.
* Maintains an **ARP Table (Cache)** with IP-MAC pairs.

### ⚙️ How ARP Works

1. **Device checks ARP table** for destination IP → MAC.
2. If entry exists → use MAC.
3. If no entry → broadcast **ARP Request**:

   * "Who has IP 192.168.1.1? Tell me."
4. Target responds with **ARP Reply**:

   * "192.168.1.1 is at AA\:BB\:CC\:DD\:EE\:FF."
5. Device stores result in ARP table.

> 🧪 **ARP Table Expiry**: Entries are temporary, removed after timeout (typically \~2-4 minutes depending on OS).

### 🌐 ARP in Remote Communication

* For remote destinations, the MAC address of the **default gateway** is used.
* ARP resolves the **gateway's MAC**.

### 🛠️ ARP Operation in Detail

* **ARP Request**: Sent as broadcast (FF\:FF\:FF\:FF\:FF\:FF) asking for the MAC of a specific IP.
* **ARP Reply**: Sent as unicast from the device owning the IP.
* **Each NIC has a unique MAC address**, which is used to respond.

### 📦 ARP Frame Details

#### 🔍 ARP Request Frame:

* **Destination MAC**: FF\:FF\:FF\:FF\:FF\:FF (broadcast)
* **Source MAC**: MAC of sender
* **Target MAC**: 00:00:00:00:00:00 (unknown)
* **Sender IP**: IP of sender
* **Target IP**: IP address being resolved

#### 📨 ARP Reply Frame:

* **Destination MAC**: MAC of original requester
* **Source MAC**: MAC of responding host
* **Target MAC**: MAC of requester
* **Sender IP**: IP of responder
* **Target IP**: IP of requester

### ⚠️ ARP Vulnerabilities

* **Broadcast Overhead**: Every ARP request is broadcasted.
* **ARP Spoofing / MITM Attack**: Attackers can send fake ARP replies to intercept or reroute traffic.

  * Example: Attacker says, "I’m the gateway" and hijacks traffic.
  * 🌟 **Man-in-the-middle (MITM)**: Attacker places themselves between two communicating parties.

> 🛡️ **Mitigation**: Use Dynamic ARP Inspection (DAI) or switch security features in enterprise environments.

### 📅 Dynamic ARP Inspection (DAI)

* **Prevents ARP spoofing** by verifying ARP packets against known IP-MAC mappings.
* Enabled on trusted ports with DHCP snooping.
* Drops invalid or spoofed ARP packets.
* Ensures integrity of Layer 2 communications.

### 📅 CLI Tools

| Platform     | Command       |
| ------------ | ------------- |
| Cisco Router | `show ip arp` |
| Windows      | `arp -a`      |

---

## 9.3 🌐 Neighbor Discovery (IPv6)

### 🔀 What is ND?

* Replacement for ARP in **IPv6** networks.
* Uses **ICMPv6** messages for:

  * Address resolution
  * Router discovery
  * Redirection

### 📅 ND Message Types

| Message                         | Purpose                              |
| ------------------------------- | ------------------------------------ |
| **NS** (Neighbor Solicitation)  | Ask: "Who has this IPv6 address?"    |
| **NA** (Neighbor Advertisement) | Answer: "I have that address."       |
| **RS** (Router Solicitation)    | Ask: "Are there routers?"            |
| **RA** (Router Advertisement)   | Answer: "Yes, I’m a router."         |
| **Redirect**                    | Router says: "Use this better path." |

### 🤮 IPv6 Address Resolution

* Uses **multicast** instead of broadcast.
* Multicast scope is limited and more efficient.
* Each ND message includes source and destination IPv6 + MAC address in Ethernet.

### ⚙️ Advantage of ND over ARP

* ND uses **ICMPv6**, which includes built-in security options.
* It’s more efficient due to **multicast** rather than broadcast.
* Supports **Router Discovery**, **Prefix Discovery**, and more automation.

> 🚀 ND enables dynamic and efficient IPv6 communication.
> 📌 In **IPv6**, **fragmentation happens at the source device**, not in routers.

---

## 🔍 Fragmentation (in-depth)

### ✂️ What is Fragmentation?

* When a packet is too large for the next network's **MTU (Maximum Transmission Unit)**, it must be **fragmented** into smaller pieces.

### IPv4 Fragmentation

* Performed by **routers** along the path.
* Uses **Identification**, **Flags**, and **Fragment Offset** fields in IPv4 header.

### IPv6 Fragmentation

* Routers do **not** fragment packets.
* Fragmentation is done **only by the sending device**.
* Path MTU discovery is used to avoid fragmentation.

### ⚠️ Fragmentation Problems

* Increases overhead.
* Causes delay.
* Fragments may get lost → entire packet is dropped.
* More difficult for firewalls to inspect fragmented packets.

---

## 🚀 Traceroute (tracert)

* Uses TTL (IPv4) or **Hop Limit** (IPv6).
* Sends packets with increasing TTL values to map the path to the destination.
* Each router that decrements TTL to 0 replies with ICMP "Time Exceeded".

> 🤠 Used to identify each hop on the route between source and destination.

---

## 🧠 IP Longest Match (Routing)

* Routers use **Longest Prefix Match** in routing tables.
* If multiple routes match, the **most specific route (longest subnet mask)** is chosen.

> Example:
>
> * Match 1: 192.168.0.0/16
> * Match 2: 192.168.1.0/24 → This is chosen due to longer match

---

## 🧾 Additional Notes for NetAcad Questions

* If the destination is on the **same network**, the MAC address of the **destination device** is used.
* If on a **remote network**, the MAC address of the **default gateway/router** is used.
* **ARP** is used in IPv4 to resolve MAC from IP, while **ND** (Neighbor Discovery) is used in IPv6.
* **ARP table is stored in RAM** and **entries are temporary**.
* Commands:

  * Cisco: `show ip arp`
  * Windows: `arp -a`
* ARP is vulnerable to **spoofing (ARP poisoning)**.
* **ICMPv6 ND** uses messages:

  * **RA, RS** for SLAAC
  * **NS, NA** for MAC resolution
* **ICMPv6 uses multicast**, not broadcast.

---

## 📊 Final Word

Address resolution is at the heart of IP communication. ARP and ND handle the task of finding MAC addresses, fragmentation ensures data fits within MTUs, and tools like traceroute help visualize the route. Understanding these mechanisms builds a strong foundation in networking and is crucial for both security and troubleshooting.

> 🎯 Key Takeaway: If IP addresses are "names," then MAC addresses are "faces." The network must match the two before any communication can happen.

---
