# CCNA ENSA - Semester 3

## 📡 Module 1: Single-Area OSPFv2 Concepts

### 🎯 Objectives

Explain how single-area OSPF operates in both point-to-point and broadcast multiaccess networks.

---

## 🔑 1.1 OSPF Features and Characteristics

### 📝 Introduction to OSPF

* **OSPF (Open Shortest Path First)**: Link-state routing protocol (alternative to RIP).
* **Advantages over RIP**:

  * Faster convergence.
  * Scales to larger networks.
* **Link**: Router interface, network segment between routers, or stub network (e.g., LAN).
* **Link-state info**: Includes network prefix, prefix length, and cost.
* Uses **areas** to control routing update traffic.
* Focus here: **Single-area OSPF** (all routers in one area, usually Area 0).
* **Maximum recommended routers per OSPF area: \~50** (practical design limit).

### ⚙️ OSPF Components

* Routers exchange **5 packet types**:

  1. **Hello** – discover neighbors.
  2. **Database Description (DBD)** – database synchronization.
  3. **Link-State Request (LSR)** – request specific link info.
  4. **Link-State Update (LSU)** – send LSAs.
  5. **Link-State Acknowledgment (LSAck)** – confirm receipt.

* Databases built from messages:

  * **Adjacency Database** → Neighbor Table (`show ip ospf neighbor`). *Unique for each router.*
  * **Link-State Database (LSDB)** → Topology Table (`show ip ospf database`). *Same on all routers in an area.*
  * **Forwarding Database** → Routing Table (`show ip route`). *Unique for each router.*

* **Link-state** = the networks directly connected to the router and its interfaces.

* **LSA (Link-State Advertisement)**: Carries link-state information that routers flood to neighbors.

* **SPF Algorithm (Dijkstra)**:

  * Builds SPF tree with router as root.
  * Chooses shortest path based on **cumulative cost**.
  * Best routes → Routing Table.

### 🔄 Link-State Operation (Steps to Convergence)

1. Establish Neighbor Adjacencies.
2. Exchange LSAs.
3. Build LSDB.
4. Run SPF Algorithm.
5. Install Best Route.

### 🌐 OSPF Area Types

* **Single-Area OSPF**: All routers in one area (best practice: Area 0). Simple design, easier to manage, useful for small/medium networks.
* **Multiarea OSPF**: Multiple areas connected to backbone (Area 0). Uses **ABRs** (Area Border Routers).

**Benefits of Multiarea**:

* Smaller routing tables (route summarization possible).
* Less LSA flooding within each area.
* Reduced SPF recalculations (topology changes localized to an area).
* Improves scalability for large enterprise networks.
* Provides hierarchical structure, easier troubleshooting, and better performance.

### 🌍 OSPFv3 (IPv6)

* Same as OSPFv2 but for IPv6 prefixes.
* Uses IPv6 transport.
* Separate process from IPv4 OSPF.
* With **Address Families**, OSPFv3 can support both IPv4 and IPv6 (not in this module).

---

## 📦 1.2 OSPF Packets

### 📑 Types of OSPF Packets

1. **Hello** – discover neighbors, maintain adjacencies, elect DR/BDR.

   * Router sends Hello packets; if another router replies, OSPF is running there too.
   * Contains parameters like RID, Area ID, Hello interval, Dead interval, priority, DR, and BDR fields.
   * Used to ensure two routers agree on settings before adjacency.
2. **DBD (Database Description)** – summarizes LSDB contents for synchronization.

   * Provides an abbreviated list of LSAs.
   * Helps check database consistency between neighbors.
3. **LSR (Link-State Request)** – requests specific LSAs not found in local LSDB.

   * Sent when router detects missing or outdated info.
4. **LSU (Link-State Update)** – carries LSAs (link changes, topology updates).

   * Used to announce new information.
   * Can contain multiple LSAs in a single packet.
5. **LSAck** – acknowledges receipt of LSUs and DBDs (not used for Hellos).

   * Ensures reliable delivery.

* **LSU vs LSA**: LSU (packet) contains one or more LSAs (messages).
* **LSA Types**: Include Router LSAs, Network LSAs, Summary LSAs, and External LSAs.
* **Hello Timers**:

  * Hello interval (default 10s on Ethernet).
  * Dead interval (default 40s) → usually 4x Hello interval.
* OSPF packet header includes: version, type, packet length, RID, Area ID, checksum, authentication, etc.

---

## ⚡ 1.3 OSPF Operation

### 🛠️ 7 OSPF States

1. **Down** – No Hello received. Router starts sending Hellos.
2. **Init** – Hello received (contains sender RID).
3. **Two-Way** – Bidirectional comms established. On multiaccess, DR/BDR election takes place. After election:

   * Routers have **Two-Way state with DROTHERs**.
   * Routers form **Full state with DR and BDR**.
4. **ExStart** – Decide which router will send the first DBD (based on highest RID).
5. **Exchange** – Routers exchange DBD packets containing LSDB summaries.
6. **Loading** – Routers send LSRs for missing LSAs, receive LSUs in response. SPF calculation performed.
7. **Full** – LSDBs fully synchronized with DR/BDR (or directly on point-to-point).

### 🆔 Router ID (RID) Selection

Order of preference:

1. **Manually configured RID**.
2. Highest IP of a **loopback (virtual) interface**.

   * Preferred because it never goes down, safer choice.
3. Highest IP of a **physical interface**.

### 👑 DR/BDR Election and Database Sync

* On multiaccess networks:

  * **DR = Highest RID**.
  * **BDR = Second highest RID**.
  * Other routers = DROTHER.
* Each router sends its database **only to DR and BDR**.
* DR builds the LSDB, sends it back to all routers.
* BDR keeps a copy in case DR fails.
* Advantage: Instead of every router flooding all routers, traffic is reduced.

### 🤝 Neighbor Adjacency Process

* Routers send **Hello** to multicast **224.0.0.5**.
* If Hello matches neighbor criteria → adjacency formed.
* On point-to-point: direct adjacency forms (no DR/BDR needed).
* On Ethernet: DR/BDR election occurs.

### 🔄 Database Synchronization

1. Highest RID router sends DBD first.
2. Routers exchange DBDs.
3. If missing info → LSR sent.
4. LSU replies.
5. LSDBs synchronized → routers in **Full state**.

* Updates triggered:

  * On topology change (incremental).
  * Every 30 minutes (refresh).

### 📡 Need for DR/BDR (Multiaccess Networks)

* **Problem**: Too many adjacencies → flooding chaos.
* **Solution**: Elect DR to manage LSA distribution, BDR as backup.
* Other routers = DROTHER (communicate LSAs only with DR/BDR).

### 🔗 Virtual Interfaces

* Used when two routers in different networks need to establish OSPF adjacency.
* Configured as a **virtual link** through another OSPF area.
* Example use case: when an area is not directly connected to backbone (Area 0).

---

## ✅ Key Takeaways

* OSPF = Link-state protocol, uses **areas**.
* Uses **SPF (Dijkstra)** algorithm.
* 3 main databases: **Neighbor, LSDB, Routing Table**.
* Adjacency & forwarding DBs are **unique per router**, LSDB is **identical across all routers in an area**.
* Convergence steps: Neighbor adjacency → LSA exchange → LSDB → SPF → Routing table.
* 5 packet types: Hello, DBD, LSR, LSU, LSAck.
* **LSAck acknowledges all except Hellos**.
* **7 states**: Down → Init → Two-Way (with DROTHERs) → ExStart → Exchange → Loading → Full (with DR/BDR).
* DR/BDR needed on multiaccess to reduce flooding.
* **Hello sent to 224.0.0.5**.
* **RID preference**: Manual > Loopback IP > Physical IP.
* OSPFv3 = IPv6 version, separate process.
* **Max \~50 routers per area recommended**.
* **ABR (Area Border Router)**: connects multiple OSPF areas.

---

## 📘 Additional Notes for NetAcad Questions

* **OSPF Databases**: Adjacency = Neighbor Table, LSDB = Topology Table, Forwarding = Routing Table.
* **SPF (Dijkstra)** computes route costs.
* **Link-State Operation order**: Neighbor adjacency → LSA exchange → Build LSDB → Run SPF → Best Route.
* **Packet Roles**: Hello = discover/maintain neighbors, DBD = LSDB summary, LSR = request info, LSU = announce updates, LSAck = confirm receipt.
* **Router ID** uniquely identifies routers in Hellos.
* **OSPF States**: Init (Hello received), Two-Way (DR/BDR election), ExStart (decide DBD sender), Exchange (DBDs sent), Loading (SPF calculation), Full (DBs synced). Down = no Hellos.

## 🏁 Final Word

OSPF is a powerful, scalable, and flexible routing protocol. Mastering its databases, packet types, neighbor states, and DR/BDR logic is essential for network engineers. With single-area OSPF, all routers share the same LSDB, ensuring consistent routing decisions, while multiarea design offers scalability and efficiency for larger networks. Understanding these foundations will make OSPF configuration, troubleshooting, and optimization much easier in real-world networks.
