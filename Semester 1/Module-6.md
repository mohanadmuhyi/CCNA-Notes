# 📘 CCNA - Module 6: Data Link Layer

## 🎯 Objective

Explain how the Data Link Layer enables communication across various types of physical media by managing MAC addressing, media access control, and framing.

---

## 6.1 🔌 Purpose of the Data Link Layer

### 📦 Responsibilities:

* Ensures reliable NIC-to-NIC communication between devices on the same network segment.
* Encapsulates Layer 3 packets (IPv4/IPv6) into **Layer 2 frames**.
* Adds addressing and error detection info.
* Detects errors and discards corrupted frames.

### �\uddna Two Sublayers (IEEE 802 Standards):

1. **LLC (Logical Link Control):**

   * Interfaces with Layer 3 (network layer)
   * Passes data to MAC sublayer
2. **MAC (Media Access Control):**

   * Controls how devices access media
   * Adds source/destination MAC addresses
   * Performs **framing** and **error checking**
   * Provides method to place frames on and off the media

### 🛠 How Frames Are Handled at Routers:

Each router hop performs:

1. Accepts a frame from incoming link
2. **De-encapsulates** the frame (extracts packet)
3. **Re-encapsulates** the packet into a new frame with new MAC addresses
4. Forwards it to the next hop

> ✅ Note: **IP stays constant** across hops, but **MAC addresses change** at each hop.

> 🧠 Each NIC (Network Interface Card) has a unique **physical MAC address**, burned into its hardware by the manufacturer.

---

## 6.2 🕸 Topologies

### 📡 Physical vs Logical Topology

* **Physical topology:** Physical layout of devices and cables (e.g. star, bus, ring)
* **Logical topology:** How data flows through the network (defined by addressing, not cables)

### 🏓 Common Topologies

* **Star:** Devices connect to a central switch/hub
* **Bus (Legacy):** Devices connected along a single backbone; collisions common
* **Ring (Legacy):** Devices connected in a circular path

```ascii
[Bus] A ---- B ---- C ---- D

[Ring] A → B → C → D → A (loop)
```

> 🧠 Bus and Ring are **legacy topologies**, rarely used today due to collision and redundancy issues.

### 🚁 WAN Topologies

* **Point-to-Point:** Direct connection between two routers
* **Hub-and-Spoke:** Central site connects to remote sites (like star)
* **Mesh:** Every node connected to every other node (high redundancy)

---

## 🔀 Duplex Modes

* **Half-duplex:** Send OR receive at a time (used in hubs, legacy bus, wireless)
* **Full-duplex:** Simultaneous send/receive (used in switches)

> 📌 **Collision Domain Explained:**
> A collision domain is a network segment where devices share the same medium, and data packets can collide.
>
> * **Hubs** create **one big collision domain** — everyone talks on the same wire.
> * **Switches** create **separate collision domains per port** — reducing collisions.

> Example:
>
> * 3 PCs on a hub = 1 collision domain
> * 3 PCs on a switch = 3 separate collision domains

---

## 6.3 🚦 Access Control Methods

### 🥊 Contention-Based (Competitive)

* **Multiple devices** compete for use of the medium
* Used in **half-duplex** environments

#### 🔀 CSMA/CD (Collision Detection)

* Used in **wired legacy Ethernet** (bus topology)
* Devices listen before sending
* If collision occurs:

  * Send **jamming signal**
  * Wait random time (backoff)
  * Retry

#### 📡 CSMA/CA (Collision Avoidance)

* Used in **wireless (802.11)**
* Devices share how long they'll transmit → Others wait
* Prevents rather than detects collisions

> 🔎 CSMA/CD was necessary in **legacy shared Ethernet environments**, such as **hubs** and **bus topologies**, where devices could collide on a single wire.

> 🔎 CSMA/CA is essential in **wireless** because you can't detect collisions over air like in cables. So devices wait before sending.

### �� Controlled Access

* Each device is assigned a turn (used in legacy systems like Token Ring)

---

## 6.4 �\uddna Data Link Frame

### 📦 Frame = Header + Data + Trailer

Each frame includes:

* **Header:**

  * **Destination MAC address**
  * **Source MAC address**
  * **Type/Length field** (e.g., IPv4, ARP)
* **Data:**

  * Payload from Layer 3 (e.g. IP packet)
* **Trailer:**

  * **FCS (Frame Check Sequence)** — detects errors via CRC

> ✅ FCS helps detect errors in transmission. If corrupted, the frame is discarded.

### 🏠 Addressing (Layer 2 / MAC):

* **MAC = physical address** (burned into NIC)
* Only used for **local delivery** (one link at a time)
* **Updated** at every hop by the router

### 💡 Example:

| Hop | Source MAC | Destination MAC |
| --- | ---------- | --------------- |
| 1   | PC         | Router 1        |
| 2   | Router 1   | Router 2        |
| 3   | Router 2   | Final Device    |

> IP address remains the same; MAC is changed per segment.

> ⌛ **MAC table entries are temporary**. Switches delete them after a timeout if the device is no longer active or reachable.

---

## 🔧 Switch vs Hub

| Feature     | Switch                   | Hub                      |
| ----------- | ------------------------ | ------------------------ |
| Forwarding  | Based on MAC address     | Broadcasts to all ports  |
| Collision   | Fewer domains (per port) | One big collision domain |
| Duplex Mode | Full-duplex supported    | Half-duplex only         |
| Learning    | Learns MAC addresses     | Doesn't learn MACs       |

### 🧠 How Switch Learns:

* Examines **source MAC** in incoming frames
* Stores it in **MAC Address Table** with port
* If destination MAC unknown → floods the frame

---

## 🔄 ARP (Address Resolution Protocol)

### 🔎 Purpose:

Resolves **IPv4 addresses to MAC addresses** for local delivery

### 🛳 ARP Request:

* Sent as **broadcast** asking "Who has this IP?"
* Only the target device replies with its MAC

### 📩 ARP Reply:

* Sent as a **unicast** back to requester

> ❗ ARP works only within the same LAN (broadcast domain)

> 📌 **Broadcast Domain Explained:**
> A broadcast domain includes all devices that can receive a broadcast sent by one of them.
>
> * **Routers break broadcast domains**
> * **Switches forward broadcasts** to all ports (except the one it came from)

> Example:
>
> * A broadcast from PC1 will reach PC2 and PC3 in the same VLAN/switch
> * It **won’t reach** devices on the other side of a router

> 🧠 ARP is crucial for mapping IP to MAC within a LAN before frames can be delivered locally. It’s a **foundational step** in communication.

---

## 📜 Additional Notes for NetAcad Questions

* The **last field in a frame (trailer)** is the **FCS** used to **detect transmission errors**, not for QoS or identifying protocols.
* **MAC addresses appear first** in the frame: `Destination MAC → Source MAC`, followed by Layer 3 headers.
* Data link layer protocols include: **Ethernet, PPP, and 802.11 (Wi-Fi)**.
* The **MAC sublayer** provides the method to place frames onto the media — it doesn't communicate with Layer 3.
* Routers **do perform Layer 2 functions** like accepting frames, de-encapsulating, and re-encapsulating.
* **Media access control methods** depend on **media sharing** and **topology**.
* **IEEE** defines standards for the **physical and data link layers**, not IETF or IANA.
* **Logical topologies** display how data flows — including Layer 3 addresses like IPs.
* WLANs (Wi-Fi) use **half-duplex communication** and **CSMA/CA** as the media access method.

---

## 💡 Final Word

The Data Link Layer is the bridge between raw physical signals and logical data packets. It ensures devices on the same network can talk reliably, with proper addressing, frame structure, and error control. By mastering Layer 2, you’ll understand how switches, MAC addresses, and frames power every click and ping across the LAN. This knowledge is foundational for both networking and cybersecurity roles.

---
