# CCNA ENSA - Semester 3

## 📘 Module 2: Single-Area OSPFv2 Configuration


### 🎯 Objectives

In this module, you will learn how to implement single-area OSPFv2 in **point-to-point** and **broadcast multiaccess** networks, configure and verify the **OSPF Router ID**, configure OSPF in **point-to-point** and **multiaccess networks**, modify single-area OSPFv2 settings such as cost, bandwidth, and timers, configure **default route propagation**, and verify single-area OSPFv2 operation.

---

## 🔑 2.1 OSPF Router ID

* **Router ID (RID):** 32-bit value (IPv4 address format), uniquely identifies an OSPF router.
* Used in:

  * **Database synchronization:** Highest RID sends DBD packets first.
  * **DR/BDR election:** Highest RID becomes DR, second highest becomes BDR.

**Process ID:**

* When you enable OSPF → `router ospf <process-id>`.
* **Process ID** is locally significant (only matters on that router, doesn’t have to match other routers).
* Range: **1–65535**.
* Best practice: use the same process-id for consistency across the network.

**Router ID Selection Order:**

1. Manually set → `router-id X.X.X.X`
2. Highest loopback IP (virtual interface)
3. Highest physical interface IP

**Configure Router ID:**

```bash
R1(config)# router ospf 10
R1(config-router)# router-id 1.1.1.1
R1# show ip protocols | include Router ID
```

**Configure Loopback Interface:**

* A loopback interface is a virtual interface that never goes down unless shut down.

```bash
R1(config)# interface loopback0
R1(config-if)# ip address 1.1.1.1 255.255.255.255
```

* OSPF will use this IP as RID if no manual RID is set.

**Change Router ID:** Requires OSPF process reset.

```bash
R1(config-router)# router-id 1.1.1.1
R1# clear ip ospf process
```

---

## 🔗 2.2 Point-to-Point OSPF Networks

### ➡️ Network Command Syntax

```bash
Router(config-router)# network network-address wildcard-mask area area-id
```

* You put the directly connected networks to the router
* **Wildcard mask** = Inverse of subnet mask (255.255.255.255 – subnet mask).
* Can also use **0.0.0.0** to specify exact interface.

**Example:**

```bash
R1(config-router)# network 10.10.1.0 0.0.0.255 area 0
R1(config-router)# network 10.1.1.5 0.0.0.0 area 0
```

### ➡️ ip ospf Command (Interface Mode)

* Instead of `network` command, you can enable OSPF **directly under an interface**.
* This is useful when you want **precise control** (e.g., advertise only one interface, not an entire subnet).

```bash
Router(config-if)# ip ospf process-id area area-id
```

Example:

```bash
R1(config-if)# ip ospf 10 area 0
```

### 🚫 Passive Interface

* By default, OSPF sends Hello packets out of every enabled interface.
* **Passive-interface** stops sending Hellos but still advertises the network.
* Useful for:

  * **LANs with end devices only** (no routers).
  * **Security:** prevent outsiders from forming adjacencies.

```bash
Router(config-router)# passive-interface g0/0   # Make one interface passive
Router(config-router)# passive-interface default # Make all passive
Router(config-router)# no passive-interface g0/1 # Allow specific interfaces
```

➡️ Making an interface passive means: it will **advertise the subnet** but will **not form OSPF neighbor relationships** on that link.

### 🔄 Point-to-Point vs Broadcast

* By default, Ethernet interfaces elect **DR/BDR**, even in point-to-point links.
* To disable election:

```bash
R1(config-if)# ip ospf network point-to-point
```

### 🌀 Loopback Interfaces

* Virtual interfaces that are always up.
* By default: advertised as `/32` host routes.
* To simulate a LAN:

```bash
R1(config-if)# ip ospf network point-to-point
```

* Loopbacks are preferred for **Router IDs** because they don’t go down like physical interfaces.

---

## 🌐 2.3 Multiaccess OSPF Networks

### 🏆 DR/BDR Election

* **DR:** Collects & distributes LSAs (uses 224.0.0.5).
* **BDR:** Standby, promotes itself if DR fails.
* **DROTHER:** Neither DR nor BDR, communicates via 224.0.0.6.

### 🔍 Verify Roles

```bash
R1# show ip ospf interface g0/0/0
```

### 🤝 Verify Adjacencies

```bash
R2# show ip ospf neighbor
```

Neighbor states:

* FULL/DROTHER
* FULL/DR
* FULL/BDR
* 2-WAY/DROTHER

### ⚙️ DR/BDR Election Rules

1. Highest interface **priority** wins (0–255, default = 1).

   * `ip ospf priority <value>`
   * `0` = never DR/BDR.
2. If tie → highest RID wins.

Example:

```bash
R1(config-if)# ip ospf priority 255
R1# clear ip ospf process
```

➡️ **If a higher priority router enters the network later, it will NOT become DR immediately.** OSPF does not preemptively re-elect DR/BDR. Only if the current DR fails, then election happens again.

➡️ `clear ip ospf process` forces OSPF to reset → all adjacencies are torn down and rebuilt, effectively triggering a new election.

---

## ⚒️ 2.4 Modify Single-Area OSPFv2

### 📊 OSPF Cost Metric

* Formula: **Cost = Reference Bandwidth / Interface Bandwidth**
* Default reference BW = 100,000,000 bps (100 Mbps).

**Adjust Reference Bandwidth:**

```bash
R1(config-router)# auto-cost reference-bandwidth 1000   # For 1 Gbps
R1(config-router)# auto-cost reference-bandwidth 10000  # For 10 Gbps
```

**Manually Set Cost:**

```bash
R1(config-if)# ip ospf cost 30
```

### ⏱️ OSPF Timers

* **Hello Interval:** Default 10 sec
* **Dead Interval:** Default 40 sec (4 × Hello)

**Modify Timers:**

```bash
R1(config-if)# ip ospf hello-interval 5
R1(config-if)# ip ospf dead-interval 20
```

**Verify:**

```bash
R1# show ip ospf interface g0/0/0
R1# show ip ospf neighbor
```

---

## 🚪 2.5 Default Route Propagation

* Sometimes an OSPF domain needs a **default route to the internet**. This is done by the **edge router** (ASBR).
* Steps:

  1. Create a static default route → `ip route 0.0.0.0 0.0.0.0 <exit-intf | next-hop>`
  2. Tell OSPF to advertise it → `default-information originate`

**Example:**

```bash
R2(config)# interface loopback1
R2(config-if)# ip address 64.100.0.1 255.255.255.255

R2(config)# ip route 0.0.0.0 0.0.0.0 loopback 1
R2(config)# router ospf 10
R2(config-router)# default-information originate
```

* `ip route 0.0.0.0 0.0.0.0 loopback 1` → creates a default route pointing to loopback1 (simulating internet).
* `default-information originate` → tells R2 to **inject this default route into OSPF** so that all routers learn it.

**Verify:**

```bash
R2# show ip route          # Static default route
R1# show ip route | begin Gateway   # OSPF-learned default route (O*E2)
```

➡️ `O*E2` means: OSPF External type 2, default route redistributed into OSPF.

---

## 🔎 2.6 Verify Single-Area OSPFv2

### Useful Commands

```bash
show ip interface brief       # Verify interfaces
show ip route                 # Verify routes
show ip ospf neighbor         # Verify adjacencies
show ip protocols             # Verify OSPF process, RID, networks
show ip ospf                  # OSPF process details
show ip ospf interface        # Verify interface settings
show ip ospf interface brief  # Quick summary
```

---

## 📌 Key Takeaways

* Always configure **Router IDs** manually for stability.
* Use **passive-interface** on non-OSPF links for security & efficiency.
* Set **ip ospf network point-to-point** to avoid unnecessary DR/BDR elections.
* Adjust **reference bandwidth** in high-speed networks.
* Use `default-information originate` to propagate default routes.
* Verification commands are crucial: `show ip ospf`, `show ip protocols`, `show ip ospf neighbor`.

---

## 🏁 Final Word

Mastering single-area OSPFv2 ensures you can build scalable, efficient, and secure internal routing domains. By understanding router IDs, interface configurations, DR/BDR elections, cost metrics, and default route propagation, you gain the foundation to troubleshoot and design reliable OSPF networks.
