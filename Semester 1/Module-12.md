# 📘 CCNA Module 12: IPv6 Addressing

## 🧠 Objective
Implement an IPv6 addressing scheme.

---

### 🔶 12.1 IPv4 Issues

#### ❗ Why IPv6?

* IPv4’s **32-bit address space** is exhausted (\~4.3 billion addresses).
* IPv6 uses a **128-bit** address space — far more capacity.
* IPv6 fixes many IPv4 limitations:

  * No need for NAT
  * Better suited for IoT
  * Simplified headers and security

#### ♻️ IPv4 and IPv6 Coexistence

The transition to IPv6 is gradual, and both protocols will coexist for years. The IETF defined three migration techniques:

**Dual Stack**
Devices run **both IPv4 and IPv6** at the same time.
**Command:**

```
R1(config)# ipv6 unicast-routing
R1(config)# interface g0/0
R1(config-if)# ipv6 address 2001:db8::1/64
R1(config-if)# ip address 192.168.1.1 255.255.255.0
```

**Tunneling**
Encapsulates IPv6 packets **inside IPv4 packets** for transport. Examples: **6to4**, **ISATAP**

**Translation (NAT64)**
Allows **IPv6-only devices** to talk to **IPv4-only devices** using protocol translation similar to NAT

> ⚠️ Tunneling and translation are temporary tools. The long-term goal is **native IPv6** communication from end to end.

---

### 🔶 12.2 IPv6 Address Representation

#### 📍 Basic Format

* IPv6 addresses are **128 bits**, written in **hexadecimal**, divided into **8 hextets**:

```
x:x:x:x:x:x:x:x
```

* Case-insensitive and written in either lowercase or uppercase
* Example:

```
2001:0db8:0000:1111:0000:0000:0000:0200
```

#### 🔻 Rule 1 – Omit Leading Zeros

* Example: `01ab` → `1ab`

#### 🧱 Rule 2 – Double Colon `::`

* Use `::` to replace one **contiguous group** of zero hextets
* Can only appear **once** in an address
* Example:

```
2001:db8:0:1111::200
```

---

### 🔶 12.3 IPv6 Address Types

#### 📍 3 Categories

1. **Unicast** – One-to-one
2. **Multicast** – One-to-many (FF00::/8)
3. **Anycast** – One-to-nearest (delivered to the closest)

> ❌ IPv6 has **no broadcast**. Use **multicast** instead.

#### 📏 Prefix Length

* Uses **slash notation** (e.g., `/64`) to indicate the **network portion**
* Ranges from `/0` to `/128`
* Most networks use `/64` because it's required for **SLAAC**

#### 🧍‍♂️ Types of IPv6 Unicast Addresses

| Type                     | Purpose                                     | Routability               | Where it Operates                             |
| ------------------------ | ------------------------------------------- | ------------------------- | --------------------------------------------- |
| **GUA** (Global Unicast) | Public IPv6 address                         | **Globally routable**     | Across the **global internet**                |
| **LLA** (Link-local)     | Auto/manual address for local communication | **Not routable**          | Only within the **local link**                |
| **ULA** (Unique Local)   | Internal use (like IPv4 private)            | **Not globally routable** | Inside an **organization or limited network** |

> 🌍 GUA = internet-wide
> 🔗 LLA = same-link communication
> 🏠 ULA = internal-only communication

---

### 🔶 12.4 Link-Local Address (LLA)

* Operates only on the **local link** (non-routable).
* Every IPv6-enabled interface **must have** an LLA.
* Used for:

  * Router-to-router communication
  * Routing protocols (OSPFv3, EIGRPv6)
  * Default gateway

**Prefix:** `fe80::/10`

* Auto-configured using EUI-64 or randomly generated.

---

### 🔶 12.5 Unique Local Address (ULA)

* Used internally in an organization. Not routable on internet.

**Prefix Range:** `fc00::/7` (only `fd00::/8` used)

**Format:** `fdxx:xxxx:xxxx::/48`

**Use Cases:**

* Internal communication between company sites
* Devices that don't need internet access

**Compare LLA vs. ULA:**

| Feature  | LLA             | ULA                      |
| -------- | --------------- | ------------------------ |
| Prefix   | `fe80::/10`     | `fd00::/8`               |
| Scope    | Local link only | Inside site/organization |
| Routable | ❌               | ❌ (internally routable)  |
| Required | ✅               | ❌                        |

---

### 🔶 12.6 Global Unicast Address (GUA)

* Publicly routable IPv6 address

**Prefix:** `2000::/3`

* Assigned by ISP

**Structure:**

```
| Global Routing Prefix | Subnet ID | Interface ID |
```

**Example:**

```
2001:0db8:acad:0001::/64
```

**Configured on router interface:**

```
R1(config)# interface g0/0
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
```

---

### 🔶 12.7 Interface ID and Address Assignment

**2 Ways to Generate Interface ID:**

* **EUI-64** (uses MAC address)
* **Randomly generated** (privacy)

**Router Advertisement (RA):**

* Sent by router
* Host responds with:

  * **RS (Router Solicitation)** → Router replies with RA

**Address Assignment Methods:**

| Method                   | What it Provides               |
| ------------------------ | ------------------------------ |
| SLAAC                    | Prefix + Default Gateway       |
| SLAAC + Stateless DHCPv6 | Prefix + Default Gateway + DNS |
| Stateful DHCPv6          | Full addressing config         |

**Duplicate Address Detection (DAD):**

* Sends **NS (Neighbor Solicitation)** to verify uniqueness
* If another device responds with **NA (Neighbor Advertisement)**, address is in use

---

### 🔶 12.8 Subnetting IPv6

* IPv6 is designed to simplify subnetting
* Most networks use a **/64** prefix
* Default subnetting occurs between the **/48** and the **/64**

**Example:**

```
Global Routing Prefix: 2001:db8:acad::/48
Subnet 1: 2001:db8:acad:1::/64
Subnet 2: 2001:db8:acad:2::/64
```

**Subnet ID:** 16 bits → `2^16 = 65,536` subnets
**Host addresses per /64:** `2^64` = **18 quintillion hosts**

**Router Example:**

```
R1(config)# interface g0/0
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
```
---

## 🧩 Additional Notes for NetAcad Questions

### 📌 IPv6 Motivation & Address Depletion

* The **main driver for IPv6 adoption** is the **depletion of IPv4 addresses**, not performance, simplicity, or security.
* As of now, **4 out of 5 Regional Internet Registries (RIRs)** have **exhausted their IPv4 allocations**.

### 📌 Native IPv6 Connectivity

* Only **dual stack** provides **native IPv6 connectivity**.
* **Tunneling** and **translation** are transition methods — not native.

### 📌 Subnetting Details

* The **Subnet ID** sits between Global Routing Prefix and Interface ID — does **not borrow** bits.
* Example:

  ```
  Address: 2001:db8:cafe:1111:2222:3333:4444:5555
  → Subnet portion: **1111**
  ```

### 📌 Subnet ID Bit Allocation

* Given a `/32` Global Routing Prefix and `/64` prefix:
  → Subnet ID = **32 bits** (from bit 33 to 64)

---

## ✅ Final Word

IPv6 isn’t just about more addresses — it’s a complete rethinking of how modern networks operate. From simplified headers and efficient routing to eliminating NAT and enabling end-to-end connectivity, IPv6 is critical for the future of the internet.

As you study, focus on **address structure**, **dynamic assignment methods**, and **command-line configuration**. These are core skills every network professional must master in the IPv6 world.

> "The best way to predict the future is to build it." – IPv6 is the future. Let's build it right.



