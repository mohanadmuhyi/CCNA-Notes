# CCNA SRWE - Semester 2

## 🌐 Module 14: Routing Concepts

### 🎯 Objectives

Routers use information in packets to make forwarding decisions by determining the best path and then forwarding packets toward their destination.

---

## 🛣️ 14.1 Path Determination

### 📌 Router Functions

* Determine the best path using the **routing table**.
* Forward packets toward the destination.

### 🔍 Best Path = Longest Match

* The longest prefix match means the route with the most matching far-left bits with the destination IP address.
* This ensures packets are forwarded with the **most specific route available** instead of using a broader/general route.
* For example, if a router knows both 172.16.0.0/12 and 172.16.0.0/26, the /26 is more specific and therefore chosen.
* Used because it prevents misrouting packets and ensures the router sends traffic through the **closest possible match**.
* IPv4 Example: 172.16.0.10 matches /12, /18, and /26 → **/26 wins**.
* IPv6 Example: 2001\:db8\:c000::99 matches /40 and /48 → **/48 wins**.

### 🏗️ Building the Routing Table

* **Directly Connected Networks**: Added when interface has an IP + is up.
* **Remote Networks**: Learned via static routes or dynamic routing protocols.
* **Default Route (/0)**: Used when no more specific match exists. Also called **Gateway of Last Resort**.

---

## 📦 14.2 Packet Forwarding

### 🔄 Forwarding Decision Process

1. Frame arrives on ingress interface.
2. Router checks destination IP against routing table.
3. Finds longest prefix match.
4. Encapsulates packet in new data link frame → forwards to egress interface.
5. If no match → packet is **dropped**.

### 🚀 Forwarding Options

* **Directly Connected Device** → Resolve MAC via ARP (IPv4) or NDP (IPv6).
* **Next-Hop Router** → Forward to next-hop’s MAC address.
* **Drop** → No route and no default route.

### ⚡ Packet Forwarding Mechanisms

* **Process Switching** → Oldest method. CPU checks the routing table for **every packet**, even if the destination repeats. Very slow.
* **Fast Switching** → First packet is process-switched, and its result (next-hop info) is cached. Future packets to the same destination use the cache instead of CPU lookup. Faster than process switching.
* **CEF (Cisco Express Forwarding)** → Modern and default. Builds a **Forwarding Information Base (FIB)** and an **Adjacency Table** in advance, based on routing table changes. This allows very fast lookups since decisions are made before packets arrive.
* **Summary:** CEF > Fast Switching > Process Switching in efficiency.

---

## ⚙️ 14.3 Basic Router Configuration Review

### 🖥️ Initial Setup Commands

```bash
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)# enable secret class
R1(config)# line console 0
R1(config-line)# logging synchronous
R1(config-line)# password cisco
R1(config-line)# login
R1(config)# line vty 0 4
R1(config-line)# password cisco
R1(config-line)# login
R1(config-line)# transport input ssh telnet
R1(config)# service password-encryption
R1(config)# banner motd # Unauthorized access is prohibited! #
```

### 🌍 Interface Configuration Example

```bash
R1(config)# interface gigabitethernet 0/0/0
R1(config-if)# description Link to LAN 1
R1(config-if)# ip address 10.0.1.1 255.255.255.0
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# ipv6 address fe80::1:a link-local
R1(config-if)# no shutdown
```

Repeat for other interfaces (LAN2, Serial link).

### 💾 Save Configuration

```bash
R1# copy running-config startup-config
```

### ✅ Verification Commands

* `show ip interface brief`
* `show running-config`
* `show interfaces`
* `show ip route`
* `ping`
* Replace `ip` with `ipv6` for IPv6 versions.

### 🔎 Output Filtering

```bash
show running-config | section interface
show ip route | include 10.0.0.0
show ip route | exclude static
show ip route | begin Gateway
```

---

## 📚 14.4 IP Routing Table

### 🗂️ Route Sources

* **L** → Local (IP of the router interface itself).
* **C** → Connected network (LANs connected directly to router).
* **S** → Static route (manually configured by admin).
* **O** → OSPF-learned (dynamic).
* **\*** → Candidate default route.

### 🧠 Routing Principles

* Each router makes decisions based on **its own table**.
* Routing info in one router ≠ guaranteed in another.
* Forwarding info does **not guarantee return path**.

### 📄 Routing Table Entries Contain:

* Route source (C, S, O, etc.)
* Destination network + prefix length
* Administrative Distance (AD)
* Metric (lower = better → numerical value used to compare multiple routes learned by the same protocol).
* Next-hop IP
* Exit interface
* Timestamp

### 🔗 Directly Connected Routes

* Learned automatically when interfaces are up and assigned IPs.
* Codes: `C` = network, `L` = interface IP (/32 IPv4 or /128 IPv6).

### 🛠️ Static Routes

* Manually configured → tells router exactly where to send packets for a network.
* Example command:

```bash
R1(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

### 🔄 Dynamic Routing Protocols

* Router learns networks automatically using routing protocols.
* Example: OSPF, RIP, EIGRP.

### 🌐 Default Route

* Works as a “catch-all.”
* If router doesn’t recognize a destination, it forwards packets to this route.
* Example:

```bash
R1(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

* IPv6 equivalent:

```bash
R1(config)# ipv6 route ::/0 2001:db8:acad:2::1
```

### 📋 IPv4 Routing Table Structure

* Parent route (classful network).
* Child routes (indented).

### 📋 IPv6 Routing Table Structure

* No classful structure → all routes shown equally.

### 🔑 Administrative Distance (AD)

* **Lower AD = more trusted**.
* If multiple paths exist, the router selects the route with the **lowest AD**.
* Key values:

  * Directly connected = 0
  * Static = 1
  * EIGRP (internal) = 90
  * OSPF = 110
  * RIP = 120
  * BGP (external) = 20, internal = 200

---

## 🔄 14.5 Static and Dynamic Routing

### 📌 Static Routing

* Best for small/stub networks.
* Used for:

  * Default routes to ISP
  * Routes outside routing domain
  * Explicitly controlled paths

### 📌 Dynamic Routing

* Best for medium/large networks.
* Automatically adapts to topology changes.
* Scales easily.

### 💡 Best Practice

* **Use both Static and Dynamic together.**

  * Static can handle default/stub routes.
  * Dynamic can handle network discovery and adaptability.

### 📊 Static vs Dynamic

| Feature             | Static              | Dynamic                     |
| ------------------- | ------------------- | --------------------------- |
| Config Complexity   | Increases with size | Independent of size         |
| Topology changes    | Manual update       | Auto adapts                 |
| Scalability         | Simple              | Complex networks            |
| Security            | Inherent            | Must be configured          |
| Resources           | None                | Uses CPU, memory, bandwidth |
| Path Predictability | Fixed               | Protocol decides            |

### 🏛️ Routing Protocol Categories

* **IGPs** (within org): RIPv2, EIGRP, OSPF, IS-IS.
* **EGP** (between orgs): BGP.
* Algorithms:

  * Distance Vector (RIP, EIGRP)
  * Link-State (OSPF, IS-IS)
  * Path Vector (BGP)

### 🧮 Dynamic Routing Protocol Components

* **Data structures** (tables)
* **Messages** (hello, updates)
* **Algorithm** (best path selection)

### 🛤️ Best Path Metrics

* **RIP** → Hop count (max 15)
* **OSPF** → Cost (based on bandwidth)
* **EIGRP** → Bandwidth + delay (optionally load/reliability)

### ⚖️ Load Balancing

* **Equal-cost multipath**: If two or more equal-cost paths exist, router forwards using all available paths equally.
* **EIGRP** also supports unequal cost load balancing.

---

## ❓ Additional Notes for NetAcad Questions

* A router determines how to forward an IP packet using the **routing table**.
* If the destination is on a remote network, the router forwards the packet to a **next-hop router**.
* Routing tables may contain **directly connected networks, static routes, dynamic routing protocol routes, and a default route**.
* The **prefix length in the routing table entry** decides the minimum number of far-left bits that must match for a route to be used.
* If a router sends an ARP request for the destination IPv4 address itself, it is forwarding the packet **to the destination device**. If it sends an ARP request for an address in its route entry, it is forwarding **to the next-hop router**.
* By default, Cisco routers use **Cisco Express Forwarding (CEF)** as the forwarding method.
* Important routing table principle: Routing information in one direction does **not** guarantee return path information.
* The route entry **L (Local)** is used if the destination IP matches one of the router’s interface addresses.
* A **stub network** is accessed by a single route, with the router having only one neighbor.
* **OSPF and EIGRP** can automatically discover a new best path when the topology changes.
* A default route can be **static or dynamic**, not just static.
* If both OSPF and a static route exist for the same destination, the **static route is chosen** because it has a lower Administrative Distance.
* **Dynamic routing protocols** adapt to topology changes automatically, while static routes do not.
* Static routes are commonly used with **stub networks**.
* **OSPF uses cost** as its metric for best path selection.
* Using two or more equal-cost paths is called **Equal Cost Load Balancing**.

---

## 📝 Final Word

Routers are the decision-makers of a network. They examine packet headers, consult routing tables, and forward traffic using the **longest prefix match** to ensure accuracy. When multiple paths exist, the router prefers the one with the **lowest Administrative Distance** for trustworthiness, and if costs are equal, it can use **equal-cost load balancing** to optimize performance. By combining static and dynamic routing wisely, and mastering forwarding mechanisms like **CEF**, you gain control over efficient and resilient data delivery. Understanding routing is key to becoming a skilled network engineer. 🚀
