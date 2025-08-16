# CCNA SRWE - Semester 2

## 📘 Module 1: Basic Device Configuration

### 🎯 **Module Objective**

Configure switches and routers using security best practices.

---

## 🔌 1.1 Configure a Switch with Initial Settings

### ⚡ **Switch Boot Sequence**

1. **POST (Power-On Self-Test)** – Tests CPU, DRAM, and flash.
2. **Boot loader loads** – Small program in ROM.
3. **CPU initialization** – Sets memory mapping, size, and speed.
4. **Initialize flash file system**.
5. **Load IOS** – Control passed to IOS.

**`boot system` command** – Sets the IOS boot file.

```bash
Switch(config)# boot system flash:/<IOS-file-path>
Switch# show boot     # View current boot file
```

**Startup-config** is stored in flash as `config.text`.

---

### 💡 **Switch LED Indicators**

* **SYST (System)**:

  * **Off** – No power.
  * **Green** – Normal operation.
  * **Amber** – System malfunction.

* **RPS (Redundant Power Supply)**:

  * **Off** – No RPS.
  * **Green** – RPS is ready.
  * **Blinking Green** – RPS available but in standby.
  * **Amber** – RPS is providing power (main supply failed).

* **STAT (Port Status)**:

  * **Off** – No link or port shutdown.
  * **Green** – Link up.
  * **Blinking Green** – Activity.
  * **Amber** – Port blocked due to STP or error.

* **DUPLX (Duplex)**:

  * **Off** – Half-duplex.
  * **Green** – Full-duplex.

* **SPEED**:

  * **Off** – 10 Mbps.
  * **Green** – 100 Mbps.
  * **Blinking Green** – 1000 Mbps.

* **PoE (Power over Ethernet)**:

  * **Off** – PoE not selected.
  * **Green** – PoE delivering power.
  * **Amber** – PoE fault or denied.

---

### 🛠 **Recover from System Crash**

1. Console connection.
2. Power off switch.
3. Power on and hold **Mode** button within 15 sec.
4. Release when LED turns amber then green.
5. Access `switch:` prompt.

**Useful commands:**

```bash
dir          # List files in flash
format flash: # Format flash
```

---

### 🌐 **Configure Switch Management Access**

* Assign IP to **SVI (Switch Virtual Interface)**.
* Configure **default gateway** for remote management.
* **Security best practice:** Avoid using VLAN 1 (default) as the management VLAN. Use a dedicated VLAN (e.g., VLAN 99) instead.

**Example – IPv4 & IPv6 Management VLAN 99**

```bash
Switch(config)# interface vlan 99
Switch(config-if)# ip address 172.17.99.11 255.255.255.0
Switch(config-if)# ipv6 address 2001:db8:acad:99::1/64
Switch(config-if)# no shutdown
Switch(config)# ip default-gateway 172.17.99.1
Switch# copy running-config startup-config
```

**Verify:**

```bash
show ip interface brief
show ipv6 interface brief
```

> *Note: IP on SVI is for management only, not routing.*

---

## 🔗 1.2 Configure Switch Ports

### ↔️ **Duplex Modes**

* **Full-duplex** – Simultaneous send/receive, no collisions.
* **Half-duplex** – One direction at a time, collisions possible.
* Gigabit/10Gb require **full-duplex**.

### ⚠️ **Collision Details**

* Normal Ethernet collisions occur **within the first 64 bytes (512 bits)** of a frame.
* **Late collisions** occur **after** 64 bytes have been transmitted — often caused by excessive cable length or duplex mismatch.

### ⚙️ **Manual Speed/Duplex Configuration**

```bash
Switch(config)# interface fa0/1
Switch(config-if)# duplex full
Switch(config-if)# speed 100
```

### 🔄 **Auto-MDIX**

* Auto-detects cable type (straight-through/crossover).

```bash
Switch(config-if)# mdix auto
```

> Requires speed & duplex set to **auto**.

---

### 🔍 **Verification Commands**

```bash
show interfaces [id]          # Interface details
show running-config           # Current config
show startup-config           # Saved config
show flash                    # Flash contents
show version                  # HW/SW info
show history                  # Command history
show ip interface [id]
show mac address-table
```

---

### 🧰 **Troubleshooting Interface Issues**

**Status meanings:**

* **up/down** – Physical OK, Data link issue.
* **down/down** – Cable/interface problem.
* **administratively down** – Shutdown command applied.

**Common errors:**

* **Runts** (<64B) – Frames smaller than allowed minimum.
* **Giants** (>1518B) – Frames larger than allowed maximum.
* **CRC errors** – Checksum mismatch, usually cabling/electrical issues.
* **Collisions** – Retransmissions due to shared medium (normal in half-duplex).
* **Late collisions** – Collisions after 64 bytes, often cable length/duplex issues.

**Input vs Output Errors:**

* **Input errors** – Sum of all receive errors (runts, giants, CRC, frame, no buffer, ignored).
* **Output errors** – Errors that prevented successful frame transmission (collisions, late collisions, underruns).

---

## 🔒 1.3 Secure Remote Access

### 🔑 **Telnet vs SSH**

* **Telnet** – TCP 23, unencrypted.
* **SSH** – TCP 22, encrypted, preferred.
* **K9 in IOS filename** – Indicates the IOS image supports cryptographic features (e.g., SSH, encryption).

### 🖥 **Enable SSH on Switch**

```bash
Switch(config)# hostname S1
Switch(config)# ip domain-name example.com
Switch(config)# crypto key generate rsa
Switch(config)# username admin secret ccna
Switch(config)# line vty 0 15
Switch(config-line)# transport input ssh
Switch(config-line)# login local
Switch(config)# ip ssh version 2
```

**Verify:**

```bash
show ip ssh
```

---

## 📡 1.4 Basic Router Configuration

### ⚙️ **Initial Steps**

```bash
Router(config)# hostname R1
Router(config)# enable secret class
Router(config)# banner motd # Unauthorized access prohibited! #
Router# copy running-config startup-config
```

### 🌍 **Configure Router Interfaces**

```bash
Router(config)# interface g0/0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
Router(config)# interface g0/0/1
Router(config-if)# ipv6 address 2001:db8:acad:1::1/64
Router(config-if)# no shutdown
```

### 🔁 **Loopback Interface**

```bash
Router(config)# interface loopback0
Router(config-if)# ip address 10.0.0.1 255.255.255.255
```

---

## 🛰 1.5 Verify Directly Connected Networks

### 📋 **Show Commands**

```bash
show ip interface brief
show ipv6 interface brief
show running-config interface g0/0/0
show ip route
show ipv6 route
```

**Routing Table Codes:**

* **C** – Connected network
* **L** – Local route

### 🔎 **Filtering Output**

```bash
show running-config | section line vty
show ip route | include 192.168
show ip route | exclude 10.0.0.0
show running-config | begin interface g0/0/0
```

* **section** – Shows the entire section starting with the expression.
* **include** – Displays only lines matching the expression.
* **exclude** – Hides lines matching the expression.
* **begin** – Shows output starting from the first matching line.

**History Control:**

```bash
show history
terminal history size 50
```

---

## 📌 **Key Takeaways**

* Boot process: POST → Boot Loader → CPU Init → Flash Init → IOS Load.
* Use VLAN SVI for switch management and avoid VLAN 1.
* Set duplex & speed manually when possible.
* Always use SSH instead of Telnet. IOS images with **K9** support encryption.
* Collisions happen within 64 bytes, late collisions after.
* Input and output errors provide detailed insight into network health.
* Loopback interfaces are always up and useful for testing.
* Show commands (with filters) are essential for troubleshooting & verification.

---

## 📝 Additional Notes for NetAcad Questions

* Use `show ipv6 interface brief` to display a summary of all IPv6-enabled interfaces with address & status. ✅
* Code **C** in routing table = directly connected route. ✅
* `show interface` displays packet flow counts, collisions, and buffer failures. ✅
* Every IPv6-enabled interface must have a **Link-local** address. ✅
* Filtering uses the **pipe (|)** character. ✅
* Filtering with **begin** shows output starting from the matching line. ✅

---

## 🔚 Final Word

This module builds the foundation for secure switch and router configuration. Mastery of boot processes, interface troubleshooting, secure remote access, and verification commands ensures strong skills for both configuration and problem-solving in real-world networks. 🚀

