# CCNA SRWE - Semester 2

## 📘 Module 3: VLANs


### 🎯 Module Objectives

This module explains the purpose of VLANs in a switched network, describes how a switch forwards frames based on VLAN configuration in a multi-switch environment, shows how to configure a switch port to be assigned to a VLAN, demonstrates how to configure a trunk port on a switch, and teaches how to configure and verify Dynamic Trunking Protocol (DTP).

---

## 3.1 🔎 Overview of VLANs

### 🔹 What is a VLAN?

* VLAN = **Virtual Local Area Network**.
* Logical grouping of devices, regardless of physical location.
* Provides **network segmentation** on switches.
* Each VLAN is its own **broadcast domain**.

#### 📌 When do we need VLANs?

* When different groups of users/devices need to be isolated for **security or performance**.
* When there are multiple departments (e.g., HR, IT, Finance) on the same switch.
* **Scenario:** If PCs from different departments are connected to the same switch, VLANs ensure that their traffic is separated.

### ✨ Benefits of VLANs

| Benefit                   | Description                                                                       |
| ------------------------- | --------------------------------------------------------------------------------- |
| Smaller Broadcast Domains | Reduces unnecessary traffic                                                       |
| Improved Security         | Devices in different VLANs cannot directly communicate without a router/L3 switch |
| Improved IT Efficiency    | Group devices with similar needs (e.g., students, staff)                          |
| Reduced Cost              | One switch can support multiple groups                                            |
| Better Performance        | Less congestion, improved bandwidth utilization                                   |
| Simpler Management        | Easier grouping of similar devices                                                |

### 🔸 Types of VLANs

1. **Default VLAN**

   * VLAN 1 → Default for all ports.
   * Default Native VLAN + Management VLAN.
   * Cannot be deleted or renamed.
   * ⚠️ **Security Best Practice:** Remove interfaces from VLAN 1 to avoid attacks.

2. **Data VLAN**

   * Used for user traffic (web, email, etc.).
   * By default, VLAN 1.

3. **Native VLAN**

   * Used on **trunk links**.
   * Frames on native VLAN are **not tagged**.
   * Both ends of the trunk must have the same native VLAN.

4. **Management VLAN**

   * Used for switch management (SSH, Telnet, SNMP).
   * Best practice: not VLAN 1.

5. **Voice VLAN**

   * Dedicated for IP phones.
   * Needs high priority, low delay (<150 ms), no congestion.
   * Supports QoS.
   * **Note:** A VoIP phone is essentially a **3-port switch** (phone, PC, switch uplink).

---

## 3.2 🌐 VLANs in a Multi-Switched Environment

### 🔹 VLAN Trunks

* A **trunk** is a point-to-point link between switches (or switch-to-router).
* Carries multiple VLANs.
* Uses **802.1Q tagging**.
* By default, all VLANs are allowed on a trunk.
* **PC → Switch = Access port.**
* **Switch → Switch = Trunk port.**

### 🔸 Networks Without VLANs

* All devices see all broadcasts.
* Security and performance issues.

### 🔸 Networks With VLANs

* Broadcasts limited to VLAN.
* VLANs are isolated; need Layer 3 device for inter-VLAN communication.
* **Important:** Same VLANs should use the **same IP network/subnet**.

### 🔹 802.1Q VLAN Tag

* 4-byte tag inserted in Ethernet frame.
* Fields:

  * **Type (TPID)**: 0x8100
  * **User Priority**: 3 bits (QoS)
  * **CFI**: 1 bit (compatibility)
  * **VLAN ID (VID)**: 12 bits (1–4094 VLANs)

### 🔸 Native VLAN & Tagging

* Frames on native VLAN are **not tagged**.
* Must match on both trunk ends.

### 🔹 Voice VLAN Tagging

* Cisco IP phone acts like a **3-port switch** (PC + Phone + Uplink).
* Switch informs phone of Voice VLAN via **CDP**.
* Phone tags **voice traffic** with CoS.
* Data traffic from PC may or may not be tagged.

**Verification:**

```bash
show interfaces fa0/18 switchport
```

---

## 3.3 ⚙️ VLAN Configuration

### 🔸 VLAN Ranges

* **Normal Range (1–1005)**

  * Used in SMBs.
  * Stored in `vlan.dat` (flash).
  * Can be synchronized with VTP.
* **Extended Range (1006–4094)**

  * Used by service providers.
  * Stored in running-config.
  * Requires VTP transparent mode.

### 🔹 VLAN Creation Commands

```bash
Switch# configure terminal
Switch(config)# vlan <vlan-id>
Switch(config-vlan)# name <vlan-name>
Switch(config-vlan)# end
```

**Example:**

```bash
S1(config)# vlan 20
S1(config-vlan)# name Student
```

### 🔸 VLAN Port Assignment

```bash
Switch(config)# interface fa0/18
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20
Switch(config-if)# end
```

✅ You can configure multiple interfaces at once:

```bash
Switch(config)# interface range fa0/1 - 12
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 30
```

### 🔹 Data + Voice VLANs

* One access VLAN + One voice VLAN allowed.
* QoS enabled for voice.

```bash
Switch(config)# interface fa0/18
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20
Switch(config-if)# switchport voice vlan 150   # Command to add Voice VLAN
Switch(config-if)# mls qos trust cos          # Trust CoS for voice priority
```

### 🔸 Verify VLANs

```bash
show vlan brief
show vlan id <id>
show vlan name <name>
show vlan summary
```

### 🔹 Change VLAN Port Membership

```bash
Switch(config-if)# switchport access vlan <new-vlan>
Switch(config-if)# no switchport access vlan   # Back to VLAN 1
```

### 🔸 Delete VLANs

```bash
Switch(config)# no vlan <id>
Switch# delete vlan.dat
Switch# reload
```

---

## 3.4 🔗 VLAN Trunks

### 🔹 Trunk Configuration

```bash
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk native vlan 99
Switch(config-if)# switchport trunk allowed vlan 10,20,30,99
```

### 🔸 Verify Trunk

```bash
show interfaces fa0/1 switchport
show interfaces trunk
```

* Shows mode, encapsulation, native VLAN, allowed VLANs.

### 🔹 Reset Trunk to Default

```bash
Switch(config-if)# no switchport trunk allowed vlan
Switch(config-if)# no switchport trunk native vlan
Switch(config-if)# switchport mode access
```

---

## 3.5 🔄 Dynamic Trunking Protocol (DTP)

### 🔹 DTP Basics

* Cisco proprietary protocol.
* Default: **dynamic auto** on most switches.
* Can be disabled with:

```bash
Switch(config-if)# switchport nonegotiate
```

### 🔸 Interface Modes

| Mode              | Behavior                                     | Command                             |
| ----------------- | -------------------------------------------- | ----------------------------------- |
| access            | Permanent access mode                        | `switchport mode access`            |
| dynamic auto      | Becomes trunk if neighbor is trunk/desirable | `switchport mode dynamic auto`      |
| dynamic desirable | Actively tries to form trunk                 | `switchport mode dynamic desirable` |
| trunk             | Permanent trunk mode                         | `switchport mode trunk`             |

### 🔹 DTP Negotiation Results

Matrix of outcomes when two interfaces with different DTP modes are connected:

|                       | Access               | Dynamic Auto | Dynamic Desirable | Trunk                |
| --------------------- | -------------------- | ------------ | ----------------- | -------------------- |
| **Access**            | Access               | Access       | Access            | Limited Connectivity |
| **Dynamic Auto**      | Access               | Access       | Trunk             | Trunk                |
| **Dynamic Desirable** | Access               | Trunk        | Trunk             | Trunk                |
| **Trunk**             | Limited Connectivity | Trunk        | Trunk             | Trunk                |

### 🔹 Verify DTP

```bash
show dtp interface
```

---

## 3.6 🏗️ Network Layer Design

* **Access Layer:** PCs, Phones, Printers connected to access switches (ports in access mode).
* **Distribution Layer:** Aggregates multiple access switches, applies policies.
* **Core Layer:** High-speed backbone interconnecting distribution switches and routers.

---

## 3.7 ✅ Summary – Key Takeaways

* VLANs provide **logical segmentation** → smaller broadcast domains.
* Types: Default, Data, Native, Management, Voice.
* VLANs must be configured and assigned to switchports.
* **Trunks** carry multiple VLANs using **802.1Q tagging**.
* **Native VLAN** remains untagged.
* **Voice VLAN** ensures QoS.
* VLAN ranges: Normal (1–1005), Extended (1006–4094).
* **DTP** negotiates trunking but best practice is to set **manual mode**.
* **Best Practice:** Move devices out of VLAN 1 for security.
* **Access = PC → Switch, Trunk = Switch → Switch.**
* **Same VLANs should share the same IP subnet.**

---

## 📝 Additional Notes for NetAcad Questions

### 🔹 VLANs

* VLANs improve performance because each VLAN is a separate **broadcast domain** → reduces broadcast traffic.
* VLANs improve security by **isolating traffic**. Sensitive data can be kept in a separate VLAN.
* The **Native VLAN** carries untagged traffic on trunk ports. This is different from Default VLAN.
* Best practice: Do **not** leave the Native VLAN as VLAN 1. Change it for security.
* VLAN 1 facts:

  * All ports belong to VLAN 1 by default.
  * VLAN 1 is the **default native VLAN**.
  * VLAN 1 is the **default management VLAN**.
  * VLAN 1 cannot be deleted or renamed.

### 🔹 DTP

* **DTP is Cisco proprietary**, not an open IEEE standard.
* Cisco Catalyst switches are in **dynamic auto** mode by default.
* Two ports both in **dynamic auto** will not form a trunk (both wait for the other to negotiate).
* DTP negotiation:

  * Dynamic Auto forms trunk with **Trunk** or **Dynamic Desirable**.
  * Dynamic Desirable forms trunk with **Trunk**, **Dynamic Desirable**, or **Dynamic Auto**.

---

## 🔚 Final Word

VLANs are essential for network segmentation, security, and performance. Trunking and DTP allow VLANs to span multiple switches, but best practice is to use **manual configurations** for consistency and security. Remember: **PC ↔ Switch = Access**, **Switch ↔ Switch = Trunk**, and always plan VLAN-to-IP subnet mapping carefully.
