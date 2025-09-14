# CCNA ENSA - Semester 3
## 🌐 Module 7: WAN Concepts


### 🎯 Objectives

Explain how WAN access technologies can be used to satisfy business requirements.

---

## 7.1 📌 Purpose of WANs

### 🖥️ LANs vs. WANs

* **LAN (Local Area Network):**

  * Small geographic area (home, office, campus).
  * Owned & managed by an organization/home.
  * High bandwidth using Ethernet & Wi-Fi.
  * No recurring fees beyond infrastructure.
  * **Use case:** Connecting PCs, printers, servers locally.
* **WAN (Wide Area Network):**

  * Large geographic coverage (city, country, worldwide).
  * Owned & managed by providers (ISP, telco, satellite, cable).
  * Requires subscription fees.
  * Bandwidth ranges from low to very high.
  * WiMAX is designed for WAN (unlike Wi-Fi which is LAN only).
  * **Use case:** Connecting remote offices, users, or branches over large distances.

### 🔒 Private vs. Public WANs

* **Private WAN:**

  * Dedicated to one customer.
  * Guaranteed service level, consistent bandwidth, secure.
  * Example: MPLS leased service from a provider.
* **Public WAN:**

  * Shared over Internet (ISP-based).
  * Variable service & bandwidth, lower cost.
  * Example: Business broadband Internet.

### 🕸️ WAN Topologies

* **Point-to-Point:** One-to-one link between sites (transparent to customer, costly at scale).
* **Hub-and-Spoke:** Central hub router connects multiple spokes (simple, but hub is a single point of failure).
* **Dual-Homed:** Hub has two connections for redundancy/load balancing (costlier, requires complex configuration).
* **Full Mesh:** All sites connect to each other (highest redundancy, expensive).
* **Partial Mesh:** Some but not all sites interconnect (balance of cost & resiliency).

### 📡 Carrier Connections

* **Single Carrier:** One service provider + SLA.
* **Dual Carrier:** Two providers for redundancy + higher availability.

### 📈 Evolving Networks Example (SPAN Engineering)

* **Small Network:** LAN with DSL connection, wireless router, outsourced IT.
* **Campus Network (CAN):** Multiple floors/buildings, firewall, in-house IT staff.
* **Branch Network (MAN):** Interconnect city offices using dedicated lines.
* **Distributed Network:** Global WAN using VPNs (site-to-site & remote).

---

## 7.2 ⚙️ WAN Operations

### 📜 WAN Standards

* **Authorities:**

  * **TIA/EIA:** Telecommunications Industry Association / Electronic Industries Alliance.
  * **ISO:** International Organization for Standardization.
  * **IEEE:** Institute of Electrical and Electronics Engineers.

### 🧩 WANs in OSI Model

* **Layer 1 (Physical Services):**

  * **SDH (Synchronous Digital Hierarchy):** Multiplexing standard for long-distance fiber transmission.
  * **SONET (Synchronous Optical Networking):** North American standard equivalent to SDH.
  * **DWDM (Dense Wavelength Division Multiplexing):** Increases fiber capacity by multiplexing light wavelengths (most widely used today).
* **Layer 2 (Data Link Services):**

  * **Broadband (DSL, Cable):** Encapsulates user data over provider infrastructure.
  * **Wireless:** Mobile broadband, satellite links.
  * **Ethernet WAN (MetroE):** Uses Ethernet frames across metro fiber.
  * **MPLS (Multiprotocol Label Switching):** Adds labels for traffic engineering, QoS, VPNs.
  * **PPP (Point-to-Point Protocol):** Legacy protocol, authentication, link quality management.
  * **HDLC (High-Level Data Link Control):** Default encapsulation on Cisco serial interfaces.
  * **Frame Relay:** Legacy NBMA packet-switching.
  * **ATM (Asynchronous Transfer Mode):** Legacy, fixed 53-byte cells.

### 📖 Key Terminology

* **DTE (Data Terminal Equipment):** Connects LAN to WAN device (e.g., router).
* **DCE (Data Communications Equipment):** Communicates with provider (e.g., modem, CSU/DSU).
* **CPE (Customer Premises Equipment):** Equipment owned by customer at the edge (DTE + DCE).
* **POP (Point of Presence):** ISP entry point.
* **Demarcation Point (Demarc):** Boundary between customer and provider equipment.
* **Local Loop (Last Mile):** Physical connection from CPE to CO.
* **CO (Central Office):** Provider’s local switching facility.
* **Toll Network:** Provider’s long-distance backbone.
* **Backhaul:** Links from access nodes into provider’s core.
* **Backbone:** High-capacity interconnection of provider networks.

### 🖥️ WAN Devices

* **Voiceband Modem:** Converts digital data ↔ analog over PSTN (obsolete).
* **DSL/Cable Modem:** Broadband access, connects router to DSLAM/CMTS.
* **CSU/DSU (Channel Service Unit/Data Service Unit):** Interface for leased digital circuits.
* **Optical Converter:** Fiber ↔ copper, converts optical signals to electronic pulses.
* **Wireless Router/AP:** Connects to cellular or satellite WAN.
* **WAN Core Devices:** High-speed provider routers & L3 switches.

### 🔄 Communication Types

* **Serial:** Bits sequentially sent over single channel. Preferred in WAN due to synchronization issues in parallel.
* **Circuit-Switched:** Dedicated channel per session. Examples: PSTN (voice), ISDN.
* **Packet-Switched:** Data split into packets, shares paths dynamically. Examples: MetroE, MPLS, Frame Relay, ATM.

### 💡 Optical Standards

* **SDH/SONET:** Multiplex multiple streams (voice, data, video).
* **DWDM:** Expands fiber capacity with wavelength multiplexing (currently dominant).

---

## 7.3 🏛️ Traditional WAN Connectivity

### 📡 Leased Lines (Dedicated)

* Point-to-point dedicated circuit.
* **T-carrier (NA):** T1 = 1.544 Mbps, T3 = 43.7 Mbps.
* **E-carrier (EU):** E1 = 2.048 Mbps, E3 = 34.368 Mbps.

**Advantages:** Always available, high quality, suitable for VoIP/Video.
**Disadvantages:** Very expensive, fixed capacity (not flexible).

### ☎️ Circuit-Switched (Switched WAN)

* **PSTN Dial-up:** Voiceband modem, < 56 kbps, uses telephone lines.
* **ISDN (Integrated Services Digital Network):** Digital local loop, 45 Kbps → 2 Mbps.

### 📦 Packet-Switched (Switched WAN)

* **Frame Relay:** Layer 2 NBMA, PVCs identified by DLCI. Cost-effective, but legacy.
* **ATM:** Fixed-length 53-byte cells, supports voice/video/data. Legacy but introduced QoS.

---

## 7.4 🚀 Modern WAN Connectivity

* **Why shift from traditional?** Traditional WANs are costly, limited bandwidth, often retired.
* **Modern Options:**

  * **Dedicated Broadband:** Own or lease fiber (“dark fiber”).
  * **Metro Ethernet (Ethernet WAN):** High speed, cheaper, scalable.
  * **MPLS:** Provider-managed, QoS, redundancy, supports multi-protocol traffic.
  * **Internet-based Broadband:** Lower cost, less guaranteed, widely used.

### 🏙️ Ethernet WAN (MetroE, EoMPLS, VPLS)

* Fiber-based Ethernet delivery across metro areas.
* **Benefits:**

  * Cost-effective compared to leased lines.
  * Seamless integration with existing LAN Ethernet.
  * Flexible speeds (10 Mbps – Gbps).
  * High productivity due to easy scaling.

### 🔖 MPLS (Multiprotocol Label Switching)

* High-performance WAN routing technology.
* Labels determine packet forwarding instead of IP lookup.
* Supports IPv4, IPv6, Layer 2 VPNs.
* **Advantages:** Traffic engineering, QoS, scalability, redundancy, VPN support.
* **Devices in MPLS:**

  * **CE (Customer Edge):** Customer router.
  * **PE (Provider Edge):** Provider-facing router.
  * **P (Provider Core Router):** Internal backbone routers.

---

## 7.5 🌍 Internet-Based Connectivity

### 📶 Wired Options

* **DSL (Digital Subscriber Line):**

  * Uses telephone copper lines.
  * **ADSL/ADSL2+:** Asymmetrical, faster download.
  * **SDSL:** Symmetrical, same up/down.
  * **DSLAM (DSL Access Multiplexer):** Aggregates DSL signals at CO.
  * **PPPoE (PPP over Ethernet):** Used for authentication, IP assignment.
* **Cable:**

  * Uses coaxial + DOCSIS standard.
  * Shared bandwidth, can slow at peak hours.
  * CMTS (Cable Modem Termination System) at provider headend.
* **Fiber (FTTx):**

  * **FTTH (Home):** Fiber direct to residence.
  * **FTTB (Building):** Fiber to building edge.
  * **FTTN (Node):** Fiber to neighborhood node, last hop over copper/coax.
  * Highest bandwidth, most future-proof.

### 📡 Wireless Options

* **Municipal Wi-Fi:** City-provided Wi-Fi (LAN-scale coverage).
* **Cellular (3G/4G/5G, LTE):**

  * Uses mobile towers.
  * Speeds vary, coverage issues in rural areas.
* **Satellite Internet:**

  * Used in remote areas.
  * High latency, affected by weather.
* **WiMAX (IEEE 802.16):**

  * Broadband wireless with wide coverage (WAN scale).

### 🔐 VPNs (Virtual Private Networks)

* Encrypts communication over public Internet.
* **Site-to-Site:** Configured on routers, transparent to users.
* **Remote Access:** User-initiated, VPN client or browser HTTPS.
* **Benefits:** Cost saving (no leased lines), secure encryption, scalable, compatible with broadband.

### 🌐 ISP Connectivity Options

* **Single-Homed:** One ISP, simple, no redundancy.
* **Dual-Homed:** Two links to same ISP, redundancy/load balancing, risk if ISP fails.
* **Multihomed:** Two ISPs, expensive but resilient.
* **Dual-Multihomed:** Dual links to multiple ISPs, maximum redundancy.

### ⚖️ Broadband Comparison

* **Cable:** Shared bandwidth, variable speeds.
* **DSL:** Distance-sensitive, limited upload.
* **Fiber:** Fastest, reliable, future-proof.
* **Cellular:** Limited bandwidth, coverage issues.
* **Municipal Wi-Fi:** Rare, limited coverage.
* **Satellite:** High latency, expensive, weather-dependent.

---

## 7.6 📝 Summary

* WANs extend beyond LAN boundaries.
* WANs can be private (secure, guaranteed) or public (shared Internet).
* WAN topologies: Point-to-Point, Hub-and-Spoke, Dual-homed, Full Mesh, Partial Mesh.
* WAN services at **Layer 1:** SONET, SDH, DWDM (DWDM widely used).
* WAN services at **Layer 2:** DSL, Cable, Wireless, MetroE, MPLS, PPP, HDLC, Frame Relay, ATM.
* WANs use **serial transmission** due to synchronization limits of parallel at distance.
* Traditional WANs: Dedicated (leased lines) & Switched (circuit/packet-switched).
* Modern WANs: Ethernet WAN, MPLS, Fiber.
* Internet-based broadband: DSL, Cable, Fiber, Wireless (Wi-Fi, Cellular, Satellite, WiMAX).
* VPNs secure Internet WANs.
* ISP connection options: Single, Dual, Multi, Dual-Multi homed.

---

## 📚 Additional Notes for NetAcad Questions

1. **WAN Characteristics:**

   * WAN covers large geographical areas & is provider-managed (fee-based).
   * LANs are small-scale & organization-owned.

2. **Topologies:**

   * Logical topology defines virtual connection paths.
   * Fully-meshed topology = most fault tolerant.

3. **Carrier Connections:**

   * Dual-carrier WAN provides redundancy.

4. **Layer 1 & 2 Services:**

   * Layer 1: Electrical/mechanical transmission standards (SDH, SONET, DWDM).
   * Layer 2: Frame encapsulation (PPP, HDLC, MetroE, MPLS).

5. **Key Terms:**

   * POP = point where customer connects to provider.
   * CPE = customer equipment.
   * DCE = device interfacing with provider.
   * Demarc = boundary line.

6. **WAN Devices:**

   * Cable & DSL modems function like voiceband modems but with broadband.
   * CSU/DSU = leased line interface.

7. **Communication:**

   * WAN always uses **serial transmission**.

8. **Technologies:**

   * Circuit-switched: PSTN, ISDN.
   * Packet-switched: Ethernet WAN, Frame Relay.
   * DWDM boosts fiber capacity.

9. **Modern WAN:**

   * Metro Ethernet is Ethernet-based WAN.
   * MPLS uses labels to route traffic efficiently.

---

## 🏁 Final Word

WANs are the **backbone of enterprise communication**, evolving from costly leased lines to modern scalable solutions. Today’s WANs focus on **speed, scalability, redundancy, and security** through technologies like **DWDM, MetroE, MPLS, VPNs, and FTTx**.
