# CCNA SRWE - Semester 2
 
## 📘 Module 10: LAN Security Concepts

### 🎯 Objectives

Explain how vulnerabilities compromise LAN security.

---

## 🖥️ 10.1 Endpoint Security

### ⚡ Common Network Attacks

* **DDoS (Distributed Denial of Service):** Many compromised devices (zombies) flood a service to degrade/stop access.
* **Data Breach:** Attackers steal confidential data from servers or hosts.
* **Malware:** Malicious software like ransomware (e.g., WannaCry) encrypts or damages data.
* **DoS (Denial of Service):** A single attacker floods resources with excessive traffic, unlike DDoS which uses many distributed hosts.

### 🔐 Security Devices

* **VPN-enabled router:** Secure remote access into enterprise.
* **Next-Generation Firewall (NGFW):** Stateful inspection, app control, intrusion prevention, malware protection, URL filtering.
* **Network Access Control (NAC):** Authentication, authorization, accounting (AAA). Example: Cisco Identity Services Engine (ISE).

### 🛡️ Endpoint Protection

* Endpoints = laptops, desktops, servers, IP phones, BYOD.
* Traditionally: antivirus, host firewall, HIPS.
* Best practice: NAC + AMP + ESA (Email Security Appliance) + WSA (Web Security Appliance).

### 📧 Cisco Email Security Appliance (ESA)

* Monitors SMTP.
* Updated by Cisco Talos every 3–5 minutes.
* Functions:

  * Block known threats.
  * Remediate stealth malware.
  * Block bad links & infected sites.
  * Encrypt outbound emails.

### 🌐 Cisco Web Security Appliance (WSA)

* Protects and controls web traffic.
* Provides: malware protection, app control, URL filtering, reporting.
* Can block/limit access to apps (chat, video, etc.).
* Performs blacklisting, malware scanning, HTTPS decryption.

---

## 🔑 10.2 Access Control

### 🔑 Authentication with Local Password

* Simple login/password on console, VTY, AUX.
* SSH preferred (secure, requires username+password).
* Limitations: not scalable, no fallback.

### 🛡️ AAA (Authentication, Authorization, Accounting)

* **Authentication:** Who can access.
* **Authorization:** What they can do.
* **Accounting:** Logs/audits activity.

### 👤 Authentication Methods

* **Local AAA:** User accounts stored locally (small networks).
* **Server-based AAA:** Central server (RADIUS or TACACS+), scalable.

### ✅ Authorization

* Applied automatically after authentication.
* Uses attributes to define user privileges.

### 📊 Accounting

* Collects/logs usage data: connection times, commands, packets, bytes.
* Useful for troubleshooting and forensic evidence.

### 🔒 IEEE 802.1X

* **Definition:** IEEE 802.1X is a port-based access control and authentication protocol.
* **Purpose:** Ensures that only authenticated devices can connect to the LAN through switch ports or wireless APs.
* **How it Works:**

  1. Device (Supplicant) connects to a switch port.
  2. The port is in an “unauthorized” state and only allows 802.1X traffic.
  3. Switch (Authenticator) requests identity info and forwards it to the Authentication Server (e.g., RADIUS).
  4. If credentials are valid, the port becomes “authorized” and normal traffic is allowed.
* **Roles:**

  * **Supplicant (Client):** Device seeking access.
  * **Authenticator (Switch or AP):** Gatekeeper between client and server.
  * **Authentication Server:** Validates identity and grants/denies access.
* **Benefit:** Prevents rogue devices from plugging into unused switch ports and gaining LAN access.

---

## ⚠️ 10.3 Layer 2 Security Threats

### 🔎 Layer 2 Vulnerabilities

* If compromised, higher layers (L3–L7) also exposed.
* LANs are weak because of trust assumptions (BYOD increases risk).

### 🚨 Attack Categories

* **MAC Table Attacks (Flooding):** Attacker floods switch with fake MACs to overflow table → switch floods all traffic → attacker sniffs data.
* **VLAN Attacks (Hopping):** Attacker forces port to become a trunk using DTP/802.1Q, gaining access to multiple VLANs.
* **VLAN Double-Tagging:** Frame contains two VLAN tags; outer tag stripped by first switch, inner tag delivered to second switch → attacker bypasses VLAN restrictions.
* **DHCP Attacks:**

  * **Starvation:** Attacker requests all DHCP addresses to exhaust pool.
  * **Spoofing:** Rogue DHCP server provides false config (wrong DNS, gateway, IP).
* **ARP Attacks:** Attacker poisons ARP cache with false mappings, enabling MITM attacks.
* **Address Spoofing:** Attacker changes IP or MAC to impersonate another device.
* **STP Attacks:** Attacker sends fake BPDUs to become root bridge and intercept traffic.

### 🛡️ Mitigation Techniques

* **Port Security:** Restricts number of MAC addresses on a port → stops MAC flooding & DHCP starvation.
* **DHCP Snooping:** Switch checks DHCP messages, allows only trusted servers → stops starvation/spoofing.
* **DAI (Dynamic ARP Inspection):** Validates ARP packets against trusted bindings → prevents ARP spoofing/poisoning.
* **IPSG (IP Source Guard):** Ensures IP/MAC binding validity → stops spoofing.
* **BPDU Guard:** Shuts down ports that receive BPDUs → prevents STP manipulation.

Additional practices:

* Use secure protocols (SSH, SCP, SFTP, SSL/TLS).
* Use out-of-band management.
* Dedicated management VLAN.
* ACLs for access filtering.

---

## 📥 10.4 MAC Address Table Attack

### 🔄 Switch Operation

* Switch builds MAC table from source MAC in frames.
* Used for forwarding decisions.

### 🌊 MAC Flooding

* Switch MAC table has finite size.
* Attacker floods with bogus MAC addresses until table is full.
* Switch floods all traffic (unknown unicast behavior).
* Attacker can capture traffic within VLAN.

### 🛡️ Mitigation

* Tools like **macof** can flood 8,000 frames/sec.
* Overflow impacts multiple switches.
* **Solution: Port Security** – limits number of MAC addresses learned per port.

---

## 🔓 10.5 LAN Attacks

### 🌉 VLAN Hopping Attack

* **How it works:** Attacker configures device to act as a switch using DTP + 802.1Q.
* **Result:** Switch forms trunk link with attacker → attacker gains access to all VLANs.
* **Mitigation:**

  * Disable trunking on all access ports.
  * Set ports to **access mode only**.
  * Disable auto-trunk negotiation (set to nonegotiate).

### 🏷️ VLAN Double-Tagging Attack

* **How it works:** Attacker embeds two VLAN tags.

  * First switch strips outer tag (native VLAN).
  * Second switch forwards frame using inner tag → attacker reaches target VLAN.
* **Result:** Attacker bypasses VLAN isolation.
* **Mitigation:**

  * Do not use VLAN 1 as native VLAN.
  * Assign unused VLAN as native VLAN.
  * Disable trunking on access ports.

### 📡 DHCP Attacks

* **Starvation:** Attacker exhausts DHCP pool.
* **Spoofing:** Rogue DHCP server misleads clients.
* **Mitigation:**

  * Enable **DHCP Snooping**.
  * Mark legitimate interfaces as trusted.
  * Drop DHCP messages on untrusted ports.

### 📡 ARP Attacks

* **How it works:** Attacker sends false ARP replies → binds their MAC to victim’s IP or default gateway.
* **Result:** MITM or DoS attack.
* **Mitigation:**

  * **DAI (Dynamic ARP Inspection).**
  * Use static ARP entries on critical devices.

### 🆔 Address Spoofing Attacks

* **IP Spoofing:** Attacker fakes IP → can impersonate or bypass ACLs.
* **MAC Spoofing:** Attacker fakes MAC → traffic for target is redirected.
* **Mitigation:**

  * **IPSG (IP Source Guard)** enforces valid IP-to-MAC bindings.

### 🌍 DNS Attacks

* **How it works:** DNS cache poisoning redirects users to malicious sites.
* **Mitigation:**

  * Use DNSSEC for integrity.
  * Filter unauthorized DNS traffic.
  * Monitor DNS anomalies.

### 🌳 STP Attack

* **How it works:** Attacker sends BPDUs with low bridge priority → becomes root bridge.
* **Result:** Attacker redirects traffic via their device.
* **Mitigation:**

  * **BPDU Guard** shuts down ports receiving rogue BPDUs.
  * Use **Root Guard** on trusted links.

### 🔍 CDP Reconnaissance

* **How it works:** Attacker listens to CDP broadcasts to gather device details (IP, VLAN, IOS version).
* **Result:** Reconnaissance for future attacks.
* **Mitigation:**

  * Disable CDP where not needed.
  * **Commands:**

    * `no cdp run` → disable globally.
    * `cdp run` → enable globally.
    * `no cdp enable` → disable on interface.
    * `cdp enable` → enable on interface.
  * For LLDP: `no lldp run`, `no lldp transmit`, `no lldp receive`.

---

## 📝 Summary (Module Review)

* Endpoints vulnerable to malware, DoS/DDoS, breaches → protect with NAC, AMP, ESA, WSA.
* AAA = Authentication, Authorization, Accounting.
* 802.1X = port-based authentication with Supplicant, Authenticator, Authentication Server.
* Layer 2 threats = MAC flooding, VLAN hopping/double-tagging, DHCP starvation/spoofing, ARP poisoning, spoofing, DNS poisoning, STP manipulation, CDP reconnaissance.
* Key defenses: Port Security, DHCP Snooping, DAI, IPSG, BPDU Guard, Root Guard, DNSSEC, secure management.

---

## 📌 Additional Notes for NetAcad Questions

* **Ransomware** is a specific type of malware that encrypts host data and demands payment for decryption.
* **WLC (Wireless LAN Controller)** is not considered a primary security device in this module.
* **ESA vs. WSA:** ESA secures email (SMTP), WSA secures web traffic (HTTP/HTTPS).
* **AAA:**

  * Authentication = identity check.
  * Authorization = what a user can do.
  * Accounting = logs and reports usage data.
* **802.1X:** The Authenticator (switch/AP) relays identity requests/responses between Supplicant and Authentication Server.
* **Layer 3–7 Mitigations:** VPNs, firewalls, and IPS devices protect upper layers; Layer 2 needs additional defenses.
* **MAC Address Table Attack Result:** Switch floods frames out all ports when table overflows.
* **DHCP Starvation Attack:** Attacker consumes all available DHCP leases, denying IP addresses to legitimate clients.
* **DHCP Spoofing Attack:** Rogue DHCP server issues incorrect IP config to clients.
* **ARP vs. Address Spoofing:**

  * ARP spoofing = false ARP replies to redirect traffic (man-in-the-middle).
  * Address spoofing = altering IP or MAC to impersonate another device.
* **STP Attack:** Attacker sends BPDUs with priority 0 to become root bridge.
* **CDP Reconnaissance:** Exploits CDP to discover IOS version, IP addresses, VLANs.

---

## 🏁 Final Word

LAN security is about understanding how attackers exploit weak trust at Layer 2 and endpoints, and then applying layered defenses. Always assume that any open port or unprotected endpoint can be abused. The core lessons: harden endpoints, enforce AAA and 802.1X, secure Layer 2 with port security, DHCP snooping, DAI, and IPSG, and minimize exposure of discovery protocols like CDP. Combine technical mitigations with monitoring, auditing, and user awareness. In LAN security, prevention at Layer 2 is as critical as firewalls and VPNs at higher layers.
