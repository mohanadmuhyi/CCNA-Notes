# CCNA ENSA - Semester 3

## 🌐 Module 8: VPN and IPsec Concepts

### 🎯 Objectives

Explain how VPNs and IPsec are used to secure site-to-site and remote access connectivity.

---

## 🔐 8.1 VPN Technology

### 📡 What is a VPN?

* **Virtual** → Data travels over a public network (like the Internet), but appears private.
* **Private** → Traffic is **encrypted** to ensure confidentiality.
* Creates **end-to-end private connections** across untrusted networks.
* **VPN vs Proxy** → A VPN encrypts and tunnels your traffic, making it appear as if you are securely inside the private network. A proxy only forwards your messages and makes it appear as if the proxy itself sent them (no encryption by default).

### 💡 Benefits of VPNs

| Benefit          | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| 💰 Cost Savings  | Lower connectivity costs, higher bandwidth.                  |
| 🔒 Security      | Encryption + authentication protect data.                    |
| 📈 Scalability   | Easy to add users via Internet without extra infrastructure. |
| 🌍 Compatibility | Works across many WAN options (DSL, cable, fiber, etc.).     |

### 🔄 Site-to-Site vs. Remote Access VPNs

* **Site-to-Site VPN** → Permanent tunnel between gateways. Hosts don’t know VPN exists.
* **Remote Access VPN** → User-initiated, creates dynamic tunnel between client & VPN device.
* Example: Used by employees working from home to access company resources. Applications like **PulseSecure** allow secure remote connections.
* **VPN Gateway** → A device (router, firewall, or server) that terminates the VPN tunnel, performing encryption and decryption on behalf of the end hosts.

### 🏢 Enterprise vs. Service Provider VPNs

* **Enterprise VPNs** → Managed by the company using protocols like **IPsec** and **SSL/TLS**. Suitable for secure site-to-site or remote employee access.
* **Service Provider VPNs** → Offered and managed by ISPs. Typically use **MPLS (Layer 2/3)** to create logically separated VPNs for customers. Enterprises trust the provider to segregate their traffic from others.

---

## 🔎 8.2 Types of VPNs

### 👤 Remote-Access VPNs

* **Secure connection for mobile/remote users**.
* Two types:

  * **Clientless VPN** → SSL/TLS via web browser.
  * **Client-based VPN** → Requires client software (e.g., Cisco AnyConnect).

### 🔑 SSL/TLS VPNs

* **SSL (Secure Sockets Layer)** and its successor **TLS (Transport Layer Security)** use **PKI (public key infrastructure)** and digital certificates for authentication.
* Digital certificates contain the **public key** used for secure communication.
* Note: **IPsec and SSL/TLS can work together** in some deployments to provide layered security.

**Comparison: IPsec vs. SSL/TLS**

| Feature        | IPsec                          | SSL/TLS                           |
| -------------- | ------------------------------ | --------------------------------- |
| Applications   | All IP-based                   | Mostly web-based, file sharing    |
| Authentication | Strong (two-way)               | Moderate (1 or 2-way)             |
| Encryption     | Strong (56–256 bits)           | Moderate–Strong (40–256 bits)     |
| Complexity     | Medium (needs client software) | Low (just browser)                |
| Device Options | Limited (specific configs)     | Extensive (any device w/ browser) |

### 🖧 Site-to-Site IPsec VPNs

* Connects **entire networks** across Internet.
* Gateways encrypt outbound traffic → tunnel → decrypt at destination.
* End hosts send **normal unencrypted traffic**, VPN gateways handle security.

### 🔄 GRE over IPsec

* **GRE (Generic Routing Encapsulation)**: Encapsulates multiple protocols, supports multicast/broadcast, but **no encryption**.
* Combine with **IPsec** for confidentiality and authentication.
* Supports routing protocol updates (e.g., OSPF, EIGRP) through VPN tunnels.
* Terminology:

  * Passenger protocol = original packet (e.g., IPv4, OSPF).
  * Carrier protocol = GRE.
  * Transport protocol = IPsec tunnel.
* Benefit: Normal IPsec tunnels only carry **unicast**. GRE over IPsec allows multicast/broadcast traffic (needed for routing protocols).

### 🌐 DMVPN (Dynamic Multipoint VPN)

* Cisco solution for **scalable, dynamic, multi-site VPNs**.
* Uses **hub-and-spoke**, but can dynamically build **spoke-to-spoke direct tunnels**.
* Uses **mGRE (multipoint GRE)** + IPsec.
* Benefits:

  * Simplifies configuration for multiple sites.
  * Scales easily without manually defining all tunnels.
  * Allows secure on-demand communication between branches.

### 🛰️ IPsec Virtual Tunnel Interface (VTI)

* Simplifies multi-site/remote configuration.
* Applied to **virtual interface** instead of physical mapping.
* Supports **unicast + multicast encrypted traffic** (routing protocols supported without GRE).
* Can be hub-and-spoke or site-to-site.

### 🏬 MPLS VPNs (Service Provider)

* Service providers use **MPLS (Multiprotocol Label Switching)** in their core network. Traffic is forwarded by labels, not IP headers.
* Traffic is logically separated, so customers cannot see each other’s data.
* **Layer 3 MPLS VPN** → Provider participates in routing (customer ↔ provider router peering).
* **Layer 2 MPLS VPN** → Provider only transports Ethernet frames using **VPLS (Virtual Private LAN Service)**. Customer routers act like they are on the same LAN.
* Benefits: High scalability, provider-managed, and no encryption required (since isolation is handled by MPLS).

---

## 🔒 8.3 IPsec

### 🎯 Purpose of IPsec

* Standard by IETF for securing VPNs across IP networks.
* Provides:

  * **Confidentiality** → encryption.
  * **Integrity** → hashing.
  * **Origin Authentication** → IKE.
  * **Key Exchange** → Diffie-Hellman.

### 🧩 IPsec Framework

* **Flexible architecture** with building blocks that can be combined to create a unique **Security Association (SA)**.
* Components of the framework:

  * **Protocol encapsulation** → AH or ESP.
  * **Encryption algorithms** → DES, 3DES, AES, SEAL.
  * **Hashing algorithms** → MD5, SHA (for integrity).
  * **Authentication method** → PSK or RSA digital certificates.
  * **Key management** → Internet Key Exchange (IKE) protocol.
  * **Key exchange** → Diffie-Hellman groups.
* This modular design allows new technologies (e.g., new ciphers) to be added without rewriting the standard.

### 📦 Protocol Encapsulation

* Two options:

  * **AH (Authentication Header)** → Authentication only (no encryption).
  * **ESP (Encapsulation Security Protocol)** → Provides **encryption + authentication**.

### 🔑 Confidentiality (Encryption Algorithms)

* Encryption has **two types**:

  * **Stream ciphers** (encrypt data continuously, e.g., SEAL).
  * **Block ciphers** (encrypt blocks of data, e.g., AES, DES, 3DES).
* Examples:

  * **DES** → 56-bit (weak, outdated).
  * **3DES** → 168-bit (3×56).
  * **AES** → 128, 192, 256-bit (strong).
  * **SEAL** → 160-bit stream cipher.

### 📏 Integrity (Hashing)

* Ensures packet not modified in transit.
* **HMAC (Hashed Message Authentication Code)**.
* Algorithms:

  * **MD5** → 128-bit.
  * **SHA** → 160-bit.

### 🔐 Authentication Methods

1. **Pre-Shared Key (PSK)** → Simple, manual, not scalable.
2. **RSA (Digital Certificates)** → Strong, scalable, each peer authenticates the other. Certificates include the **public key**.

### 🔄 Key Exchange – Diffie-Hellman (DH)

* Securely generates shared keys over insecure channels.
* **Groups**:

  * Weak (deprecated): 1, 2, 5.
  * Stronger: 14 (2048-bit), 15 (3072-bit), 16 (4096-bit).
  * ECC (Elliptic Curve Cryptography): 19 (256-bit), 20 (384-bit), 21 (521-bit), 24 (2048-bit).

### 📡 Transport vs Tunnel Mode

* **Transport Mode** → Encrypts only payload, not IP header.
* **Tunnel Mode** → Encrypts **entire IP packet**, new IP header added.

---

## 📋 8.4 Summary

* VPN = secure, private, encrypted traffic over public networks.
* Benefits = cost savings, security, scalability, compatibility.
* **Remote Access VPNs** = IPsec/SSL/TLS, client or clientless (used in remote work via apps like PulseSecure).
* **Site-to-Site VPNs** = gateways encrypt/decrypt, hosts unaware.
* **GRE over IPsec** = supports multicast + routing updates.
* **DMVPN & IPsec VTI** = scalable, simplify config.
* **MPLS VPNs** = provider-managed secure VPN.
* IPsec framework = Confidentiality, Integrity, Authentication, DH key exchange.
* Algorithms: AES, DES, SHA, MD5, RSA, PSK, ECC.
* Encryption types = Stream vs Block ciphers.
* Modes: Transport vs Tunnel.

---

## 📘 Additional Notes for NetAcad Questions

* **Scalability** → VPN benefit that allows easy addition of new users.
* **Cost Savings** → VPN benefit that increases bandwidth for remote sites without extra WAN links.
* **Security** → VPN benefit that protects data with encryption/authentication.
* **Remote-Access VPN** → Connects mobile users.
* **Enterprise-managed VPNs** → IPsec, SSL/TLS, DMVPN.
* **Clientless VPN** → Uses HTTPS via web browser.
* **SSL/TLS VPN** → Only requires browser on host.
* **GRE** → Carrier protocol for tunneling.
* **DMVPN** → Rapidly scales secure access organization-wide.
* **MPLS VPN** → Emulates Ethernet multiaccess LAN with remote sites.
* **IPsec protects Layers 4–7** of OSI model.
* **IPsec framework is modular** (not updated for every new standard).

---

## 📝 Final Word

VPNs are a cornerstone of modern secure networking, enabling organizations to connect remote workers, branch offices, and partners safely over untrusted networks. With different types (remote access, site-to-site, DMVPN, MPLS), and powerful security from the IPsec framework (confidentiality, integrity, authentication, key exchange), VPNs remain essential for scalability, cost savings, and security in today’s interconnected world.
