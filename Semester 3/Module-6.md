# CCNA ENSA – Semester 3

## 📌 Module 6: NAT for IPv4

### 🎯 Objectives

Configure NAT services on edge routers to provide IPv4 address scalability.

---

## 🔹 6.1 NAT Characteristics

### 🌍 IPv4 Address Space

* Private IPv4 ranges (RFC 1918):

  * Class A: `10.0.0.0 – 10.255.255.255` (/8)
  * Class B: `172.16.0.0 – 172.31.255.255` (/12)
  * Class C: `192.168.0.0 – 192.168.255.255` (/16)
* Private addresses **cannot be routed on the internet**.
* NAT translates **private (inside local)** to **public (inside global)** so local users can connect to the outside network.
* Private IPs can be replicated across different networks but **not duplicated within the same local network**.

### 🎯 Purpose of NAT

* Conserves public IPv4 addresses.
* Allows internal devices to use private addressing while still accessing external resources.
* Implemented on border routers.

### 🔄 NAT Process Example

1. PC (192.168.10.10) sends packet to web server (209.165.201.1).
2. Router checks NAT table and maps local to global address.
3. Packet sent with translated source (209.165.200.226).
4. Server replies to inside global address.
5. Router translates back to inside local and forwards to PC.

### 🏷️ NAT Terminology

* **Inside local**: Private IP inside the network (e.g., 192.168.10.10).
* **Inside global**: Public IP representing inside device to outside (e.g., 209.165.200.226).
* **Outside global**: Public IP of external device (e.g., 209.165.201.1).
* **Outside local**: Address of external device as seen from inside.

---

## 🔹 6.2 Types of NAT

### 🔒 Static NAT

* One-to-one mapping of private ↔ public.
* Useful for servers requiring consistent public accessibility.
* Also useful for security: devices that **do not need external access will not be assigned a public IP**.
* Requires enough public IPs for all mapped hosts.

**Configuration Rule:**

```bash
ip nat inside source static <inside-local> <inside-global>
```

**Configuration Example:**

```bash
R1(config)# ip nat inside source static 192.168.10.254 209.165.201.5
R1(config)# interface s0/1/0
R1(config-if)# ip address 192.168.1.2 255.255.255.252
R1(config-if)# ip nat inside
R1(config)# interface s0/1/1
R1(config-if)# ip address 209.165.200.1 255.255.255.252
R1(config-if)# ip nat outside
```

---

### 🔄 Dynamic NAT

* Uses a pool of public addresses.
* First-come, first-served assignment.
* Typically, there are **fewer public IPs than private IPs** in the network.
* Requires sufficient public IPs for simultaneous users.

**Configuration Rule:**

```bash
ip nat pool <name> <start-ip> <end-ip> netmask <mask>
access-list <number> permit <inside-network>
ip nat inside source list <number> pool <name>
```

**Configuration Example:**

```bash
R1(config)# ip nat pool NAT-POOL1 209.165.200.226 209.165.200.240 netmask 255.255.255.224
R1(config)# access-list 1 permit 192.168.0.0 0.0.255.255
R1(config)# ip nat inside source list 1 pool NAT-POOL1
R1(config)# interface s0/1/0
R1(config-if)# ip nat inside
R1(config)# interface s0/1/1
R1(config-if)# ip nat outside
```

---

### 🔀 Port Address Translation (PAT / NAT Overload)

* Maps multiple private IPs to **one public IP** (or small pool) using **port numbers**.
* If port already in use, router assigns next available.
* PAT can also be **merged with dynamic NAT**, allowing each public IP in the pool to be reused by multiple internal devices through different port numbers.

**Configuration Rule (Interface):**

```bash
ip nat inside source list <number> interface <interface> overload
```

**Configuration Example (Interface):**

```bash
R1(config)# access-list 1 permit 192.168.0.0 0.0.255.255
R1(config)# ip nat inside source list 1 interface s0/1/1 overload
R1(config)# interface s0/1/0
R1(config-if)# ip nat inside
R1(config)# interface s0/1/1
R1(config-if)# ip nat outside
```

**Configuration Rule (Pool):**

```bash
ip nat inside source list <number> pool <name> overload
```

**Configuration Example (Pool):**

```bash
R1(config)# ip nat pool NAT-POOL2 209.165.200.226 209.165.200.240 netmask 255.255.255.224
R1(config)# access-list 1 permit 192.168.0.0 0.0.255.255
R1(config)# ip nat inside source list 1 pool NAT-POOL2 overload
```

---

### ⚖️ NAT and PAT Comparison

* **NAT**: Translates only IP addresses (one-to-one).
* **PAT**: Translates both IP addresses and port numbers, allowing many-to-one.
* NAT requires a unique public IP per inside host, PAT allows multiple inside hosts to share one public IP using different port numbers.

**Visualization:**

**NAT (One-to-One Mapping)**

```
Inside Local (192.168.10.10) ---> Inside Global (209.165.200.226)
Socket: 192.168.10.10 : *      ---> 209.165.200.226 : *
```

Each private IP is mapped to a unique public IP, sockets are not modified.

**PAT (Many-to-One Using Ports)**

```
Inside Local (192.168.10.10:1025) ---> Inside Global (209.165.200.226:3001)
Inside Local (192.168.10.11:1030) ---> Inside Global (209.165.200.226:3002)
Inside Local (192.168.10.12:1040) ---> Inside Global (209.165.200.226:3003)
```

Multiple private hosts share the same public IP, differentiated by port numbers.

### 📦 Packets Without a Layer 4 Segment

* Some packets (like ICMPv4 echo requests/replies) do not contain TCP/UDP port numbers.
* PAT handles these using other identifiers such as the ICMP Query ID.
* Other protocols without Layer 4 headers are handled differently by NAT and are beyond the scope of this module.

---

## 🔹 6.3 NAT Advantages and Disadvantages

### ✅ Advantages

* Conserves public IPv4 addresses.
* Provides address consistency.
* Allows internal addressing to remain unchanged.
* Hides internal addresses (basic security).

### ⚠️ Disadvantages

* Increases forwarding delays.
* Breaks end-to-end addressing and traceability.
* Complicates tunneling (e.g., IPsec).
* Some services requiring inbound initiation may fail.

---

## 🔹 6.4 Verification Commands

### 🔎 For Static NAT

```bash
R1# show ip nat translations     # Displays the current NAT table entries (active translations)
R1# show ip nat statistics       # Shows total translations, hits/misses, and inside/outside interfaces
R1# clear ip nat statistics      # Clears previous NAT statistics counters
```

### 🔎 For Dynamic NAT

```bash
R1# show ip nat translations             # Shows static and dynamic translations in the NAT table
R1# show ip nat translation verbose      # Adds details: timers, creation time, use count, etc.
R1# show ip nat statistics               # Shows number of translations, pool usage, hits/misses
R1# clear ip nat translation *           # Clears all dynamic NAT entries from the translation table
R1# show running-config | include NAT    # Displays NAT, ACL, and pool-related commands in the config
```

### 🔎 For PAT

```bash
R1# show ip nat translations     # Shows inside global, inside local, and port numbers for PAT sessions
R1# show ip nat statistics       # Displays PAT usage: how many addresses are allocated and shared
```

---

## 🔹 6.7 NAT64

* IPv6 was designed to remove NAT needs.
* ULAs (Unique Local Addresses) are local-only, similar to IPv4 private ranges.
* **NAT64**: Allows communication between IPv6-only and IPv4-only networks.
* Temporary migration tool, not long-term.

---

## 🏁 Final Word

NAT is essential for IPv4 scalability. It allows private devices to communicate with the internet using fewer public IPs, while PAT maximizes efficiency by using port numbers. Static NAT is predictable, dynamic NAT is flexible, and PAT is the most widely used. Despite drawbacks like breaking end-to-end connectivity, NAT remains a cornerstone of IPv4 networking, while NAT64 supports transition to IPv6.

---
