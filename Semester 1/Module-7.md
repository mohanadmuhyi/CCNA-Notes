# 📘 CCNA - Module 7: Ethernet Switching

## 🌟 Objective

Explain how Ethernet works in a switched network, including encapsulation, MAC addressing, switching decisions, and forwarding methods.

---

## 7.1 🚗 Ethernet Frames

### 🔹 Ethernet Encapsulation

* Ethernet operates at the **Data Link (Layer 2)** and **Physical (Layer 1)** layers.
* Defined by IEEE 802.3 (MAC) and 802.2 (LLC).

### 🏑 Data Link Sublayers

* **LLC (802.2)**: Identifies the network layer protocol (e.g., IPv4 or IPv6)
* **MAC (802.3)**:

  * Adds source/destination MAC addresses
  * Performs encapsulation and media access

### 🔹 MAC Sublayer Responsibilities

1. **Data Encapsulation:**

   * Ethernet frame includes: Source & Destination MAC, EtherType (protocol indicator), Payload (Layer 3 packet), and FCS
2. **Media Access Control:**

   * Controls access to the shared medium using CSMA/CD in legacy networks

### 🔹 Ethernet Frame Format

| Field                       | Description                        |
| --------------------------- | ---------------------------------- |
| Preamble                    | Synchronization bits (7 bytes)     |
| Start Frame Delimiter (SFD) | Marks start of frame (1 byte)      |
| Destination MAC             | 6 bytes                            |
| Source MAC                  | 6 bytes                            |
| EtherType                   | 2 bytes (e.g. 0x0800 for IPv4)     |
| Data                        | Payload (46 to 1500 bytes)         |
| Frame Check Sequence (FCS)  | 4 bytes, CRC-based error detection |

### 🔎 Frame Size

* **Minimum:** 64 bytes
* **Maximum:** 1518 bytes (without preamble)
* **Runt Frames:** <64 bytes (invalid, usually from collision or transmission error)
* **Jumbo Frames:** >1500 bytes (used in specialized networks like data centers; must be supported by devices)

> 🔗 **Frame Size Overview:**
>
> * **Segment (TCP/UDP):** Typically 1460 bytes of data (due to MTU limits)
> * **Packet (IP):** 20 byte header + data (fits in MTU of 1500 bytes)
> * **Frame (Ethernet):** Ethernet adds 18 bytes → max total size \~1518 bytes

> 🔍 **Header Sizes Recap:**
>
> * TCP Header: **20 bytes**
> * IPv4 Header: **20 bytes**
> * Ethernet Header: **18 bytes** (14 header + 4 FCS trailer)

---

## 7.2 📲 Ethernet MAC Address

### 🔹 MAC Address Types

* **Unicast Frame:** Sent from one device to a single specific device (one-to-one).
* **Broadcast Frame:** Sent to all devices on the LAN; uses destination MAC: `FF:FF:FF:FF:FF:FF`.
* **Multicast Frame:** Sent to a group of devices; destination MAC starts with `01:00:5E` (IPv4) or `33:33` (IPv6).

🔎 Example:

* A DHCP Discover message is broadcast.
* A video stream could use multicast.
* File transfer (like FTP) typically uses unicast.

### 🔹 MAC Address Format

* 48-bit address written as 12 hexadecimal digits (e.g., 00:1A:2B:3C:4D:5E)
* Consists of:

  * **OUI (Organizationally Unique Identifier)** – First 3 bytes (assigned to vendors)
  * **Device ID** – Last 3 bytes (assigned by vendor)
* A company can have **multiple OUIs** for large-scale production or different device lines

### 🔹 Types of MAC Addresses

| Type      | Description                                             |
| --------- | ------------------------------------------------------- |
| Unicast   | One-to-one communication                                |
| Broadcast | FF\:FF\:FF\:FF\:FF\:FF – sent to all devices in the LAN |
| Multicast | Starts with 01-00-5E (IPv4), 33-33 (IPv6)               |

### 🔹 Frame Handling

* Destination MAC checked by receiving NIC
* If MAC doesn't match or isn't broadcast/multicast → frame is dropped
* Matching NIC passes it up the OSI stack for de-encapsulation

---

## 7.3 📊 MAC Address Table

### 🔹 Learning

* Switch learns source MAC from incoming frames
* Stores it with associated port
* MAC address entries are **temporary** (default: 5 mins timeout)

### 🔹 Forwarding

* If destination MAC is in table → forwards out the correct port
* If unknown → **floods** frame to all ports (unknown unicast)
* Broadcast/multicast → forwarded to all ports except incoming one

### 🔹 Filtering

* Once table has MAC, future frames are only sent to that port

> 📅 This table is sometimes called the **CAM (Content Addressable Memory) table**

---

## 7.4 ⚖️ Switch Forwarding Methods

### 🔹 Store-and-Forward

* Receives entire frame, performs **CRC check**, then forwards
* Reliable but introduces latency
* Required for **QoS and error checking**

### 🔹 Cut-Through Switching

* Starts forwarding after reading destination MAC
* Low latency, no error checking

#### 🔊 Variants:

* **Fast-Forward:** Forwards immediately (lowest latency)
* **Fragment-Free:** Checks first 64 bytes for errors, then forwards (middle ground)

> ❗ Fragment-Free combines benefits of both methods: avoids forwarding corrupted frames (most collisions happen in first 64 bytes)

### 🌀 Memory Buffering

| Method        | Description                                                    |
| ------------- | -------------------------------------------------------------- |
| Port-based    | Separate memory per port; blocking on one port delays others   |
| Shared Memory | Common pool; dynamic allocation, better for asymmetric traffic |

---

## 🔢 Port Configuration

### 🔹 Duplex Modes

| Mode        | Description                                              |
| ----------- | -------------------------------------------------------- |
| Half-duplex | Send OR receive, not both (used in hubs, legacy devices) |
| Full-duplex | Simultaneous send/receive (standard today)               |

* Mismatched duplex → collisions, performance drops

### 🔹 Autonegotiation

* Switches and NICs negotiate speed and duplex automatically
* Best practice: ensure both sides match manually or enable auto on both
* **Gigabit ports require full-duplex**

### 🔹 Auto-MDIX

* Detects and adjusts to straight-through or crossover cables
* Enabled by default in most modern switches

---

## 📚 Additional Notes for NetAcad Questions

### 🧩 Ethernet Frame Details

* **EtherType**: Indicates the Layer 3 protocol (e.g., IPv4, IPv6).
* **Preamble**: Series of alternating 1s and 0s used to synchronize communication.
* **Start Frame Delimiter (SFD)**: Signals the actual start of the frame.
* **Frame Check Sequence (FCS)**: CRC-based error detection at the end of the frame.
* **Data Field**: Can include padding if the total frame is less than 64 bytes.

### 🧠 Data Link Sublayer Responsibilities

* **MAC (Media Access Control)**:

  * Controls access to the media using CSMA/CD or CSMA/CA.
  * Provides physical addressing (MAC) and checks for errors in received bits.
  * Operates with software drivers to control NIC access.
* **LLC (Logical Link Control)**:

  * Interacts with Layer 3.
  * Allows multiple protocols to use the same interface (e.g., IPv4 and IPv6).

### 🔁 Switching Methods & Buffering

* **Store-and-Forward** and **Cut-Through** are the two primary switching methods.
* **Cut-Through** includes:

  * **Fast-Forward** (no error checking)
  * **Fragment-Free** (checks first 64 bytes only)
* **Buffering Types**:

  * **Port-based**: Each port has isolated memory.
  * **Shared memory**: All ports pull from a shared pool for efficiency.

### 🔧 Auto Features

* **Autonegotiation**: Auto-selects best speed/duplex mode.
* **Auto-MDIX**: Allows use of any cable type (cross/straight).

### Topology & Duplex

* WLAN uses **half-duplex** communication.
* Legacy Ethernet used **CSMA/CD**.
* Logical topology displays IP/device relationships; physical shows real layout.

---


## 📊 Final Word

Understanding how Ethernet switching operates is key to mastering modern networking. Ethernet frames, MAC addressing, switching decisions, and port configurations all play essential roles in efficient data delivery. Grasping these Layer 2 concepts lays a strong foundation for troubleshooting and optimizing networks.

> 🎯 **Key Takeaway:** A switch is not just a dumb connector—it learns, decides, and optimizes traffic flow based on MAC addresses and forwarding logic.

