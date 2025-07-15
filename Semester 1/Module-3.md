# 📘 CCNA - Module 3: Protocols and Models

## 🎯 Objective
Understand how communication rules (protocols), models (OSI/TCP-IP), and encapsulation help devices send and receive data over networks — whether local or remote.

---

## 3.1 📜 The Rules of Communication

Networking isn't just about cables — devices must agree on *how* to talk.

### 📶 Every communication needs:
- A **source** (sender)
- A **destination** (receiver)
- A **channel** (medium to carry data)

### 🧩 Protocols define:
- Who can send/receive
- In what format
- At what speed
- With what kind of acknowledgment

---

## 🗣️ Types of Delivery

- **Unicast:** One-to-one communication  
- **Multicast:** One-to-many (selected recipients)  
- **Broadcast:** One-to-all (used in IPv4, not IPv6)

🔍 *Insight:* Unicast is used in web browsing, multicast in streaming (e.g. IPTV), and broadcast in ARP requests.

---

## 3.2 🔐 Protocols

Protocols are the language of networking. They can be in:
- Hardware (Ethernet)
- Software (TCP, HTTP)
- Or both

### 🌐 Common Protocols and Functions

| Protocol  | Purpose                               |
|-----------|----------------------------------------|
| HTTP      | Web communication                      |
| HTTPS     | HTTP with encryption (TLS/SSL)         |
| TCP       | Reliable, ordered delivery             |
| IP        | Packet routing                         |
| Ethernet  | Frame delivery on LAN (MAC-based)      |

### 📌 Extra: HTTP vs HTTPS
- **HTTP:** Data in plain text  
- **HTTPS:** Encrypts data using **TLS** for privacy and integrity

---

### 🧠 Protocol Functions (More Examples)

| Function        | What it Does                              |
|----------------|--------------------------------------------|
| Addressing      | Identifies sender/receiver (IP/MAC)       |
| Sequencing      | Keeps track of data order                 |
| Error Detection | Detects corruption during transmission    |
| Flow Control    | Prevents flooding slow devices            |
| Reliability     | TCP guarantees successful delivery        |

---

## 3.3 💼 Protocol Suites

A **protocol suite** = group of protocols that work together to ensure full communication.

- Most used: **TCP/IP** (internet standard)
- Other/older: OSI model, AppleTalk, Novell NetWare

Each layer serves the one above and depends on the one below.

---

## 3.4 🧪 Standards Organizations

| Org     | Role                                                   |
|---------|--------------------------------------------------------|
| IETF    | Maintains TCP/IP standards                             |
| IEEE    | Defines Ethernet/Wi-Fi protocols (e.g. 802.3, 802.11)  |
| ICANN   | Manages domains and IP allocations                     |
| ISO/ITU | International standards (OSI, DSL, etc.)               |
| EIA/TIA | Hardware specs: cables, connectors, racks              |

---

## 3.5 🧱 Reference Models

### 🧭 Why Use Models?
- Simplifies learning and troubleshooting
- Encourages vendor interoperability
- Allows changes in one layer without affecting others

---

### OSI Model (7 Layers)

    ┌───────────────┐
    │ 7. Application│ ← Apps (e.g. browser, email)
    │ 6. Presentation│ ← Encoding, encryption
    │ 5. Session     │ ← Dialog control
    │ 4. Transport   │ ← TCP/UDP (ports, reliability)
    │ 3. Network     │ ← IP addressing & routing
    │ 2. Data Link   │ ← MAC addresses, frames
    │ 1. Physical    │ ← Bits over wires
    └───────────────┘

### TCP/IP Model (4 Layers)

    ┌───────────────────────┐
    │ Application (7–5 OSI) │
    │ Transport (4 OSI)     │ ← TCP, UDP
    │ Internet (3 OSI)      │ ← IP
    │ Network Access (2–1)  │ ← Ethernet, MAC
    └───────────────────────┘

🔍 *Extra Insight:* OSI is conceptual; TCP/IP is what we actually use.

---

## 3.6 📦 Data Encapsulation

As data moves down the stack, it's wrapped at each layer (encapsulation).  
Each layer creates a **PDU** (Protocol Data Unit):

| Layer      | PDU Name      |
|------------|---------------|
| Application/Transport | Data / Segment |
| Network     | Packet        |
| Data Link   | Frame         |
| Physical    | Bits          |

### 🔁 On Receiving Side:
Each layer removes its header/trailer — this is called **de-encapsulation**.

---

### 🔀 Multiplexing

- Allows multiple communications to share the same channel.
- Like watching multiple channels on one cable.

---

### 📋 PDU Summary

    Application → Data  
    Transport   → Segment  
    Network     → Packet  
    Data Link   → Frame  
    Physical    → Bits  

---

## 3.7 🔄 Data Access and Addressing

### 📬 IP vs MAC (Simply)

| Type  | Layer | Purpose                         |
|-------|-------|---------------------------------|
| IP    | Layer 3 (Network) | Global addressing     |
| MAC   | Layer 2 (Data Link) | Local device ID     |

🧠 Both are required: IP finds the destination, MAC delivers it on the LAN.

---

### 🔁 How MAC Changes at Each Hop

MAC addresses change at each router hop, IP stays the same.

Example:  
From PC → Web Server (3 hops)

| Hop | Source MAC | Destination MAC     |
|-----|------------|----------------------|
| 1   | PC NIC     | Router 1             |
| 2   | Router 1   | Router 2             |
| 3   | Router 2   | Web Server NIC       |

✅ IP stays constant: `192.168.1.110 → 172.16.1.20`

---

### 🗂️ MAC vs Routing Tables

| Table Type    | Used By | Stores                           |
|---------------|---------|----------------------------------|
| MAC Table     | Switch  | MAC → Port mappings              |
| Routing Table | Router  | IP Network → Next Hop mappings   |

---

## 🔁 Bonus: CSMA/CD & CSMA/CA

### 🔌 CSMA/CD (Collision Detection)
- Used in **wired Ethernet** (bus topology)
- Detects collision → sends jamming signal → retries
- Now considered legacy

### 📡 CSMA/CA (Collision Avoidance)
- Used in **wireless**
- Tries to avoid collisions by checking if the medium is free before sending

---

## 🌐 DNS – Domain Name System

- Translates domain names to IP addresses  
  (e.g., `google.com → 142.250.185.206`)
- Makes browsing user-friendly and works like a phonebook

---

## 🧱 Layer Summary

### Layer 2 – Data Link
- Uses MAC addresses
- Delivers frames on local LAN
- Switches work here

### Layer 3 – Network
- Uses IP addresses
- Routes packets across networks
- Routers work here

### Layer 4 – Transport
- Uses ports to identify services
- Ensures delivery (TCP) or fast (UDP)

---

## 🛰️ OSPF vs BGP (Simply)

| Protocol | Type      | Use Case                         |
|----------|-----------|----------------------------------|
| OSPF     | Internal  | Used within an organization      |
| BGP      | External  | Used between ISPs and big orgs   |

🧠 *Tip:* OSPF = fast inside, BGP = routes the internet.

---



### 💡 Final Word

This module is your networking grammar.  
Mastering it gives you the power to understand, design, troubleshoot, and secure any digital conversation — the foundation of every network job ahead.
