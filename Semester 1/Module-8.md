# 📘 CCNA - Module 8: Network Layer

## 🎯 Objective

Explore how the network layer enables end-to-end communication, logical addressing, packet forwarding, and routing across multiple networks.

---

## 8.1 🌐 Network Layer Protocols

### 🤩 Core Functions:

* **Addressing**: Adds source and destination IP addresses.
* **Encapsulation**: Wraps transport segment into a packet.
* **Routing**: Finds best path from source to destination.
* **Decapsulation**: Destination device removes IP header and passes segment to transport layer.

### 📦 Packet Structure (IPv4)

| Field           | Description                                                             |
| --------------- | ----------------------------------------------------------------------- |
| Version         | IP version (4)                                                          |
| Header Length   | Size of header (min 20 bytes)                                           |
| Total Length    | Entire packet size                                                      |
| TTL             | Limits packet lifespan (e.g., 128 for Windows, 64 for Linux/Unix/macOS) |
| Protocol        | Upper-layer protocol (e.g., TCP = 6, UDP = 17)                          |
| Source IP       | Sender's IP address                                                     |
| Destination IP  | Receiver's IP address                                                   |
| Fragment Offset | Used to reassemble fragmented packets                                   |
| Identification  | Uniquely identifies fragmented packet groups                            |
| Header Checksum | Detects header corruption                                               |

> 🔎 TTL (Time to Live):
>
> * **Windows**: 128
> * **Linux/macOS**: 64
> * **Routers**: 255

### 📏 Header Sizes:

* **IPv4 Header**: 20 bytes
* **IPv6 Header**: 40 bytes (fixed size, simpler, no checksum)

> 🚫 **IPv6 doesn’t use NAT** — it has a vast address space, removing the need for private-public translation.

---

## 8.2 🗚️ IPv4 vs IPv6

### ⚠️ IPv4 Limitations

* **Limited Address Space**: Only \~4.3 billion unique IPs.
* **Requires NAT**: To allow multiple devices in a private network to share a single public IP.
* **No built-in security**: IPsec was retrofitted.
* **Broadcast Traffic**: Can flood networks.
* **Complex Header**: Slower to process than IPv6.

### 🌐 Addressing Comparison

* **IPv4**: \~4.3 billion addresses (2^32)
* **IPv6**: \~340 undecillion addresses (2^128)

| Feature        | IPv4                        | IPv6                             |
| -------------- | --------------------------- | -------------------------------- |
| Address Length | 32-bit                      | 128-bit                          |
| Format         | Decimal (e.g., 192.168.1.1) | Hexadecimal (e.g., 2001\:db8::1) |
| Header         | 20 bytes, complex           | 40 bytes, simplified             |
| NAT Usage      | Common                      | Not used                         |
| Broadcast      | Yes                         | No (uses multicast)              |
| Configuration  | Manual or DHCP              | Stateless (SLAAC) or DHCPv6      |
| Fragmentation  | Supported                   | Handled by sender, not routers   |
| Number of IPs  | 4,294,967,296               | \~3.4 x 10^38                    |

> 🧠 IPv6 is designed for scalability, security, and performance in modern networks.

---

## 8.3 📌 IP Characteristics

### 🔹 Media Independent

* IP operates independently of the physical medium — copper, fiber, or wireless. It only relies on the delivery mechanisms provided by lower layers.

### 🔹 Connectionless

* IP does not establish a session before sending data.
* Each packet is treated independently, with no reference to any previous packet.

### 🔹 Best Effort Delivery

* IP does not guarantee packet delivery, order, or duplicate protection.
* Reliability must be handled by upper-layer protocols (e.g., TCP).

> 🚀 IP enables communication without needing to care how it physically gets there.

---

## 8.4 🤭 Host Forwarding Decision

When a device sends a packet:

1. Checks destination IP:

   * **Same network** → send directly.
   * **Different network** → send to default gateway.
2. Uses **ARP** to find MAC of the next hop.

> ↻ The MAC address changes at each hop, but the IP stays the same end-to-end.

---

## 8.5 🚣 Routers

Routers operate at the **network layer (Layer 3)**. Their tasks:

* Examine destination IP
* Consult **routing table**
* Forward packet out appropriate interface

Routers also:

* **De-encapsulate** frame → examine packet
* **Re-encapsulate** into new frame with next-hop MAC address

> 🌐 Broadcasts don’t pass through routers — they separate **broadcast domains**.

### 📘 Routing Table and Codes (Letters)

Routers use a **routing table** to decide where to forward packets. Each entry includes:

* **Destination Network**
* **Next Hop IP Address**
* **Exit Interface**
* **Route Source (code/letter)**

| Code | Meaning                            |
| ---- | ---------------------------------- |
| C    | Connected (directly)               |
| S    | Static route                       |
| S\*  | Static default route (last resort) |
| D    | EIGRP                              |
| O    | OSPF                               |
| R    | RIP                                |
| B    | BGP                                |
| L    | Local (assigned to interface)      |

> 🧠 Routing table codes help admins quickly identify how a route was learned.

---

## 8.6 🧠 Routing

### 🔹 Static Routing

* Manually configured
* Simple, predictable
* No overhead
* Not scalable for large networks

### 🔹 Dynamic Routing

* Uses protocols (e.g., OSPF, EIGRP, BGP)
* Automatically adapts to changes
* Requires CPU/memory

| Feature        | Static | Dynamic     |
| -------------- | ------ | ----------- |
| Configuration  | Manual | Automatic   |
| Scalability    | Low    | High        |
| Resource Usage | Low    | Medium/High |
| Adaptability   | None   | High        |

---

## 8.7 🗾 DHCP and Address Assignment

### 📦 Dynamic Host Configuration Protocol (DHCP)

* Automatically assigns IP, subnet mask, gateway, and DNS
* Can run on routers or dedicated servers

### ↻ DORA Process:

1. **Discover** → Client broadcasts looking for DHCP server
2. **Offer** → DHCP server replies with available IP
3. **Request** → Client requests offered IP
4. **Acknowledge** → Server confirms assignment

> 🔧 DHCP simplifies management and reduces IP conflicts.

---

## 📚 Additional Notes for NetAcad Questions

* The **transport layer** is responsible for sending segments to be encapsulated in IPv4/IPv6 packets.
* **Fragmentation** occurs when a packet is forwarded onto a medium with smaller MTU.
* IPv6 **Hop Limit** replaces IPv4’s TTL.
* `route print` or `netstat -r` shows the host routing table.
* `show ip route` shows the routing table on Cisco IOS.
* **Best effort** delivery means no guarantee of delivery or error-checking.

---

## 📊 Final Word

Understanding the network layer is essential to mastering IP addressing, routing, and logical communication across networks. From IP header structure to how routers handle forwarding decisions, this module builds a solid foundation for all networking and security skills.

> 🎯 Key Takeaway: IP doesn’t care how the message gets there — only that it does. That’s why routing and addressing are at the core of every networked interaction.

