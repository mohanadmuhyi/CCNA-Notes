# 📘 CCNA ENSA - Semester 3

## 🔒 Module 4: ACL Concepts



### 🎯 Objectives

The objective of this module is to explain how ACLs are used as part of a network security policy by describing how ACLs filter traffic, how wildcard masks are used, how to create ACLs, and how to compare standard and extended IPv4 ACLs.

---

## 🔐 4.1 Purpose of ACLs

**What is an ACL?**

* A series of IOS commands used to filter packets based on header information.
* ACLs are configured on **routers**.
* Routers have no ACLs configured by default.
* When applied to an interface, the router evaluates packets to decide if they should be forwarded.

**ACL Structure:**

* Sequential list of permit/deny statements → called **Access Control Entries (ACEs)**.
* Packet filtering compares packet info against each ACE in order.
* The order of ACL commands matters, since ACEs are checked sequentially from top to bottom.

**Uses of ACLs:**

* Limit network traffic (performance).
* Provide traffic flow control.
* Basic security for network access.
* Filter traffic based on type.
* Screen hosts for network service access.
* Prioritize traffic classes.

**Packet Filtering:**

* Controls access by analyzing incoming/outgoing packets.
* Works at Layer 3 (IP) or Layer 4 (Transport).
* Cisco ACL types:

  * **Standard ACLs** → filter by source IPv4 only.
  * **Extended ACLs** → filter by source IP, destination IP, protocol, and ports.

**ACL Operation:**

* Can be applied **inbound** (before routing) or **outbound** (after routing).
* ACLs don’t affect packets originated from the router itself.
* Inbound ACL saves resources (drops packets before routing lookup).
* Outbound ACL filters after routing.

**Standard IPv4 ACL process:**

1. Extract source IPv4.
2. Compare sequentially with ACEs.
3. On match → apply action (permit/deny), stop checking further.
4. If no match → implicit deny (all traffic blocked).

⚠️ By default, ACLs deny all traffic (implicit deny). Always include at least one `permit` statement, for example `permit ip any any` to allow all traffic.

---

## 🎭 4.2 Wildcard Masks in ACLs

**Overview:**

* Similar to subnet mask but reversed logic.
* **Wildcard 0** → match bit.
* **Wildcard 1** → ignore bit.

**Examples:**

* `0.0.0.0` → match exact host.
* `0.0.0.255` → match /24 network.
* `0.0.15.255` → match range (e.g., 192.168.16.0 to 192.168.31.0).

**Detailed Range Example:**

For `192.168.16.0 0.0.15.255`:

* Wildcard mask = `00000000.00000000.00001111.11111111` (binary).
* First three octets must match exactly → `192.168.16`.
* In the fourth octet:

  * `00010000` (16) to `00011111` (31) are valid because the first 4 bits (`0001`) must match, while the last 4 bits are ignored.
* This means it covers networks from `192.168.16.0` through `192.168.31.0`.

**Shortcut:**

* To calculate: `Wildcard = 255.255.255.255 - Subnet Mask`.
* To get a subnet mask back from a wildcard: `Subnet Mask = 255.255.255.255 - Wildcard`.

Examples:

* /24 → `0.0.0.255`
* /28 → `0.0.0.15`
* /23 (two networks) → `0.0.1.255`

**Keywords:**

* `host` → replaces `0.0.0.0`
* `any` → replaces `255.255.255.255`

---

## 🛠️ 4.3 Guidelines for ACL Creation

**ACL Limits per Interface:**

* Maximum of **4 ACLs per interface**:

  * 1 inbound IPv4 ACL.
  * 1 outbound IPv4 ACL.
  * 1 inbound IPv6 ACL.
  * 1 outbound IPv6 ACL.
* You don’t need to configure all four; only those required by policy.

**Inbound vs Outbound:**

* **Inbound ACL**: Filters traffic *before* it enters the router’s routing decision. Saves resources because unwanted traffic is dropped immediately.
* **Outbound ACL**: Filters traffic *after* routing decision, just before leaving the router.
* **Which to choose?**

  * Use **inbound** when you want to block/allow traffic as early as possible.
  * Use **outbound** when you need to filter after the router already decided the route.

**Best Practices:**

* Base ACLs on security policies.
* Write out ACL logic first (planning).
* Use a text editor to prepare ACLs.
* Document with `remark` command.
* Test ACLs in lab before production.

---

## 🌐 4.4 Types of IPv4 ACLs

**Standard ACLs:**

* Filter only by **source IP**.

**Extended ACLs:**

* Filter by source IP, destination IP, protocol, ports, and more.

**Numbered ACLs:**

* Standard: `1–99`, `1300–1999`
* Extended: `100–199`, `2000–2699`

**Named ACLs (Preferred):**

* More descriptive (e.g., `FTP-FILTER`).
* Example:

  ```
  R1(config)# ip access-list extended FTP-FILTER
  R1(config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 any eq ftp
  ```

**Placement Guidelines:**

* Place ACLs where they are most efficient.
* **Extended ACLs → nearest to source.**
* **Standard ACLs → nearest to destination.**

**Placement Factors:**

* Control over source/destination.
* Bandwidth savings (filter early if possible).
* Ease of configuration.

---

## 📍 Placement Examples

**Standard ACL Example:**

* Deny traffic from `192.168.10.0/24` to `192.168.30.0/24`.
* Best placement: Outbound on R3’s G0/0 interface (prevents blocking other destinations).

Topology:

```
[192.168.10.0] --- R1 --- R2 --- R3 --- [192.168.30.0]
                                   \
                                    --- [192.168.31.0]
```

* Applying on R3 outbound G0/0 blocks only traffic to 192.168.30.0, while still allowing .10 network to reach 192.168.31.0.

**Extended ACL Example:**

* Block Telnet/FTP from `192.168.11.0/24` to `192.168.30.0/24`.
* Best placement: Inbound on R1 G0/0/1 (only filters traffic from source network).

Topology:

```
[192.168.11.0] --- R1 --- R2 --- R3 --- [192.168.30.0]
```

* Applying at R1 inbound ensures Telnet/FTP traffic is denied immediately at the source.

---

## 📝 4.5 Summary

* ACLs filter packets using permit/deny rules (ACEs).
* No ACLs configured by default.
* Inbound ACLs filter before routing, outbound after routing.
* Wildcard masks determine matching logic (0 = match, 1 = ignore).
* Wildcard shortcut: `255.255.255.255 - subnet mask`.
* Can also convert wildcards back to subnet masks using the same formula.
* Detailed binary analysis shows how wildcard masks cover IP ranges.
* Keywords: `host`, `any`.
* Per-interface ACL limits: max 4 (inbound/outbound for IPv4 and IPv6).
* Standard ACL → source only. Extended ACL → source, destination, protocol, ports.
* Numbered vs Named ACLs (named preferred).
* Order of ACL commands matters.
* ACL placement: Extended near source, Standard near destination.

---

## 📚 Additional Notes for NetAcad Questions

* **ACEs (Access Control Entries)**: Permit or deny statements in an ACL are called ACEs.
* **Standard ACLs**: They filter only at Layer 3 (source IPv4).
* **Implicit deny**: There is no implicit permit; packets without a match are denied.
* **Wildcard masks**:

  * `0.0.0.0` permits a single host.
  * `0.0.255.255` matches a /16 network.
  * `255.255.255.255` permits all hosts.
  * `0.0.0.255` permits a /24 network.
* **ACL limits**: Up to 4 ACLs per interface (inbound/outbound IPv4 + IPv6 combined).
* **Best practice**: Write the ACL first before applying it to the router.
* **Extended ACLs**: Can filter based on TCP/UDP port numbers, not just IPs.
* **Named ACLs**: Can be standard or extended, and are preferred for readability.
* **Placement**:

  * Standard ACLs → near destination.
  * Extended ACLs → near source.

---

## ✅ Final Word

ACLs are essential for controlling traffic flow, enforcing security, and optimizing network efficiency. Correct ACL placement, awareness of default deny, and careful planning ensure both protection and performance.
