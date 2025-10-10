# CCNA ENSA - Semester 3

## 🧩 Module 9: QoS Concepts

### 🎯 Objectives

Explain how networking devices implement **Quality of Service (QoS)** to ensure reliable transmission of voice, video, and data traffic across the network.

---

## 9.1 Network Transmission Quality

### 🌐 What is QoS?

QoS (Quality of Service) refers to mechanisms that manage network resources to ensure certain levels of performance for specific types of traffic (voice, video, data).

QoS becomes active **only during congestion** — when the traffic volume exceeds available bandwidth.

---

### 🚦 Prioritizing Traffic

* When congestion occurs, devices **queue packets** in memory until they can be transmitted.
* **Delays** occur because new packets must wait in queue.
* If buffers fill up → packets are **dropped**.
* **QoS classifies** data into multiple queues to ensure important traffic (like voice) is prioritized.

---

### 📈 Bandwidth, Congestion, Delay, and Jitter

* **Bandwidth:** amount of data transmitted per second (bps).
* **Congestion:** occurs when more traffic arrives than an interface can handle.

  * Typical congestion points:

    * **Aggregation** (multiple links merging)
    * **Speed mismatch** (fast → slow link)
    * **LAN-to-WAN** transitions

#### 🔁 Types of Delay (Latency)

| Delay Type          | Description                                        |
| ------------------- | -------------------------------------------------- |
| Code delay          | Time to compress data at source                    |
| Packetization delay | Time to encapsulate packet with headers            |
| Queuing delay       | Time waiting in the output queue (variable)        |
| Serialization delay | Time to place bits on the wire                     |
| Propagation delay   | Time for signal to travel across the medium        |
| De-jitter delay     | Time to reorder/buffer uneven packets for playback |

* **Jitter:** Variation in delay of received packets.
* **High jitter** causes uneven playback (e.g., choppy voice).

---

### 📉 Packet Loss

* Without QoS, **all traffic is treated equally**, even time-sensitive voice/video.
* **VoIP** uses a **playout buffer** to smooth jitter.
* If jitter exceeds buffer capacity, packets are dropped → **audio dropouts**.
* **DSPs** (Digital Signal Processors) can interpolate small losses but not severe jitter.
* In well-designed networks, packet loss ≈ **0%**.

---

## 9.2 Traffic Characteristics

### 📊 Network Traffic Trends

* Early 2000s: mostly **voice and data**.
* Today: **video dominates** (>80% of all IP traffic).
* Each traffic type has **different QoS needs**.

---

### 🔊 Voice Traffic

* Predictable and smooth.
* **Highly sensitive** to delay and packet loss.
* Must have **highest priority**.

| Requirement | Value                       |
| ----------- | --------------------------- |
| Latency     | < 150 ms                    |
| Jitter      | < 30 ms                     |
| Packet loss | < 1%                        |
| Bandwidth   | 30–128 Kbps                 |
| Protocol    | UDP (RTP ports 16384–32767) |

Voice = **Smooth, benign, delay-sensitive, drop-sensitive, UDP priority**

---

### 🎥 Video Traffic

* **Bursty**, inconsistent, and consumes large bandwidth.
* More sensitive to loss than voice.

| Requirement | Value                |
| ----------- | -------------------- |
| Latency     | < 200–400 ms         |
| Jitter      | < 30–50 ms           |
| Packet loss | < 0.1–1%             |
| Bandwidth   | 384 Kbps – 20 Mbps   |
| Protocol    | UDP (RSTP, port 554) |

Video = **Bursty, delay-sensitive, drop-sensitive, greedy for bandwidth, UDP priority**

---

### 💾 Data Traffic

* Data traffic covers a wide range of applications — file transfers, emails, web browsing, system updates, and backups.
* Uses **TCP**, which ensures reliability through **acknowledgments and retransmissions**.
* **Insensitive** to small delays or packet drops, but excessive delay can affect **interactive data** (e.g., SSH or database queries).
* **Mission-critical applications** (banking, ERP systems) may still require some prioritization.
* Large downloads like FTP or backups can cause congestion if not managed.
* QoS ensures that **interactive or important data** gets priority over bulk transfers.

| Type                             | Examples              | QoS Treatment                            |
| -------------------------------- | --------------------- | ---------------------------------------- |
| Mission-Critical Interactive     | Database queries, SSH | Prioritize for low delay (1–2s response) |
| Mission-Critical Non-Interactive | Backups, updates      | Ensure minimum bandwidth                 |
| Non-Critical                     | Email, downloads      | Lowest priority, use remaining bandwidth |

---

## 9.3 Queuing Algorithms

### 🤖 Queuing Overview

Queuing = **Congestion Management Tool**

* Buffers, prioritizes, or reorders packets when congestion occurs.
* Main algorithms:

  * **FIFO** (First-In, First-Out)
  * **WFQ** (Weighted Fair Queuing)
  * **CBWFQ** (Class-Based Weighted Fair Queuing)
  * **LLQ** (Low Latency Queuing)

---

### 1️⃣ FIFO (First-In, First-Out)

* Default queuing mechanism (used when no QoS is configured).
* **No traffic differentiation** — all packets treated equally.
* Simple but can lead to high delay for critical packets during congestion.
* Good for low-load or simple networks where fairness isn’t a concern.

---

### 2️⃣ WFQ (Weighted Fair Queuing)

* Dynamically divides bandwidth fairly among all flows.
* Automatically classifies traffic into flows (based on IP, MAC, protocol, port, ToS).
* Each flow gets a “weight,” meaning higher-priority packets get slightly more bandwidth.
* Prevents a single large flow (like FTP) from starving smaller, interactive flows.
* Not suitable for encrypted or tunneled packets (classification data hidden).

---

### 3️⃣ CBWFQ (Class-Based Weighted Fair Queuing)

* Extends WFQ by allowing **administrator-defined traffic classes** using ACLs or protocols.
* Each class has its own **dedicated FIFO queue**.
* Admin can specify:

  * Guaranteed **bandwidth** percentage
  * **Weight** for scheduling
  * **Queue limit** (maximum packets allowed)
* Once the limit is reached, new packets are dropped (**Tail Drop**).
* Ensures fair distribution even under heavy load but without strict priority.

---

### 4️⃣ LLQ (Low Latency Queuing)

* Combines CBWFQ with **Priority Queuing (PQ)**.
* Provides an **absolute priority queue** for delay-sensitive traffic (e.g., VoIP).
* Voice packets bypass all other queues.
* Prevents jitter and delay but must be limited — excessive use can starve other traffic.
* Cisco recommends reserving LLQ strictly for **voice and critical real-time** flows.

---

## 9.4 QoS Models

### 🧱 Three QoS Models

| Model                                  | Description                                         | Use Case                                                 |
| -------------------------------------- | --------------------------------------------------- | -------------------------------------------------------- |
| **Best-Effort**                        | No QoS configuration; packets treated equally       | Small/simple networks                                    |
| **IntServ (Integrated Services)**      | Uses **RSVP** to reserve bandwidth for each session | Real-time or sensitive applications (video conferencing) |
| **DiffServ (Differentiated Services)** | Uses class-based traffic marking and policies       | Scalable enterprise/backbone environments                |

---

### 🤍 Best-Effort Model

* Default Internet model — packets are sent whenever resources are available.
* **No guarantees**, no classification, no prioritization.
* Suitable for general web traffic, file transfers, or when QoS isn’t critical.

**Advantages:** simple, easy to deploy, scalable.
**Disadvantages:** no performance guarantee, packets may be delayed or dropped during congestion.

---

### 👑 Integrated Services (IntServ)

* Provides **end-to-end QoS guarantees** using **Resource Reservation Protocol (RSVP)**.
* Every application flow reserves the resources it needs (bandwidth, delay, jitter).
* Works like when **a president passes on a road** — all traffic stops, and the path is dedicated solely to that session.
* Uses **admission control** to ensure the network can handle the request.
* Highly reliable but consumes significant resources (each router maintains per-flow state).

**Advantages:** predictable, guaranteed service for critical applications.
**Disadvantages:** not scalable for large networks (stateful, signaling overhead).

---

### 🚑 Differentiated Services (DiffServ)

* Scalable QoS model that classifies packets into **traffic classes** based on business needs.
* Traffic is **marked** with DSCP values and treated accordingly at each hop (**hop-by-hop behavior**).
* Works like an **ambulance on a road** — gets priority over others but still shares the road.
* No explicit reservation; instead, routers apply pre-configured QoS policies to each class.
* Provides **relative priority** rather than guaranteed performance.

**Advantages:** scalable, flexible, less resource-intensive than IntServ.
**Disadvantages:** no strict guarantees — relies on consistent configuration across the network.

---

## 9.5 QoS Implementation Techniques

### 🚧 Avoiding Packet Loss

* **Causes:** congestion and buffer overflow.
* **Fixes:**

  1. Increase link capacity.
  2. Guarantee bandwidth (WFQ, CBWFQ, LLQ).
  3. Drop low-priority packets early using **WRED** (Weighted Random Early Detection).

---

### ⚙️ QoS Tool Categories

| Tool Type                    | Function                                 |
| ---------------------------- | ---------------------------------------- |
| **Classification & Marking** | Identify and label packets               |
| **Congestion Avoidance**     | Anticipate congestion (WRED)             |
| **Congestion Management**    | Queue, schedule, or reorder (CBWFQ, LLQ) |

---

### 🤖 Classification & Marking

* Done using **interfaces**, **ACLs**, or **NBAR (Layers 4–7)**.
* Marking can be done at **Layer 2 or Layer 3**:

  * L2 = CoS (802.1p)
  * L3 = DSCP or IP Precedence

| Protocol          | Field | Bits | Description         |
| ----------------- | ----- | ---- | ------------------- |
| Ethernet (802.1p) | CoS   | 3    | Class of Service    |
| Wi-Fi             | TID   | 3    | Wi-Fi Traffic ID    |
| MPLS              | EXP   | 3    | Experimental bits   |
| IPv4/IPv6         | DSCP  | 6    | DiffServ Code Point |

---

### 📶 Layer 2 Marking (CoS Values)

| CoS | Binary  | Traffic Type   |
| --- | ------- | -------------- |
| 0   | 000     | Best Effort    |
| 3   | 011     | Call Signaling |
| 4   | 100     | Video          |
| 5   | 101     | Voice          |
| 6–7 | 110–111 | Reserved       |

---

### 🌍 Layer 3 Marking (DSCP)

* IPv4 uses **ToS field**, IPv6 uses **Traffic Class field**.
* 6 bits for DSCP → 64 possible classes.
* 2 bits for **ECN (Explicit Congestion Notification)**.

#### DSCP Categories:

| Type                      | DSCP               | Description                       |
| ------------------------- | ------------------ | --------------------------------- |
| Best Effort               | 0                  | Default                           |
| Expedited Forwarding (EF) | 46 (binary 101110) | Voice                             |
| Assured Forwarding (AFxy) | Variable           | Defines queue + drop preference   |
| Class Selector (CSx)      | First 3 bits       | Compatible with CoS/IP Precedence |

---

### 🔒 Trust Boundaries

Mark traffic as close to the **source** as possible:

1. Trusted endpoints mark traffic themselves.
2. Secure switches can mark untrusted traffic.
3. Routers may remark traffic crossing trust boundaries.

---

### ❌ Congestion Avoidance (WRED)

* Drops packets **before** buffer overflows.
* Monitors queue depth:

  * Below min threshold → no drops
  * Near max → random drops
  * Above max → all dropped
* Prevents TCP global synchronization.

---

### 📤 Shaping vs Policing

| Mechanism    | Direction | Function                                                  |
| ------------ | --------- | --------------------------------------------------------- |
| **Shaping**  | Outbound  | Buffers excess packets and sends them later (smooth rate) |
| **Policing** | Inbound   | Drops or marks packets exceeding rate (ISP enforcement)   |

---

### 🦯 QoS Policy Guidelines

* Enable queuing on all devices.
* Classify and mark traffic **close to the source**.
* Apply shaping and policing **near the source**.

---

## ✅ Module Summary

* QoS improves reliability for **voice and video**.
* Types of delay: fixed and variable.
* Voice: smooth, delay-sensitive.
* Video: bursty, delay-sensitive.
* Data: delay-insensitive (TCP).
* Queuing manages congestion.
* Models: **Best Effort**, **IntServ**, **DiffServ**.
* Tools: **Classification, Marking, Queuing, WRED, Shaping, Policing.**

---
## 📘 Additional Notes for NetAcad Questions
* Propagation delay is the variable time a frame takes to reach its destination.
* Congestion causes packet loss when buffers overflow.
* Jitter is delay variation between packets.
* Data uses most bandwidth; video is bursty; voice is smooth but loss-sensitive.
* WFQ fairly distributes bandwidth, CBWFQ defines classes, LLQ prioritizes voice.
* IntServ reserves bandwidth; DiffServ is scalable using DSCP.
* Policing drops excess traffic; shaping buffers it; WRED prevents congestion early.
---
## 📝 Final Word 
QoS ensures critical traffic like voice and video stay clear and stable even when the network is busy.
