# CCNA SRWE - Semester 2
## 📡 Module 12: WLAN Concepts

### 🎯 Module Objectives

Explain how WLANs enable network connectivity.

---

## 📶 12.1 Introduction to Wireless

### ✨ Benefits of WLANs

* Provide mobility in homes, offices, and campuses.
* Adaptable to rapid changes in needs and technologies.
* Wireless works in the **radio frequency spectrum** (radio waves), specifically in the 2.4 GHz and 5 GHz bands for WLANs.

### 🌐 Types of Wireless Networks

* **WPAN (802.15, 2.4 GHz)**: Short-range (6–9m), e.g., Bluetooth, Zigbee.
* **WLAN (802.11, 2.4/5 GHz)**: Medium range (\~100m indoor / 300ft).
* **WMAN**: City/district coverage, licensed frequencies.
* **WWAN**: National/global coverage, licensed frequencies.

### 📲 Wireless Technologies

* **Bluetooth (WPAN)**

  * BR/EDR: point-to-point, audio streaming.
  * BLE: supports mesh, IoT devices.
* **WiMAX (802.16)**: Wireless broadband, \~50 km range.
* **Cellular broadband**: 4G/5G - GSM (worldwide), CDMA (US).
* **Satellite broadband**: Requires dish + line of sight; rural use.

### 📜 802.11 WLAN Standards

| Standard | Frequency | Data Rate                | Notes                     |
| -------- | --------- | ------------------------ | ------------------------- |
| 802.11   | 2.4 GHz   | 2 Mb/s                   | First version             |
| 802.11a  | 5 GHz     | 54 Mb/s                  | Not compatible with b/g   |
| 802.11b  | 2.4 GHz   | 11 Mb/s                  | Better range, penetration |
| 802.11g  | 2.4 GHz   | 54 Mb/s                  | Backward with b           |
| 802.11n  | 2.4/5 GHz | 150–600 Mb/s             | Uses **MIMO**             |
| 802.11ac | 5 GHz     | 450 Mb/s–1.3 Gb/s        | Up to 8 antennas          |
| 802.11ax | 2.4/5 GHz | High-Efficiency Wireless | Can extend to 1–7 GHz     |

### 📡 Frequencies

* **2.4 GHz (UHF)**: b/g/n/ax
* **5 GHz (SHF)**: a/n/ac/ax

### 🏛️ WLAN Standards Bodies

* **ITU** – Regulates spectrum and satellites.
* **IEEE** – Defines modulation (802.11, 802.15, 802.16).
* **Wi-Fi Alliance** – Vendor group ensuring product interoperability.

---

## 🖧 12.2 WLAN Components

* **Wireless NICs**: Integrated in most devices; USB adapters available.
* **Wireless Home Router**: Combines

  * AP (wireless access),
  * Switch (wired LAN, usually 4 Ethernet ports),
  * Router (1 WAN/Internet port for ISP connection).
  * Antennas may be **integrated inside** or **external**.
* **Wireless Access Point (AP)**: Clients associate and authenticate here.
* **AP Categories**:

  * **Autonomous AP** – standalone, configured manually.
  * **Controller-based AP (Lightweight)** – managed by **WLC** (WLAN Controller) using LWAPP/CAPWAP. A single WLC can centrally manage **hundreds of APs**, simplifying configuration and monitoring.
* **Antennas**:

  * **Omnidirectional** – 360° coverage (homes/offices).
  * **Directional (Yagi, dish)** – focus in one direction.
  * **MIMO** – multiple antennas for bandwidth increase.
* **Beacon**: A management frame periodically sent by APs to advertise SSID, supported data rates, and security settings. It lets clients discover networks.
* **LAG (Link Aggregation Group)**: Combines multiple Ethernet links between WLC and switch to increase bandwidth and redundancy. **WLC supports LAG only** (bundled links) but not LACP or PAGP negotiation protocols.

---

## ⚙️ 12.3 WLAN Operation

### 🔄 Modes

* **Ad hoc** – Peer-to-peer between clients.
* **Infrastructure** – Clients connect via AP. APs connect to the network infrastructure using the wired distribution system.
* **Tethering** – Smartphone creates hotspot (variation of Ad hoc).

### 🧩 Topologies

* **BSS (Basic Service Set)** – One AP + clients. Different BSSs can’t talk directly.
* **ESS (Extended Service Set)** – Multiple BSSs linked via wired DS.
* **BSA (Basic Service Area)** – Physical area covered by a single AP’s signal.
* **ESA (Extended Service Area)** – Coverage area of multiple interconnected APs (larger than BSA).

```
   [Client]     [Client]
       \          /
        [AP]   ← BSS / BSA

   BSS + BSS + Wired DS = ESS / ESA
```

### 🖼️ 802.11 Frame

* Similar to Ethernet but with **more fields** (control, management, data).
* Contains **four possible addresses**:

  * **Address 1 (Receiver Address – RA)**: Device intended to receive the frame.
  * **Address 2 (Transmitter Address – TA)**: Device actually sending the frame.
  * **Address 3 (Destination Address – DA)**: Final intended destination.
  * **Address 4 (Source Address – SA)**: Original source of the packet.
* Extra addresses exist because frames may pass through an AP bridging wireless and wired segments.

### 🚦 CSMA/CA (Collision Avoidance)

1. Listen for idle channel.
2. Send RTS (Request to Send).
3. Wait for CTS (Clear to Send).
4. If no CTS, wait random backoff.
5. Send data.
6. Expect ACK; if not received → collision → retry.

### 📡 Client–AP Association

Steps: Discover → Authenticate → Associate.
Parameters: SSID, password, 802.11 mode, security (WEP/WPA/WPA2), channel.

### 🔍 Discovery

* **Passive** – AP sends beacons with SSID/security. Example: when you open your Wi-Fi list and see available networks.
* **Active** – Client probes hidden SSID. Example: when a network is hidden, you must manually enter SSID + password to connect.

### 🚶 Roaming

* Occurs when a wireless client moves between coverage areas of two APs in the same ESS.
* The client automatically re-authenticates and re-associates with the new AP to maintain seamless connectivity.

---

## 🖥️ 12.4 CAPWAP Operation

* **CAPWAP** = Control and Provisioning of Wireless APs.
* WLC manages APs centrally.
* Based on LWAPP, adds **DTLS security**.
* Uses **UDP 5246, 5247** (IPv4: protocol 17, IPv6: protocol 136).

### 🧩 Split MAC Architecture

* **AP handles**: Beacons, probe responses, ACKs, retransmissions, **encryption/decryption**.
* **WLC handles**: Authentication, association, frame translation, traffic termination.

```
   [AP] ⇆ [WLC]
   ├─ Beacons, ACKs, Encryption
   └─ Assoc, Auth, Frame Translation
```

### 🔐 DTLS Encryption

* Default: Control traffic encrypted (AP ↔ WLC).
* Data encryption requires extra DTLS license.

### 🌍 FlexConnect APs

* **Connected mode** – WLC reachable, normal CAPWAP.
* **Standalone mode** – WLC unreachable; AP authenticates locally + switches traffic.

---

## 📡 12.5 Channel Management

### ⚠️ Channel Saturation

* Too many devices → interference, poor quality.
* **Solutions**: DSSS, FHSS, OFDM.

### 📊 Techniques

* **DSSS (Direct Sequence Spread Spectrum)** – Spreads data across wider frequency band to reduce interference. Used by 802.11b.
* **FHSS (Frequency Hopping Spread Spectrum)** – Rapidly hops frequencies in a pattern known by sender & receiver. Early 802.11 standard.
* **OFDM (Orthogonal Frequency Division Multiplexing)** – Splits a channel into multiple orthogonal subcarriers for efficiency and robustness. Used in a/g/n/ac.

### 📺 Channel Selection

* **2.4 GHz**: 14 channels (depending on region). Use **1, 6, 11** (non-overlapping).
* **5 GHz**: Offers **24+ channels**, separated by 20 MHz. Non-overlapping examples: **36, 40, 44, 48** and others depending on region. More channels → less interference than 2.4 GHz.

### 🏗️ WLAN Deployment Planning

* Consider:

  * Building layout, walls, and interference sources.
  * Number of simultaneous users and devices per AP.
  * Expected data rates (video streaming, VoIP, IoT).
  * Placement: APs should have overlapping BSAs (\~10–15%) for roaming.
  * Transmit power: too high → interference, too low → coverage gaps.
  * Regional regulations:

    * **2.4 GHz**: up to 14 channels worldwide (though some regions allow fewer).
    * **5 GHz**: 24+ channels, availability depends on regulatory domain (UNII-1, UNII-2, UNII-3 bands).

---

## 🛡️ 12.6 WLAN Threats

### 🚨 Common Threats

* Data interception (sniffing unencrypted traffic).
* Wireless intruders (unauthorized access).
* DoS attacks (intentional jamming, misconfigurations).
* Rogue APs (planted or accidental).
* MITM (Evil Twin AP).

### ⛔ DoS Attacks

* Causes: misconfigured devices, malicious interference, accidental radio interference.
* Symptoms: dropped connections, high latency, inability to associate.
* Prevention: harden devices, secure passwords, backups, spectrum analysis, apply changes off-hours.

### 🎭 Rogue APs

* Unauthorized AP connected to secure network.
* Attacker can capture MACs, sniff traffic, launch MITM.
* Even personal hotspots can become rogue APs.
* Prevention: WLC rogue AP detection, spectrum monitoring, strict security policies.

### 🕵️‍♂️ MITM Attacks

* **Evil Twin AP**: fake AP with same SSID as real.
* Risks: credential theft, traffic manipulation.
* Defense: authenticate devices, use WPA3, monitor unusual traffic.

---

## 🔒 12.7 Secure WLANs

### 🛑 Legacy Security Features

* **SSID Cloaking** – Hide SSID beacon (weak protection, can still be sniffed).
* **MAC Filtering** – Allow/deny by MAC (easily spoofed).

### 🔑 Authentication

* **Open** – No password, insecure; VPN recommended.
* **Shared Key** – WEP, WPA, WPA2, WPA3.

### 🔐 Shared Key Methods

* **WEP** – RC4 static key; very weak, easily hacked.
* **WPA** – TKIP; per-packet keys, better than WEP but outdated.
* **WPA2** – AES (CCMP); strong security, still widely used.
* **WPA3** – Strongest, prevents brute force, requires PMF.

### 🏠 Home Authentication

* **Personal (PSK)** – Pre-shared password, no server needed.
* **Enterprise** – RADIUS + 802.1X/EAP. Provides centralized user authentication.

### 🧩 Encryption

* **TKIP** – Legacy support, less secure.
* **AES-CCMP** – Robust, current standard.

### 🏢 Enterprise Authentication (Details)

* Requires **AAA RADIUS** server.
* Components:

  * Server IP address.
  * UDP ports 1812 (auth), 1813 (accounting).
  * Shared secret key.
* Ensures centralized Authentication, Authorization, Accounting.

### 🆕 WPA3 Enhancements

* **Personal (SAE)** – Resists dictionary attacks.
* **Enterprise** – 192-bit encryption suite.
* **Open networks** – Opportunistic Wireless Encryption (OWE) adds encryption even without password.
* **IoT onboarding** – Device Provisioning Protocol (DPP) simplifies secure IoT setup.

---

## 📘 12.8 Module Summary

* WLAN types: WPAN, WLAN, WMAN, WWAN.
* Standards: IEEE 802.11, ITU, Wi-Fi Alliance.
* Components: APs, antennas, WLC, NICs, beacons.
* Operation: BSS/ESS, BSA/ESA, CSMA/CA, association, discovery, roaming.
* 802.11 frames: up to 4 addresses (RA, TA, DA, SA).
* CAPWAP: centralized AP management via WLC. AP handles encryption, WLC handles frame translation.
* Channel management: DSSS, FHSS, OFDM, avoid overlapping channels. 2.4 GHz = 14 channels, 5 GHz = 24+ (varies by region).
* Threats: interception, intruders, DoS, rogue APs, MITM.
* Security: SSID cloaking, MAC filtering, WPA2/WPA3, Enterprise authentication.
---
## 📘 Additional Notes for NetAcad Questions


* **WPAN** = short-range, low power (Bluetooth).
* **WLAN (802.11)** = 2.4/5 GHz, IEEE standard.
* **802.11a & 802.11ac** = only 5 GHz.
* **ITU-R** manages global frequencies.
* **Home router** = AP + Switch + Router.
* **Omnidirectional antennas** give 360° coverage.
* **Ad hoc mode** = peer-to-peer; **ESS** = multiple BSSs.
* **802.11 frame** has 4 addresses.
* **Passive mode** = beaconing, **Active** = probe request.
* **CAPWAP** supports IPv4/IPv6, uses UDP 5246 & 5247.
* **Split MAC** = AP (beacons, ACKs, encryption) vs WLC (auth, association, translation).
* **FlexConnect** can work standalone if WLC unreachable.
* **FHSS** hops channels, **DSSS** spreads, **OFDMA** in 802.11ax.
* **Channels**: 2.4 GHz Europe = 13, 5 GHz = 24.
* **Evil Twin AP** = MITM attack.
* Best security = **Authentication + Encryption** (WPA2/WPA3, AES).

---

## 🏁 Final Word

This module showed how WLANs work using radio frequencies and standards like 802.11. We learned the difference between WPAN, WLAN, WMAN, and WWAN, as well as the roles of IEEE, ITU-R, and the Wi-Fi Alliance. We explored WLAN components such as APs, routers, antennas, and controllers, and saw how clients connect, roam, and use frames with multiple addresses. CAPWAP enables centralized control with split roles between APs and WLCs. We also studied channel planning, modulation techniques, and how to avoid interference. Finally, we covered WLAN threats such as rogue APs and MITM attacks, and the importance of securing networks with modern authentication and encryption methods.

