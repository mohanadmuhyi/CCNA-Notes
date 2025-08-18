# CCNA SRWE - Semester 2

## 🌐 Module 7: DHCPv4

### 🎯 Objectives

Implement **DHCPv4** to operate across multiple LANs.

---

## 📘 7.1 DHCPv4 Concepts

### 🔑 DHCPv4 Server & Client

* **DHCPv4** dynamically assigns IPv4 addresses and network info.
* Dedicated DHCP server = scalable, easy management.
* Cisco routers, servers, or even firewalls can provide **DHCPv4 services**.
* **Lease mechanism**:

  * IPv4 addresses are leased for a set time (24h → 1 week or more).
  * Clients usually renew before lease expiration.
  * Prevents unused devices from holding addresses.

### ⚙️ DHCPv4 Operation

* Works in **Client/Server** model.
* Client leases IP until expiration.
* Client must **periodically renew** lease.
* Expired leases return to pool for reassignment.

### 🔄 DHCP Lease Process (DORA)

1. **DHCP Discover (DHCPDISCOVER)** – Client broadcasts request.

   * **Why broadcast?** Client does not yet know the DHCP server’s IP/MAC.
   * Source MAC = PC, Destination MAC = `FF:FF:FF:FF:FF:FF`
   * Source IP = `0.0.0.0`, Destination IP = `255.255.255.255`

2. **DHCP Offer (DHCPOFFER)** – Server responds with IP offer.

   * **Why unicast?** The server already knows the client’s MAC.
   * Source MAC = DHCP server, Destination MAC = PC
   * Source IP = DHCP server IP, Destination IP = offered IP

3. **DHCP Request (DHCPREQUEST)** – Client requests offered IP.

   * **Why broadcast?** Ensures all DHCP servers see which offer the client accepted.

4. **DHCP Acknowledgment (DHCPACK)** – Server confirms assignment.

   * **Why unicast?** Final confirmation sent directly to the requesting client.

### 📋 DORA Packet Table

| Step     | Source IP                        | Destination IP  | Source MAC  | Destination MAC        |
| -------- | -------------------------------- | --------------- | ----------- | ---------------------- |
| Discover | 0.0.0.0                          | 255.255.255.255 | PC          | FF\:FF\:FF\:FF\:FF\:FF |
| Offer    | DHCP Server IP                   | Offered IP      | DHCP Server | PC                     |
| Request  | 0.0.0.0 (or offered IP if renew) | 255.255.255.255 | PC          | FF\:FF\:FF\:FF\:FF\:FF |
| ACK      | DHCP Server IP                   | Client IP       | DHCP Server | PC                     |

### ♻️ Lease Renewal

1. **DHCPREQUEST** – Client unicasts renewal to original server.

   * **Why unicast?** The client already knows the server’s IP.
   * If no response → broadcasts request to reach another server.
2. **DHCPACK** – Server verifies and extends lease.

⚠️ If DHCP requests do not get a response, the PC uses **APIPA** (`169.254.0.0/16`).

---

## 📘 7.2 Configure a Cisco IOS DHCPv4 Server

### 🖥️ DHCPv4 Server on Cisco IOS

* Router can act as DHCPv4 server.
* Assigns/manages IPs from defined pools.

### 📝 Steps to Configure DHCPv4 Server

1. **Exclude addresses** (for static devices like routers/printers):

   ```bash
   R1(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.9
   ```

   ⚠️ Always exclude used IPs to avoid conflicts.

2. **Define DHCP pool**:

   ```bash
   R1(config)# ip dhcp pool LAN1
   R1(dhcp-config)#
   ```

3. **Configure pool settings**:

   ```bash
   R1(dhcp-config)# network 192.168.10.0 255.255.255.0
   R1(dhcp-config)# default-router 192.168.10.1
   R1(dhcp-config)# dns-server 8.8.8.8 8.8.4.4
   R1(dhcp-config)# domain-name example.local
   R1(dhcp-config)# lease 7
   ```

📌 If multiple pools exist, the router chooses based on the **interface** where the DHCP broadcast originated.

### ✅ Verifying DHCPv4

Commands:

* `R1# show running-config | section dhcp` → Shows DHCP config.
* `R1# show ip dhcp binding` → Lists active DHCP leases.
* `R1# show ip dhcp server statistics` → DHCP messages sent/received.
* On client PC: `C:\> ipconfig /all` → Shows assigned IP, gateway, DNS.

### 🚫 Disable DHCP Service

```bash
R1(config)# no service dhcp
R1(config)# service dhcp   # Re-enable
```

⚠️ Restarting may cause temporary duplicate IPs.

### 📡 DHCPv4 Relay (Helper Address)

* Problem: If DHCP server is on another subnet, router does **not** forward client broadcasts.
* Solution: Configure **helper address**:

  ```bash
  R1(config)# interface g0/0
  R1(config-if)# ip helper-address 192.168.11.6
  ```
* Behavior:

  * PC → Router = Broadcast
  * Router → DHCP Server = Unicast
* The helper address is configured on the **interface receiving broadcasts** and points to the DHCP server IP.

### 📡 Other UDP Services Relayed by Default

* Port 37 → Time
* Port 49 → TACACS
* Port 53 → DNS
* Port 67 → DHCP/BOOTP Server
* Port 68 → DHCP/BOOTP Client
* Port 69 → TFTP
* Port 137 → NetBIOS Name Service
* Port 138 → NetBIOS Datagram Service

---

## 📘 7.3 Configure a DHCPv4 Client

### 🖥️ Router as DHCP Client

* Some SOHO/branch routers must act as DHCP clients (e.g., ISP-provided IPs).
* Configure interface as DHCP client:

  ```bash
  R1(config)# interface g0/0/1
  R1(config-if)# ip address dhcp
  R1(config-if)# no shutdown
  ```
* Verify:

  ```bash
  R1# show ip interface g0/0/1
  ```
* 📌 You can configure **router interfaces** to take IPs directly from DHCP.

### 🏠 Home Router as DHCP Client

* Home routers typically default to **Automatic Configuration - DHCP**.
* WAN interface requests IP from ISP’s DHCP server.

---

## 📋 Module Summary

* DHCPv4 dynamically leases IP addresses to clients.
* **DORA** process: Discover → Offer → Request → Acknowledgment.
* Why broadcast/unicast in DORA:

  * Discover = Broadcast (client doesn’t know server)
  * Offer = Unicast (server knows client)
  * Request = Broadcast (to inform all servers)
  * ACK = Unicast (final confirmation)
* Routers can act as **DHCP servers, clients, or relays**.
* DHCP service enabled by default (`service dhcp`).
* Home/SOHO routers usually default to DHCP client mode.
* If no DHCP response, client uses **APIPA** addressing.

---

## 📘 Additional Notes for NetAcad Questions

* The DHCPv4 client starts the process with **DHCPDISCOVER**.
* The DHCPv4 server responds during the lease process with **DHCPOFFER** and **DHCPACK**.
* During lease renewal, the messages used are **DHCPREQUEST** and **DHCPACK**.

---
## ✅ Final Words

DHCPv4 automates IP address management, reducing admin effort and preventing conflicts. Mastering DORA, configuration commands, and relay concepts ensures smooth IP allocation in both small and enterprise networks.
