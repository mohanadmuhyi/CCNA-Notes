# CCNA ENSA - Semester 3

## 🌍 Module 11: Network Design

### 🎯 Objective

Explain the characteristics of scalable network architectures.

---

## 🏛️ 11.1 Hierarchical Networks

### ⚙️ The Need to Scale

Modern organizations depend on networks for **mission-critical services**.
Networks must support:

* Converged traffic (data, voice, video)
* Critical applications
* Centralized control
* Diverse business needs

Campus designs range from **a single switch LAN** to **large multi-building topologies**.

---

### 🌐 Borderless Switched Networks

Cisco’s **Borderless Network** allows connectivity:

> Anyone, anywhere, anytime, on any device — securely and reliably.

Key traits:

* Unified wired & wireless access
* Built on hierarchical, modular, scalable, and resilient infrastructure
* Flexible and adaptable to growth

#### 💡 What Each Term Means

**Hierarchical:**
Organizes the network into layers (Access, Distribution, Core) for manageability, scalability, and performance. Each layer has a defined function.

* Example: PCs and printers connect to **Access**, switches interconnect at **Distribution**, and high-speed links form the **Core** backbone.

**Modular:**
Divides the network into **independent modules** (e.g., departments or buildings).

* Example: A university might have separate modules for the engineering, science, and admin buildings — each operating independently.

**Scalable:**
Allows easy expansion without affecting existing operations.

* Example: Adding a new switch to a module without redesigning the entire network.

**Resilient:**
Ensures **high availability** through redundancy and fault tolerance.

* Example: Redundant core switches or links prevent outages if one fails.

---

### 🧱 Hierarchy in the Borderless Switched Network

A **tiered design** includes:

* **Access Layer**
* **Distribution Layer**
* **Core Layer**

Two main models:

1. **Three-Tier Layer**
2. **Two-Tier Layer (Collapsed Core)**

---

### 🔌 Access, Distribution, and Core Layers

**Access Layer:**

* Provides direct access for end devices (PCs, printers, IP phones).
* Implements VLANs, port security, and PoE.
* Devices: Cisco Catalyst 2960 or 9200 switches, Access Points.

**Distribution Layer:**

* Aggregates traffic from access switches.
* Implements routing, ACLs, and QoS.
* Devices: Cisco Catalyst 3850 or 9300 (Layer 3 switches).

**Core Layer:**

* Provides **high-speed backbone** and fault isolation.
* Focuses on speed and redundancy, not packet filtering.
* Devices: Cisco Catalyst 9600, Nexus 9000.

---

### 🏢 Three-Tier vs. Two-Tier Examples

**Three-Tier:**

* For **large enterprises**.
* Extended-star topology from central building to others.

**Two-Tier (Collapsed Core):**

* Combines distribution + core for **smaller sites** or **single buildings**.

---

### 🔄 Role of Switched Networks

* Networks evolved from **hub-based** to **hierarchical switched LANs**.
* Provides:

  * Flexibility
  * QoS
  * Security
  * Wireless & IP telephony integration

---

## 🚀 11.2 Scalable Networks

### 📈 Design for Scalability

A **scalable network** grows without losing **availability** or **performance**.
Achieved by:

* Redundancy 🔁
* Multiple links
* Scalable routing protocols
* Wireless connectivity

### 🔁 Plan for Redundancy

To avoid single points of failure:

* Install **duplicate devices**
* Use **failover services**
  ⚠️ Ethernet redundancy can cause **loops**, so **STP** (Spanning Tree Protocol) is required.

### 🧩 Reduce Failure Domain Size

* **Failure domain** = area impacted by a problem.
* Controlled via **hierarchical design**, limited to the **distribution layer**.
* Routers act as gateways for small user groups.
* **Switch blocks** ensure one device’s failure doesn’t affect others.

### ⚡ Increase Bandwidth

Use **EtherChannel** for link aggregation:

* Combines multiple links into one **Port Channel**.
* Simplifies management & load balancing.

### 📡 Expand the Access Layer (Wireless)

Wireless LANs (WLANs) extend access with flexibility and mobility.
Considerations: device types, coverage, interference, and security.
Each device needs a **wireless NIC** and connects to a **router/AP**.

### 🧭 Tune Routing Protocols

Use **OSPF** for large networks:

* Link-state, supports hierarchical areas.
* Synchronizes link-state databases via **LSAs**.

---

## 🖥️ 11.3 Switch Hardware

### 🧩 Switch Platforms

**Campus LAN Switches (e.g., Cisco 3850/9300):**

* High port density, QoS, and security.
* Ideal for access/distribution layers.

**Cisco Meraki Cloud-Managed Switches:**

* Controlled via cloud dashboard.
* Enables virtual stacking for multi-site management.

**Cisco Nexus Series (Data Center):**

* Designed for **high-performance** and **low latency**.
* Supports technologies like **VXLAN** and **Spine-Leaf** design.

**Service Provider Switches:**

* Used by ISPs for large-scale customer access.
* Provide virtualization, QoS, and integrated security.

**Virtual Networking Switches (Nexus 1000V):**

* Run inside virtualized environments.
* Provide network segmentation and tenant isolation.

### 🔋 Power over Ethernet (PoE)

**Concept:** Transmits power and data through the same Ethernet cable.

**Advantages:**

* Simplifies device installation (no power outlets needed).
* Enables remote reboot/power control.

**Standards:**

* **PoE (802.3af):** 15.4W/port.
* **PoE+ (802.3at):** 30W/port.
* **UPOE (802.3bt):** up to 100W/port.

**Example:** Cisco Catalyst 9300 can power 48 IP phones or APs.
**Considerations:** Plan total power budget, use quality cables, and confirm device compatibility.

### ⚙️ Forwarding Rates

Defines data handling per second.

* Enterprise switches: high forwarding rate.
* Access switches: limited by uplinks.

### 🚦 Multilayer Switching

* Combines Layer 2 switching + Layer 3 routing.
* Uses **ASICs** for fast hardware-based forwarding.

### 💼 Business Considerations for Switch Selection

| Factor           | Description                           |
| ---------------- | ------------------------------------- |
| 💰 Cost          | Depends on ports, speed, and features |
| 🔌 Port Density  | Number of supported devices           |
| ⚡ Power          | PoE support and redundancy            |
| 🔒 Reliability   | Continuous uptime                     |
| 🚀 Speed         | Affects end-user experience           |
| 📦 Frame Buffers | Handle congestion                     |
| 📈 Scalability   | Allows future growth                  |

---

## 🌐 11.4 Router Hardware

### 📦 Router Requirements

Routers use the network portion (prefix) of the destination IP address to route packets to the proper destination.
They also:

* Provide **broadcast containment** by limiting broadcasts to the local network.
* Interconnect geographically separated locations.
* Group users logically by application or department.
* Provide enhanced security via **Access Control Lists (ACLs)**.

Routers select alternate paths if a link goes down and serve as the **default gateway** for all local hosts.

### 🧭 Cisco Routers

**Branch Routers (Cisco ISR 4000 Series):**

* Optimize branch services on a single platform.
* Deliver high application performance across WANs.
* Example: ISR 4331 router.

**Network Edge Routers (Cisco ASR 9000 Series):**

* Enable the network edge to provide secure, high-performance services.
* Connect campus, data center, and branch networks.

**Service Provider Routers (Cisco NCS 6000 Series):**

* Deliver scalable, end-to-end solutions and subscriber-aware services.
* Handle massive routing tables for ISPs and carriers.

**Industrial Routers (Cisco 1100 Series):**

* Built for rugged and harsh environments.
* Provide enterprise-class features like VPN and security.

### 🧱 Router Form Factors

* **Cisco 900 Series:** Small branch routers with WAN, switching, and security in compact form.
* **Cisco ASR 9000/1000 Series:** Aggregation routers offering density, resiliency, and programmability.
* **Cisco NCS 5500 Series:** Designed to scale between large data centers and service provider networks.
* **Cisco 800 Industrial Series:** Compact routers designed for industrial and harsh environments.

---

## 🧠 Additional Notes for NetAcad Questions

* **Resilient networks** are always accessible and maintain uptime.
* **Modular networks** allow on-demand service expansion and scalability.
* **Flexible networks** optimize use of all available resources for load sharing.
* **Core Layer** provides high-speed backbone and fault isolation.
* **Access Layer** connects directly to end devices.
* **Distribution Layer** integrates users with backbone and manages routing, switching, and security.
* Scalable design should include:

  * Hierarchical structure.
  * Modular or expandable hardware.
  * Routers/multilayer switches to filter traffic and limit broadcasts.
* **OSPF** is ideal for large networks due to its hierarchical area support.
* Key scalable network features:

  * Redundant links.
  * Multiple physical connections.
  * Expandable modular devices.
* **Service Provider Switches** aggregate traffic at the edge of the network.
* **Modular switches** have field-replaceable line cards.
* **Stackable switches** operate as one logical unit.
* **Port Density** refers to the number of ports on a switch.
* **Forwarding Rate** measures how much data a switch can process per second.
* **Multilayer switches** support routing protocols and forward IP packets at near Layer 2 speed.
* **Network Edge Routers** connect data centers, campuses, and branches securely.
* **Service Provider Routers** deliver end-to-end services to subscribers.
* **Branch Routers** provide simple configuration for LANs and WANs.
* **Industrial Routers** are designed for rugged environments.

---

## ✨ **Final Word**

Network design must be **hierarchical, modular, scalable, and resilient** to ensure growth, reliability, and performance across all layers of modern enterprise networks. 🌍💡
