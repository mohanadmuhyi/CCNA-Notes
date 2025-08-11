# **📡 CCNA Module 15 – Application Layer**

## **🖥️ 1. Application, Presentation, and Session Layers**

The top 3 OSI layers (Application, Presentation, Session) map into the **TCP/IP Application Layer**.

### **📂 Application Layer**

* Interface between user applications and the underlying network.
* GUI (Graphical User Interface) the user interacts with.
* Examples: **HTTP**, **FTP**, **TFTP**, **IMAP**, **DNS**.
* Both source and destination devices must run compatible application protocols.

### **🎨 Presentation Layer Functions**

1. **Formatting** – Data is formatted for compatibility between devices (e.g., MKV, GIF, JPG standards).
2. **Compression** – Reduces size for faster transfer, decompressed by the receiver.
3. **Encryption/Decryption** – Secures data in transit.

### **💬 Session Layer Functions**

* Establishes, maintains, and terminates communication sessions.
* Handles dialog control, keeps sessions active, and restarts them if disrupted.

---

## **🤝 2. Peer-to-Peer (P2P)**

### **🔐 Client-Server Model**

* **Client** requests services, **Server** provides them.
* More secure and administratively controlled than P2P.
* Application layer protocols define request/response formats.

### **🔄 P2P Network**

* No dedicated server; all devices (**peers**) can be both clients and servers.
* Roles are assigned **per transaction**.
* Hybrid P2P uses a central index server for resource location.
* **BitTorrent** allows sharing of pieces of many files simultaneously.
* **Gnutella** allows sharing of whole files without a central index.

**Examples:** BitTorrent, Direct Connect, eDonkey, Freenet.

---

## **🌐 3. Web & Email Protocols**

### **🌍 HTTP & HTTPS**

* **HTTP**: Request/response protocol (insecure). Ports: **80, 8080**.
* **HTTPS**: Secure HTTP using encryption.
* **Common HTTP Methods:**

  * **GET** – Retrieve data (e.g., web page).
  * **POST** – Upload data files to the server (e.g., form submission).
  * **PUT** – Upload resources/content to server.

**Web Page Loading Steps:**

1. Browser parses URL (protocol, server name, file name).
2. Browser resolves server name via DNS.
3. Sends HTTP GET request for the file.
4. Server sends HTML back, browser renders it.

---

### **📧 Email Protocols**

* **SMTP** – Sends email (Port 25).
* **POP3** – Downloads email and removes from server (Port 110). Common in large networks.
* **IMAP** – Downloads copies while keeping mail on server until deleted. Gmail uses **IMAP**.

**Workflow:**

* SMTP handles sending (store-and-forward).
* POP/IMAP handle receiving.
* IMAP keeps server copies for multi-device access.

---

## **📡 4. IP Addressing Services**

### **🗂️ DNS (Domain Name System)**

* Converts domain names ↔ IP addresses.
* **Record Types:**

  * **A** – IPv4 address
  * **AAAA** – IPv6 address
  * **NS** – Name server
  * **MX** – Mail exchange
* Uses hierarchical structure with zones:

  * Root → Top-Level Domains (TLDs) → Second-Level Domains → Subdomains.
  * Each DNS server manages a portion (zone) and queries others when needed.
* If the DNS server cannot resolve locally, it forwards to another server.
* **Command:** `nslookup` shows the configured default DNS server.

---

### **📜 DHCP (Dynamic Host Configuration Protocol)**

* Automatically assigns IPv4 addresses, subnet masks, gateways, and DNS.
* Uses a **lease** system from an IP pool.
* **Static addressing** is manual.
* DHCPv6 provides IPv6 addresses but not default gateway.

**DORA Process (DHCPv4):**

1. **Discover (D)** – Broadcast request from client.
2. **Offer (O)** – Unicast or broadcast offer from server.
3. **Request (R)** – Broadcast from client accepting an offer.
4. **Acknowledge (A)** – Unicast confirmation from server.

**Extra DHCP Notes:**

* **D, R** are broadcasts; **O, A** are unicasts.
* If no DHCP/static config → device uses **APIPA** (169.254.x.x).
* If IP in R is reserved → restart from D.
* Lease renewal before expiry starts from **R** but unicast; security settings may force restart from D.
* DHCP can be a dedicated server or provided by a router.

---

## **📂 5. File Sharing Services**

### **📤 FTP (File Transfer Protocol)**

* Transfers files between client and server.
* **Control connection:** TCP 21 (commands, replies).
* **Data connection:** TCP 20 (actual file transfer).
* Supports **upload** (push) and **download** (pull).

### **🖨️ SMB (Server Message Block)**

* File, printer, and service sharing protocol (e.g., APIs, devices).
* Functions:

  1. Start, authenticate, terminate sessions.
  2. Control file/printer access.
  3. Send/receive messages.
* Long-term connection; resources appear as local.
* SMB and FTP can run on the same server.

---

## **📑 Key Ports Table**

| Service | Protocol | Port(s)                  |
| ------- | -------- | ------------------------ |
| DNS     | TCP/UDP  | 53                       |
| DHCP    | UDP      | 67 (server), 68 (client) |
| HTTP    | TCP      | 80, 8080                 |
| HTTPS   | TCP      | 443                      |
| SMTP    | TCP      | 25                       |
| POP3    | TCP      | 110                      |
| IMAP    | TCP      | 143                      |
| FTP     | TCP      | 20 (data), 21 (control)  |

---

## **🔒 Extra Insights for Networking & Security**

* **HTTP vs HTTPS:** HTTPS encrypts traffic with TLS, preventing eavesdropping.
* **POP3 drawback:** Not ideal for multi-device access—messages are deleted from server.
* **IMAP benefit:** Centralized email storage for easy access across devices.
* **DHCP security:** Can be targeted by rogue DHCP servers; secure with DHCP snooping.
* **SMB security:** Disable SMBv1 to prevent vulnerabilities (e.g., WannaCry ransomware).

---

## **📌 Additional Notes for NetAcad Questions**

* MKV, GIF, JPG → Presentation Layer.
* Application Layer handles protocols between programs on hosts.
* Application, Presentation, Session in OSI ≈ TCP/IP Application Layer.
* DNS, SMTP are Application Layer protocols.
* Session Layer handles dialog initiation and maintenance.
* P2P networks don’t require dedicated servers; peers can be both clients and servers.
* BitTorrent shares pieces of many files; Gnutella shares whole files.
* HTTP POST uploads files to a web server.
* SMTP sends email; IMAP keeps copies on server (used by Gmail).
* HTTP is **not** secure by default.
* DNS AAAA resolves IPv6; NS resolves authoritative servers.
* `nslookup` shows configured DNS server.
* FTP uses 2 connections: TCP 20 (data) and TCP 21 (control), supports push/pull.
* SMB works across multiple OS types.

---

## **✅ Final Word**

Mastering the Application Layer means understanding not only protocols and ports, but also how services interact and are secured in real-world networks. This knowledge forms the bridge between the user interface and the complex networking processes underneath.
