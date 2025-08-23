# CCNA SRWE – Semester 2

## 📘 Module 8: SLAAC and DHCPv6

### 🎯 Objectives

**Configure dynamic address allocation in IPv6 networks.**

---

## 🌍 8.1 IPv6 Global Unicast Address (GUA) Assignment

### 🔹 IPv6 Host Configuration

* Router: Configure manually →

  ```
  R1(config-if)# ipv6 address ipv6-address/prefix-length
  ```
* Windows host: Can configure manually but error-prone → better to use dynamic.
* In **IPv4**, PC cannot get an IP without static config or DHCP.
* In **IPv6**, PC can still generate a routable IP without DHCP (SLAAC). DHCP is optional.

### 🔹 Link-Local Address (LLA)

* Automatically created when interface boots.
* Needed for communication within the same link.
* `%zoneID` at end = scope ID to associate with interface.
* No ARP in IPv6 → instead uses **NS (Neighbor Solicitation)** and **NA (Neighbor Advertisement)**.
* DHCPv6 defined in RFC 3315.

### 🔹 Router Advertisements (RA)

* Sent periodically by routers (ICMPv6).
* Used for **dynamic IPv6 configuration**.
* Methods:

  * **Stateless (SLAAC)**
  * **Stateful (DHCPv6)**
  * Combination of both.

### 🔹 RA Flags

* **A flag (Address Autoconfig)** → SLAAC
* **O flag (Other Config)** → Info from **stateless DHCPv6**
* **M flag (Managed Config)** → Use **stateful DHCPv6**
* Default = `100` (SLAAC only).
* `110` → SLAAC + DHCPv6 (Other info)
* `001` → DHCPv6 only
* `R1(config-if)# ipv6 nd managed-config-flag` sets `001`
* `R1(config-if)# ipv6 nd other-config-flag` sets `110`

---

## ⚙️ 8.2 SLAAC (Stateless Address Autoconfiguration)

### 🔹 Overview

* No DHCPv6 server needed.
* Hosts **self-generate IPv6 GUA**.
* ICMPv6 RA messages every **200s** provide:

  * Prefix
  * Default gateway
  * MTU
  * DNS (in modern OS, RAs can include DNS)
* Hosts can also request RA using **RS (Router Solicitation)** to ff02::2.

### 🔹 Enabling SLAAC (Configuration Steps)

1. Enter global config mode and enable IPv6 routing:

   ```
   R1(config)# ipv6 unicast-routing
   ```
2. Go to interface and assign IPv6 GUA:

   ```
   R1(config)# interface g0/0/1
   R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
   ```
3. Assign link-local (optional, otherwise auto-generated):

   ```
   R1(config-if)# ipv6 address fe80::1 link-local
   ```
4. Verify router is in all-routers group:

   ```
   R1# show ipv6 interface g0/0/1
   ```

🔸 By default, SLAAC RAs are sent with `A=1, O=0, M=0`.

### 🔹 SLAAC Only Method

* RA Flags: `A=1, O=0, M=0`
* Host generates own GUA using prefix.
* Default GW = router’s LLA.
* SLAAC doesn’t need DHCP → router gives prefix, host generates the rest (random/EUI-64).

### 🔹 SLAAC with DHCP (Stateless)

* RA Flags: `A=1, O=1, M=0`
* Host generates its own IP (SLAAC).
* DHCPv6 provides **DNS, domain name, etc.**

### 🔹 Interface ID Generation

* Two methods:

  1. **Randomly generated** (default in modern OS like Win10).
  2. **EUI-64**: Uses MAC + `fffe` in middle.

### 🔹 Duplicate Address Detection (DAD)

* Host sends ICMPv6 **NS (Neighbor Solicitation)**.
* If no **NA (Neighbor Advertisement)** reply → address is unique.
* Provides near 100% uniqueness.

### 🔹 ICMPv6 Messages Summary

* **RS (Router Solicitation):** Host → Router asking for RA.
* **RA (Router Advertisement):** Router → Host, gives prefix, gateway, flags.
* **NS (Neighbor Solicitation):** Host → to check uniqueness of address.
* **NA (Neighbor Advertisement):** Reply confirming address already exists.

---

## 📡 8.3 DHCPv6

### 🔹 Operation Steps

1. Host sends RS → Router replies with RA.
2. Host sends **DHCPv6 SOLICIT**.
3. Server replies with **ADVERTISE**.
4. Host → **REQUEST**.
5. Server → **REPLY**.

🔸 Ports:

* Client → Server = **UDP 547**
* Server → Client = **UDP 546**

### 🔹 Stateless DHCPv6

* RA: `A=1, O=1, M=0`
* Host uses SLAAC for address.
* Gets extra info (DNS, domain name) from DHCPv6 server.
* Enable on router:

  ```
  R1(config-if)# ipv6 nd other-config-flag
  ```
* Reset:

  ```
  R1(config-if)# no ipv6 nd other-config-flag
  ```

### 🔹 Stateful DHCPv6

* RA: `A=0, O=0, M=1`
* Host gets **all IPv6 config** (IP, DNS, gateway, etc.) from DHCPv6 server (like DHCPv4).
* Enable on router:

  ```
  R1(config-if)# ipv6 nd managed-config-flag
  ```

---

## 🖥️ 8.4 Configure DHCPv6 Server

### 🔹 Router Roles

* **DHCPv6 Server** → Provides stateful/stateless services.
* **DHCPv6 Client** → Router gets IPv6 config from another server.
* **Relay Agent** → Forwards DHCPv6 messages if client/server are on different networks.

### 🔹 Configure Stateless DHCPv6 Server

1. Enable IPv6 routing:

   ```
   R1(config)# ipv6 unicast-routing
   ```
2. Define pool:

   ```
   R1(config)# ipv6 dhcp pool POOL-NAME
   R1(dhcpv6-config)# dns-server X:X:X:X
   R1(dhcpv6-config)# domain-name example.com
   ```
3. Bind interface:

   ```
   R1(config)# interface g0/0/1
   R1(config-if)# ipv6 dhcp server POOL-NAME
   R1(config-if)# ipv6 nd other-config-flag
   ```
4. Verify on host:

   ```
   PC> ipconfig /all
   ```

### 🔹 Configure Stateless DHCPv6 Client

1. Enable IPv6 routing:

   ```
   R2(config)# ipv6 unicast-routing
   ```
2. Enable LLA:

   ```
   R2(config-if)# ipv6 enable
   ```
3. Use SLAAC:

   ```
   R2(config-if)# ipv6 address autoconfig
   ```
4. Verify:

   ```
   R2# show ipv6 interface brief
   R2# show ipv6 dhcp interface g0/0/1
   ```

### 🔹 Configure Stateful DHCPv6 Server

1. Enable IPv6 routing:

   ```
   R1(config)# ipv6 unicast-routing
   ```
2. Define pool:

   ```
   R1(config)# ipv6 dhcp pool STATEFUL-POOL
   R1(dhcpv6-config)# address prefix 2001:db8:acad:2::/64
   R1(dhcpv6-config)# dns-server 2001:4860:4860::8888
   R1(dhcpv6-config)# domain-name example.com
   ```
3. Bind interface:

   ```
   R1(config)# interface g0/0/1
   R1(config-if)# ipv6 dhcp server STATEFUL-POOL
   R1(config-if)# ipv6 nd managed-config-flag
   R1(config-if)# ipv6 nd prefix default no-autoconfig
   ```
4. Verify:

   ```
   PC> ipconfig /all
   ```

### 🔹 Configure Stateful DHCPv6 Client

1. Enable IPv6 routing:

   ```
   R2(config)# ipv6 unicast-routing
   ```
2. Enable LLA:

   ```
   R2(config-if)# ipv6 enable
   ```
3. Use DHCP:

   ```
   R2(config-if)# ipv6 address dhcp
   ```
4. Verify:

   ```
   R2# show ipv6 interface brief
   R2# show ipv6 dhcp interface g0/0/1
   ```

### 🔹 Verification Commands

* DHCPv6 Pool Info:

  ```
  R1# show ipv6 dhcp pool
  ```
* Client Bindings:

  ```
  R1# show ipv6 dhcp binding
  ```

### 🔹 Configure DHCPv6 Relay Agent

```
R2(config-if)# ipv6 dhcp relay destination ipv6-address [interface]
```

* Verify:

  ```
  R2# show ipv6 dhcp interface
  R2# show ipv6 dhcp binding
  ```

---

## 📝 8.5 Module Summary

* In IPv4, a host needs either **static IP** or **DHCP**. No other option.
* In IPv6, hosts can self-generate GUAs using SLAAC (stateless). DHCP is **optional**.
* No ARP in IPv6 → uses **NS/NA** for neighbor discovery.
* RAs guide hosts on how to configure IPv6 using **A, O, M flags**.
* SLAAC = self-generate GUA.
* SLAAC + DHCPv6 (Stateless) = GUA via SLAAC + extra info from DHCP.
* Stateful DHCPv6 = full config from DHCPv6 server (like DHCPv4).
* RA flag combinations:

  * `100` (default) → SLAAC only
  * `110` → SLAAC + DHCPv6 (other info)
  * `001` → DHCPv6 only
* Cisco routers can be DHCPv6 **server, client, relay**.

---

## 📌 Additional Notes for NetAcad Questions

* **Default automatic address when no RA is received:** Link-local address.
* **Stateless DHCP:** Combination of SLAAC + stateless DHCPv6 server for extra info.
* **SLAAC ICMPv6 messages:** RS and RA.
* **Command to join all-routers group (ff02::2):** `R1(config)# ipv6 unicast-routing`.
* **SLAAC only flags:** A=1, M=0, O=0.
* **Message host sends to locate router:** Router Solicitation (RS).
* **Uniqueness check method:** Duplicate Address Detection (DAD).
* **DHCPv6 client → server UDP port:** 547.
* **Message host sends to find DHCPv6 server:** SOLICIT.
* **Message host sends in stateful DHCPv6:** REQUEST.
* **Stateless DHCP flags:** A=1, M=0, O=1.
* **M flag =1 →** Stateful DHCPv6.
* **Router roles in DHCPv6:** Server, Client, Relay Agent.
* **Not configured in stateless DHCPv6:** `address prefix`.
* **Command for client router to get GUA via SLAAC:** `R1(config-if)# ipv6 address autoconfig`.
* **Command on router interface to provide DHCPv6 server services:** `R1(config-if)# ipv6 dhcp server POOL-NAME`.
* **Command for router to acquire GUA from DHCPv6 server:** `R1(config-if)# ipv6 address dhcp`.
* **Verify link-local + GUA of DHCPv6 clients:** `R1# show ipv6 dhcp binding`.
* **Relay agent configuration on client LAN interface:** `R2(config-if)# ipv6 dhcp relay destination ipv6-address [interface]`.


---

## ✅ Final Word

Mastering SLAAC and DHCPv6 is essential for understanding **how IPv6 hosts dynamically configure themselves** and how routers guide that process. These concepts form the foundation of modern IPv6 networks and are heavily tested in CCNA exams. 🚀
