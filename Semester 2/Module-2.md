# CCNA SRWE – Semester 2

## 📘 Module 2: Switching Concepts



### 🎯 Module Objectives

Explain how **Layer 2 switches forward data**.

---

## 📦 2.1 Frame Forwarding

### 🔹 Switching Basics

* **Ingress** = frame entering an interface
* **Egress** = frame exiting an interface
* Switch forwards frames based on:

  * **Ingress port**
  * **Destination MAC address**
* Switch uses its **MAC Address Table (CAM Table)** to decide where to forward.
* ❌ Switch never forwards traffic out the same port it came from.

---

### 🔹 Switch MAC Address Table

* Records **Source MAC** + the **port it was received on**.
* Used to determine where to send traffic.

---

### 🔹 Switch Learn-and-Forward Method

1. **Learn** (Source MAC):

   * Adds new source MAC to table with port number.
   * If already in table → refresh timeout (default \~5 mins).

2. **Forward** (Destination MAC):

   * If in table → forward out that port.
   * If not in table → **floods** out all ports except ingress.

---

### 🔹 Switch Forwarding Methods

Switches rely on ASICs (Application-Specific Integrated Circuits) for speed.
Two forwarding methods:

1. **Store-and-Forward (Cisco preferred)**

   * Receives the entire frame.
   * ✅ Checks **FCS** (CRC error check).
   * ✅ Can buffer (handle speed mismatches between ports).
   * ❌ Adds slight delay.

2. **Cut-Through**

   * Forwards immediately after reading Destination MAC.
   * **Fragment-Free** (variant): checks first 64 bytes (eliminates runts).
   * ✅ Very low latency (<10 μs).
   * ❌ No FCS check (errors can propagate).
   * ❌ Cannot handle ports with different speeds.
   * ❌ May increase bandwidth issues if too many bad frames get forwarded.

---

## 🌐 2.2 Switching Domains

### 🔹 Collision Domains

* **Switches eliminate collisions** when operating in **full-duplex**.
* **Half-duplex** links = collisions can occur (contention for bandwidth).
* Auto-negotiation (duplex/speed) is the default on Cisco/Microsoft devices.
* A **hub** represents **one big collision domain** shared by all devices connected to it.
* A **switch** creates a **separate collision domain per port** (each link is isolated).

---

### 🔹 Broadcast Domains

* A **broadcast domain** = all devices that receive a broadcast frame.
* Switch forwards broadcast to all ports except ingress.
* ❌ Broadcast domains are **not broken by Layer 2 switches**.
* ✅ Only a **Layer 3 device (router)** breaks broadcast domains.
* Too many broadcasts = congestion & poor performance.
* Adding devices at L1 or L2 = expands broadcast domain.
* Two switches connected directly = **one single broadcast domain**.
* Two switches separated by a router = **two different broadcast domains**.

💡 **Note:** If there are two LANs separated by a router, a PC in LAN1 cannot directly obtain an IP address from a DHCP server in LAN2, because the router will not forward the broadcast. To make this work, a feature called **DHCP Relay** can be configured.

---

### 🔹 Features that Reduce Congestion

Switches help reduce congestion using:

| Feature                        | Function                                                  |
| ------------------------------ | --------------------------------------------------------- |
| **⚡ Fast Port Speeds**         | Up to 100 Gbps depending on model                         |
| **🚀 Fast Internal Switching** | High-speed bus or shared memory                           |
| **📂 Large Frame Buffers**     | Stores large quantities of frames temporarily             |
| **🔌 High Port Density**       | Many ports at lower cost, more local traffic stays in LAN |

💡 **Note:** Buffering can be implemented using:

* **Port-Based Buffers** → Frames are stored in queues linked to specific incoming/outgoing ports.
* **Shared Memory Buffers** → A common buffer pool dynamically allocated to ports as needed, improving efficiency when traffic is unbalanced.

---

## 📝 2.3 Summary

### ✅ Key Points

* **Ingress = entry, Egress = exit**.
* Switch builds a **MAC Address Table** for forwarding.
* Forwarding methods:

  * **Store-and-Forward** (error check, buffering, reliable).
  * **Cut-Through** (faster, less reliable).
* **Collision Domains**:

  * Hubs = one collision domain shared by all devices.
  * Switch = one collision domain per link.
  * Half-duplex = collisions possible.
  * Full-duplex = no collisions.
* **Broadcasts**:

  * Switch floods broadcast or unknown unicast.
  * Two switches together = one broadcast domain.
  * Two switches separated by a router = two broadcast domains.
  * Only routers break broadcast domains.
  * DHCP Relay allows broadcasts (like DHCP requests) to cross routers.
* Switches help **eliminate collisions** and **reduce congestion** with features like fast ports, shared memory, and buffering strategies.

---

## 📚 Additional Notes for NetAcad Questions

* **Autonegotiation of Speed/Duplex**: When devices with different maximum speeds are connected, they will negotiate the highest speed supported by both. For example, a 1 Gbps NIC connected to a 100 Mbps switch port will settle on **100 Mbps**.
* **Broadcast Domains**: Only routers (Layer 3 devices) can break broadcast domains. Switches and hubs extend them.
* **Congestion Relief**: Switches alleviate congestion through **fast port speeds** and **fast internal switching** mechanisms (like shared memory or fast bus).

---

## 🔚 Final Word

Switches are powerful tools that increase network efficiency by eliminating collisions, reducing congestion, and managing frames intelligently. They isolate collision domains per port, but broadcast domains remain unless a router is used. By understanding **forwarding methods**, **MAC address learning**, and **buffering strategies**, you can design and troubleshoot switched networks with confidence.
