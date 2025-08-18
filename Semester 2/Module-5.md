# CCNA SRWE - Semester 2

## 🌳 Module 5: STP Concepts


### 🎯 Objectives

The objective of this module is to explain why **STP (Spanning Tree Protocol)** is needed in redundant Layer 2 networks. It also aims to help you understand how **STP operations** work, including root bridge election, port roles, timers, and port states. Finally, you will learn about the **evolution of STP**, including PVST+, RSTP, Rapid PVST+, and MSTP.

---

## 5.1 🔄 Purpose of STP

### 🛡 Why Redundancy Causes Problems

* Redundancy = multiple paths = resilience against failures.
* But in **Layer 2 Ethernet**, multiple active paths cause **loops**.
* Example: **Two links between two switches** → creates a loop inside the broadcast domain.
* Issues with loops:

  * **MAC table instability** (constantly updating).
  * **Broadcast storms** (frames endlessly circling).
  * **High CPU usage** on switches → network becomes unusable.

👉 Unlike IP (Layer 3) which uses **TTL (Time-to-Live)** to prevent loops, **STP prevents loops at Layer 2**.

---

### 🌳 What is STP?

* **STP = Spanning Tree Protocol** (IEEE 802.1D).
* **Loop-prevention protocol** → allows redundancy while blocking redundant paths.
* STP dynamically **blocks certain ports** to form a **loop-free logical topology**.
* If a link fails, STP **recalculates** and unblocks backup links.

---

### ⚠️ Problems Without STP

1. **Layer 2 Loops** → unknown unicast, broadcast, multicast flood endlessly.
2. **Broadcast Storms** → huge amounts of broadcasts disable network.
3. **MAC Table Instability** → switch constantly relearns incorrect MAC locations.

---

### 🧠 Spanning Tree Algorithm (STA)

* Invented by **Radia Perlman (1985)**.
* Builds a **loop-free topology** like a tree:

  * **Root Bridge** = reference switch.
  * Other switches **calculate least-cost paths** to root.
  * **Redundant paths** are **blocked**.
  * If a failure occurs → ports are unblocked dynamically.

```
          [Root Bridge]
          /          \
      [Switch A]   [Switch B]
         |   \\       /   |
         |    \\     /    |
         -----[Switch C]--
         (One port will be blocked to avoid loop)
```

---

## 5.2 ⚙️ STP Operations

### 📌 4 Steps to a Loop-Free Topology

1. **Elect Root Bridge**
2. **Elect Root Ports** (closest to root bridge)
3. **Elect Designated Ports** (best path per segment)
4. **Block Alternate Ports**

---

### 🔑 Bridge Protocol Data Units (BPDUs)

* Used by switches to share STP info.
* Contains **Bridge ID (BID)**.

  * BID size = **8 bytes** → **2 bytes Priority + 2 bytes Extended System ID (VLAN) + 6 bytes MAC Address**.
* **Lowest BID wins** → that switch becomes **Root Bridge**.
  👉 If priorities are equal → lowest MAC wins.

---

### 🏆 Step 1: Elect Root Bridge

* Switch with **lowest BID** becomes root.
* All its ports = **Designated Ports**.
* Default role of a port on a switch = **Designated Port** until STP calculations change it.

---

### 📐 Step 2: Elect Root Ports

* Every **non-root switch** chooses **one root port** = path with **lowest total cost** to root.
* **Root Port cannot be blocked.** It’s the switch’s “best way” to reach the Root Bridge.
* **Example:** If Switch S2 has two paths to Root Bridge S1:

  * Path1 via Fa0/1 cost = 19
  * Path2 via Fa0/2 cost = 38
    👉 S2 chooses Fa0/1 as its **Root Port**.
* Important: **Root Ports are never on the Root Bridge itself.**

👉 Tie-breaking if costs are equal:

* Compare **lowest Sender BID**.
* If equal, compare **lowest Sender Port Priority**.
* If equal, compare **lowest Sender Port ID**.
* **Port ID = Port Priority (default 128) + Interface number of the sender’s interface** (not the local switch’s interface).

  * Example: If two paths exist with equal cost → one via Fa0/1, one via Fa0/2 → the path connected to the **lower sender port ID** wins.

---

### 🖇 Step 3: Elect Designated Ports

* For each **link/segment**, one port becomes the **Designated Port**.
* Rules:

  * On **Root Bridge** → all ports are Designated.
  * If one side = Root Port → the other side = Designated.
  * Ports connected to end devices = Designated.

---

### 🚫 Step 4: Elect Alternate Ports

* After identifying all **Root Ports** and **Designated Ports**, there may still be links with no role assigned.
* In such cases: one end becomes **Designated**, the other becomes **Alternate (backup)**.
* Choice based on **Priority → MAC address** (lowest wins Designated).
* Alternate = closed temporarily but ready to open if needed.

---

### ⏱ STP Timers

* **Hello Time**: 2s (interval between BPDUs).
* **Forward Delay**: 15s (Listening + Learning).
* **Max Age**: 20s (before topology change).
  👉 Root Bridge controls and propagates timer values.
  👉 To avoid instability with these timers, **IEEE recommends a maximum diameter of 7 switches** in a spanning tree network when using defaults.

---

### 📊 STP Port States

* **Blocking**: Receives BPDUs only, no traffic.
* **Listening**: Receives/sends BPDUs, no learning.
* **Learning**: Learns MACs, no forwarding.
* **Forwarding**: Learns + forwards frames.
* **Disabled**: Admin down.

---

### 📉 STP Path Costs by Speed (IEEE 802.1D Standard)

| Link Speed | STP Cost |
| ---------- | -------- |
| 10 Mbps    | 100      |
| 100 Mbps   | 19       |
| 1 Gbps     | 4        |
| 10 Gbps    | 2        |

---

### 🖥 Example with 4 Switches

```
          [S1] Root Bridge
         /   \
     (19)     (19)
      /         \
   [S2]         [S3]
     \\         //
     (19)   (19)
        \\   //
          [S4]
```

* S1 = Root Bridge. All its ports = **Designated**.
* S2 and S3 each choose their lowest cost link to S1 as **Root Ports**.
* On S4: has equal cost links to S2 and S3 → uses **tie-breaking rules** (BID → Port Priority → Port ID). One port = Root Port, the other = Alternate.

---

### 🔀 Per-VLAN Spanning Tree (PVST)

* Cisco enhancement.
* **Separate STP instance per VLAN**.
* Allows different **Root Bridges per VLAN** for load balancing.

---

## 5.3 ⚡ Evolution of STP

### 📚 STP Versions

* **STP (802.1D original)** → one tree for entire network.
* **PVST+ (Cisco)** → one STP per VLAN, supports PortFast, UplinkFast, BPDU Guard, etc.
* **RSTP (802.1w)** → Faster convergence, backward compatible.
* **Rapid PVST+ (Cisco)** → RSTP per VLAN.
* **MSTP (802.1s)** → Multiple VLANs mapped to same spanning tree instance.
* **MST (Cisco)** → Up to 16 RSTP instances.

---

### ⚡ RSTP (802.1w)

* Faster convergence than STP.
* **Port States (RSTP):**

  * **Discarding**: Combines Blocking, Listening, Disabled. Drops all data frames but still processes BPDUs.
  * **Learning**: Builds MAC address table but does not forward data yet.
  * **Forwarding**: Learns and forwards frames.
* **Port Roles (RSTP):**

  * **Root Port**: Best path to Root Bridge.
  * **Designated Port**: Best path for a segment towards Root Bridge.
  * **Alternate Port**: Backup path to Root Bridge (used if Root Port fails).
  * **Backup Port**: Backup for a Designated Port on the same switch (rare, only with hubs).
* Because of these roles, RSTP can quickly reassign ports without waiting for long timer expirations.

---

### ⚡ Cisco Features for Edge Ports

* **PortFast** → skips Listening/Learning, immediately forwards. Used on **access ports only**.
* **BPDU Guard** → shuts down PortFast port if BPDU received. Prevents loops.

---

### 🚀 Alternatives to STP

* Modern networks prefer **Layer 3 routing** for redundancy.
* Allows loops without blocking ports.
* STP still used at **access layer**, but distribution/core use Layer 3.

---

## 🖥 Commands Related to STP

```bash
S1# show spanning-tree

S1(config)# spanning-tree vlan <id> priority <value>

S1(config-if)# spanning-tree vlan <id> cost <value>

S1(config-if)# spanning-tree portfast

S1(config-if)# spanning-tree bpduguard enable

S1(config-if)# spanning-tree vlan <id> port-priority <value>
```

---

## ✅ Key Takeaways (Revision)

* STP prevents **Layer 2 loops** by blocking redundant links.
* **Election order:** Root Bridge → Root Port → Designated Port → Alternate.
* Uses **BPDUs + BID (Priority + VLAN ID + MAC)** to decide roles.
* Default timers: Hello 2s, Forward Delay 15s, Max Age 20s.
* **Maximum diameter of STP with default timers = 7 switches.**
* STP Port States: Blocking, Listening, Learning, Forwarding, Disabled.
* RSTP Port States: Discarding, Learning, Forwarding.
* RSTP Port Roles: Root, Designated, Alternate, Backup.
* Cisco enhancements: **PVST+, Rapid PVST+, MST, PortFast, BPDU Guard**.

---

## 📘 Additional Notes for NetAcad Questions

* **STP** is a **Layer 2 loop prevention protocol** for Ethernet LANs.
* Without STP, **unknown unicast, multicast, and broadcast frames** cause catastrophic loops.
* The device elected by STA is the **Root Bridge**. All other switches calculate their least-cost path to it.
* By default, the **MAC address** decides the root bridge when all priorities are the same.
* The switch with the **lowest bridge ID** is chosen as the root bridge.
* The port closest to the root bridge in terms of cost is the **Root Port**.
* On a segment with two switches, the port with the lowest path cost to the root bridge is the **Designated Port**.
* **Designated ports and Root ports forward traffic**, while Alternate/Blocked ports do not.
* The sum of costs from a switch to the root bridge is called the **Root Path Cost**.
* A switch sends **BPDUs every 2 seconds** by default.
* In **RSTP**, the states **Disabled, Blocking, and Listening** are merged into the **Discarding** state.
* The protocol designed to bring faster convergence is **RSTP**.
* **PortFast** is the feature that solves DHCP delay issues by skipping Listening and Learning.

---

## 🏁 Final Word

STP is fundamental for preventing loops and keeping Ethernet LANs stable when redundancy is introduced. It builds a loop-free tree by electing a root bridge and assigning port roles. RSTP improved convergence, while Cisco’s PVST+, Rapid PVST+, and MSTP provided scalability and VLAN support. Features like PortFast and BPDU Guard further secure access ports. Even though Layer 3 routing is often used in modern designs, STP knowledge remains essential for troubleshooting and designing robust Layer 2 networks.

