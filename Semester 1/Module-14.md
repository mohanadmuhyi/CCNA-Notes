# 📘 CCNA – Module 14: Transport Layer


---

## 📅 **Module Objective**

Compare the operations of transport layer protocols (TCP & UDP) in supporting end-to-end communication.

---

## 🚀 **14.1 Transportation of Data**

### 🔌 **Role of the Transport Layer**

* Provides **logical communication** between applications running on different hosts.
* Acts as a **link between the Application Layer** and the lower layers responsible for physical/network transmission.

### 📉 **Main Responsibilities**

1. **Tracking conversations** between applications.
2. **Segmentation & reassembly** of data.
3. Adding **header information** for delivery.
4. **Multiplexing**: managing multiple conversations at the same time — allowing multiple applications (e.g., browser, email) to work simultaneously.

### 🛠️ **Transport Layer Protocols**

* **TCP** – reliable, connection-oriented.
* **UDP** – fast, connectionless.

**Destination & Source Ports**

* **Source Port** identifies the sending application.
* **Destination Port** identifies the receiving application.
* Together, they allow multiple concurrent communications ("open more than one thing at a time").

---

## 💡 **14.2 TCP Overview**

### 🔍 **TCP Features**

* **Connection-oriented**: Uses a handshake before sending data.
* **Reliable delivery**: Lost/corrupted segments are retransmitted.
* **Same-order delivery**: Sequences data correctly.
* **Flow control**: Prevents overwhelming the receiver.
* **Initial Sequence Number (ISN)**: Chosen at connection start to begin sequence numbering (usually pseudo-random to prevent sequence prediction).

### 🛠️ **TCP is Stateful**

* Tracks sent and acknowledged data.

### 🔢 **TCP Header (fields, purpose, size)**

* **Minimum size**: 20 bytes (160 bits). **Maximum**: 60 bytes (480 bits) if options are used.

| Field                                       | Purpose                                                            | Size                 |
| ------------------------------------------- | ------------------------------------------------------------------ | -------------------- |
| Source Port                                 | Identify sending application (socket source)                       | 16 bits (2 bytes)    |
| Destination Port                            | Identify receiving application (socket destination)                | 16 bits (2 bytes)    |
| Sequence Number                             | Byte-number used for data reassembly and ordering                  | 32 bits (4 bytes)    |
| Acknowledgment Number                       | Next byte expected — used for ACKs                                 | 32 bits (4 bytes)    |
| Data Offset                                 | Indicates TCP header length (where data begins)                    | 4 bits (0.5 byte)    |
| Reserved                                    | Reserved for future use / alignment                                | 6 bits (\~0.75 byte) |
| Control Bits (URG, ACK, PSH, RST, SYN, FIN) | Connection control flags                                           | 6 bits (\~0.75 byte) |
| Window Size                                 | Flow control — number of bytes receiver can accept beyond last ACK | 16 bits (2 bytes)    |
| Checksum                                    | Error checking for header and data                                 | 16 bits (2 bytes)    |
| Urgent Pointer                              | Points to urgent data (if URG set)                                 | 16 bits (2 bytes)    |

> **Note:** These 10 main fields (above) make up the **minimum 20-byte TCP header**. Options (e.g., SACK permitted, timestamps) increase header length up to 60 bytes.

### 📲 **Applications using TCP**

* Web browsing (HTTP/HTTPS), Email (SMTP, POP3, IMAP), File Transfer (FTP).

---

## 🌐 **14.3 UDP Overview**

### 🔍 **UDP Features**

* **Connectionless**, no handshake.
* **No retransmissions** for lost segments.
* **No resource checks** – sender doesn’t know if receiver can handle data.
* **Order is not guaranteed**.

### 🔢 **UDP Header (fields, purpose, size)**

* **Fixed size**: 8 bytes (64 bits).

| Field            | Purpose                                             | Size              |
| ---------------- | --------------------------------------------------- | ----------------- |
| Source Port      | Identify sending application (may be 0 if unused)   | 16 bits (2 bytes) |
| Destination Port | Identify receiving application                      | 16 bits (2 bytes) |
| Length           | Length of UDP header + data                         | 16 bits (2 bytes) |
| Checksum         | Error checking (optional in IPv4, required in IPv6) | 16 bits (2 bytes) |

> **Note:** UDP header has **4 fields in an 8-byte header**.

### 📲 **Applications using UDP**

* **Real-time**: VoIP, streaming.
* **Simple queries**: DNS, DHCP.
* **Apps handling their own reliability**: SNMP, TFTP.
* **Note**: DNS and SNMP use UDP by default, but can fall back to TCP for larger replies or when reliability/order is required.

---

## 📢 **14.4 Port Numbers**

### 🔍 **Purpose**

* Allow multiple apps to send/receive data simultaneously.
* A **socket** = IP address + Port number.

### 🔢 **Port Number Ranges**

| Range         | Name                  | Use                                            |
| ------------- | --------------------- | ---------------------------------------------- |
| 0 – 1023      | Well-known ports      | Common services (HTTP, FTP, etc.)              |
| 1024 – 49151  | Registered ports      | Assigned to specific apps by IANA              |
| 49152 – 65535 | Dynamic/Private ports | Temporary client connections (ephemeral ports) |

**Common Well-Known Ports**

* 20/21 FTP, 22 SSH, 23 Telnet, 25 SMTP, 53 DNS, 67/68 DHCP, 69 TFTP, 80 HTTP, 110 POP3, 143 IMAP, 161 SNMP, 443 HTTPS.
* **RADIUS Server (1812)** is used for **AAA** (Authentication, Authorization, Accounting).

### 🛠️ **netstat Command**

* Shows active TCP/UDP connections (local/foreign addresses and port numbers, and state).

---

## 🔎 **14.5 TCP Communication Process**

### 💻 **TCP Server Processes**

* Each service uses a unique port number.
* “Open” port = ready to receive requests.

### 🔄 **Connection Establishment – 3-Way Handshake (detailed)**

1. **SYN** — Client → Server: sends the **ISN (Initial Sequence Number)** and SYN flag.
2. **SYN‑ACK** — Server → Client: server sends its own ISN and **ACK = client ISN + 1** (indicates it received the SYN and expects the next byte).
3. **ACK** — Client → Server: **ACK = server ISN + 1** — handshake complete and data transfer can begin.

* **ACK number meaning:** The ACK value equals the **next byte expected** from the peer.

### 🔍 **Session Termination – 4-Way Handshake (detailed)**

1. Host A sends **FIN** to indicate it has finished sending.
2. Host B sends **ACK** to acknowledge A's FIN.
3. Host B sends **FIN** when it has no more data to send.
4. Host A sends **ACK** to acknowledge B's FIN. — both directions closed.

**Flags Recap**

* **URG** – Urgent pointer field significant.
* **ACK** – Acknowledgment number is valid.
* **PSH** – Push function (deliver to application immediately).
* **RST** – Reset connection (used to abort a session; can be sent when an unexpected packet arrives).
* **SYN** – Synchronize sequence numbers (start session).
* **FIN** – No more data from sender (close session).

**RST and security:** RST packets can be forged to perform TCP reset attacks (tearing down active connections). Be aware in threat models.

**SACK example:** `ACK 3, SACK (5–10)` means the receiver is expecting byte 3 (so 3 & 4 are missing) and has successfully received bytes 5–10; next expected after the block is 11.

---

## 🛠️ **14.6 Reliability & Flow Control (TCP)**

### 🔍 **Reliability**

* Sequence numbers guarantee ordering and allow retransmission of missing data.
* **Selective Acknowledgment (SACK)** lets the receiver inform the sender about non-contiguous blocks it received, so the sender retransmits only missing pieces.

### 🔍 **Flow Control (Window Size) — clarified**

* **Window Size** is the number of **bytes the receiver is willing to accept** beyond the last byte acknowledged. It controls how much unacknowledged data the sender may have in flight.
* **Example:** If the last ACK from the receiver indicates it has received up to byte 10,000 and the receiver advertises a window size of 5,000 bytes, the sender may send bytes **10,001 through 15,000** without receiving another ACK. Once the sender reaches that limit it must wait for more ACKs (which may advertise a larger window) before sending more.

### 🔢 **MSS / MTU / Frame sizes**

* **Ethernet MTU (frame)**: 1518 bytes (typical Ethernet frame size including headers).
* **IP MTU (payload)**: 1500 bytes (maximum IP packet size carried inside Ethernet).
* **Typical MSS (TCP max segment size)** for IPv4 over Ethernet: **1500 − 20 (IP header) − 20 (TCP header) = 1460 bytes**.

### ⚠️ **Congestion Control (brief)**

* When the network is congested (packets are lost or delays increase), TCP uses congestion-control algorithms to reduce send rate. Common mechanisms:

  * **Slow Start** (grow congestion window exponentially)
  * **Congestion Avoidance** (linear growth after threshold)
  * **Fast Retransmit / Fast Recovery**
* Important variables: **cwnd (congestion window)**, **ssthresh (slow-start threshold)**.

### 🔧 **QoS**

* Quality of Service is useful to prioritize time-sensitive traffic (VoIP, video) when networks are congested.

---

## 🌐 **14.7 UDP Communication**

* **Low overhead**: Small header, no connection setup.
* **No sequencing**: Data delivered in arrival order.
* **Server processes** use well-known/registered ports.
* **Client processes** use dynamic source (ephemeral) ports.

---

## 🔄 **Key Comparisons: TCP vs UDP**

| Feature     | TCP                       | UDP                    |
| ----------- | ------------------------- | ---------------------- |
| Connection  | Connection-oriented       | Connectionless         |
| Reliability | Yes                       | No                     |
| Order       | Guaranteed                | Not guaranteed         |
| Overhead    | Higher                    | Lower                  |
| Speed       | Slower                    | Faster                 |
| Header Size | 20–60 bytes               | 8 bytes                |
| Use Cases   | Email, web, file transfer | Streaming, gaming, DNS |

---

## 📝 **Additional Notes for NetAcad Questions**


* **Socket example (web request):** If host **10.1.1.10** uses ephemeral port **1099** to request HTTP on server **10.1.1.254** (port 80), the socket pair is: **10.1.1.10:1099 → 10.1.1.254:80**. Written as a pair: **10.1.1.10:1099, 10.1.1.254:80**.

* **Socket example (email):** Client (ephemeral 49152) → Mail server (port 25): **Source: 10.x.x.x:49152 → Destination: serverIP:25**.

* **Socket example (DNS):** Client (ephemeral 49152) → DNS server (port 53): **Source: clientIP:49152 → Destination: dnsServerIP:53**.

* **TCP header / UDP header counts**

  * TCP = **10 fields** in the minimum **20-byte** header.
  * UDP = **4 fields** in the fixed **8-byte** header.

* **ISN detail:** The Initial Sequence Number (ISN) is chosen per host at connection start — typically pseudo-random — to reduce the risk of sequence-guessing attacks.

* **RST security note:** RST can be forged to forcibly close a connection (TCP reset attack). When modelling threats, consider protections like encryption/tunneling or session validation.

* **Congestion:** When congestion is detected, the **sending host reduces** how much unacknowledged data it sends (reduces cwnd).

---

## 🖌️ **ASCII Reminder**

```
TCP 3-Way Handshake:
Client                Server
  SYN(ISN=x)  ------------>
       <-----------  SYN-ACK (ACK=x+1, ISN=y)
  ACK(y+1)  ------------>

Session Termination (4-Way):
  FIN  ------------>
       <-----------  ACK
       <-----------  FIN
  ACK  ------------>
```

---

## 🔚 **Final Word**

Mastering the Transport Layer is about more than memorizing ports or handshake steps — it’s about understanding *why* TCP and UDP behave the way they do, and how their mechanisms (like flow control, reliability, and congestion handling) ensure smooth end-to-end communication. With these concepts clear, troubleshooting and optimizing networks becomes second nature.