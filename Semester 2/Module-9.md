# CCNA SRWE - Semester 2

## 🛡️ Module 9: FHRP Concepts

### 🎯 Objectives

Explain how **First Hop Redundancy Protocols (FHRPs)** provide default gateway services in a redundant network.

---

## 9.1 🔄 First Hop Redundancy Protocols (FHRPs)

### ⚠️ Default Gateway Limitations

* End devices usually configure **one default gateway IPv4 address**.
* If that gateway router interface fails → hosts **lose outside connectivity**.
* Even if a backup router exists, hosts cannot automatically switch to it.

✅ Solution → **FHRPs** provide **alternate default gateways** in switched networks with 2+ routers in the same VLAN.

---

### 🖧 Router Redundancy (Virtual Router)

* Routers use a **virtual router** to avoid single point of failure.
* **Multiple routers** act as **one router** to LAN hosts.
* They share a **virtual IP + virtual MAC**.
* Hosts configure the **virtual IP** as default gateway.
* ARP returns the **virtual MAC**, mapped to the current Active router.
* If Active fails → **Standby takes over** seamlessly.

📌 This is **First-Hop Redundancy**.

---

### 🔁 Steps for Router Failover

1. Standby router stops receiving **Hello messages** from Active.
2. Standby becomes the new **Active router**.
3. New Active takes over the **virtual IP + MAC** → seamless host experience.

---

### 📚 FHRP Options

| Protocol                                   | Description                                                                                         |
| ------------------------------------------ | --------------------------------------------------------------------------------------------------- |
| **HSRP (Hot Standby Router Protocol)**     | Cisco-proprietary. Transparent failover. Active + Standby router.                                   |
| **HSRP for IPv6**                          | Same as HSRP but for IPv6. Uses virtual MAC + virtual IPv6 link-local. Sends Router Advertisements. |
| **VRRPv2**                                 | Open standard. IPv4 only. Master + backups.                                                         |
| **VRRPv3**                                 | Supports IPv4 + IPv6. Multi-vendor, more scalable.                                                  |
| **GLBP (Gateway Load Balancing Protocol)** | Cisco-proprietary. Provides failover **+ load balancing**.                                          |
| **GLBP for IPv6**                          | Same as GLBP but for IPv6. Default gateway redundancy + load sharing.                               |
| **IRDP (ICMP Router Discovery Protocol)**  | Legacy. IPv4 only. Rarely used today.                                                               |

---

## 9.2 🔥 HSRP (Hot Standby Router Protocol)

### 📖 Overview

* Cisco-proprietary (IPv4 + IPv6).
* Provides **transparent failover** for default gateways.
* Group of routers:

  * **Active Router** → forwards packets.
  * **Standby Router** → backup, ready to take over.
* Hosts always point to **virtual IP**.

---

### 🏆 HSRP Priority & Preemption

* Default election: Router with **highest IPv4 address** wins.
* Better practice: Use **priority value** (default = 100).
* Higher priority = Active.
* Range: **0–255**.

⚡ **Preemption**

* By default, once a router is Active, it stays Active.
* To allow higher-priority router to take over → enable **preemption**:

```
R1# configure terminal
R1(config)# interface g0/0
R1(config-if)# standby 1 ip 192.168.1.254
R1(config-if)# standby 1 priority 110
R1(config-if)# standby 1 preempt
```

---

### ⏱️ HSRP States & Timers

**States:**

1. **Initial** – Config applied / interface up.
2. **Learn** – Doesn’t know virtual IP yet.
3. **Listen** – Knows virtual IP, not active/standby.
4. **Speak** – Sends Hellos, election participation.
5. **Standby** – Backup, ready to take over.

**Timers:**

* **Hello** = 3 sec
* **Hold** = 10 sec
* Can tune, but not <1s (Hello) or <4s (Hold).

---

## 9.3 🌍 VRRP (Virtual Router Redundancy Protocol)

### 📖 Overview

* Open standard protocol (multi-vendor).
* Functions similar to HSRP.
* One router = **Master**, others = **Backups**.
* **VRRPv2** → IPv4 only.
* **VRRPv3** → IPv4 + IPv6 support.

---

### ⚙️ VRRP Commands

```
R1# configure terminal
R1(config)# interface g0/0
R1(config-if)# ip address 192.168.1.2 255.255.255.0
R1(config-if)# vrrp 1 ip 192.168.1.254
R1(config-if)# vrrp 1 priority 120
R1(config-if)# vrrp 1 preempt
```

---



## 📝 Summary / Key Points

* **FHRP** ensures LAN hosts always have a working default gateway.
* Uses **virtual IP + MAC** shared between routers.
* **Failover** → Standby router takes over seamlessly.
* **HSRP** = Cisco proprietary, uses Priority + Preemption, has 5 states.
* **VRRP** = Open standard, supports IPv4 (v2) and IPv6 (v3).
* **GLBP** = Cisco proprietary, adds **load balancing**.
* **IRDP** = Legacy.

---

## 📌 Additional Notes for NetAcad Questions

* **Default Gateway**: The device that routes traffic destined for other networks when host lacks explicit routing info.
* **Virtual Router**: Appears as a single router to LAN hosts, but is actually a group of routers working together.
* **Standby Router**: Part of a virtual group; takes over when Active fails → alternate default gateway.
* **Forwarding Router (Active)**: Part of virtual group; assigned as the actual default gateway.
* **Cisco-Proprietary FHRPs**: HSRP (IPv4), HSRP for IPv6, GLBP (IPv4 + IPv6).
* **HSRP Default Priority**: 100.
* **HSRP Priority Rule**: Higher priority router does **not** take over unless **preemption** is enabled.
* **HSRP Speak State**: Interface begins sending periodic Hello messages.
* **HSRP Learn State**: Router has not yet determined virtual IP.

---

## ✅ Final Word

Mastering FHRPs (HSRP, VRRP, GLBP) ensures you can design networks with **redundancy, resiliency, and high availability** – critical skills for any network engineer.
