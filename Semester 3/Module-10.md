# CCNA ENSA - Semester 3

## 📘 Module 10: Network Management

### 🎯 Objectives

In this module, you will learn how to implement protocols to manage the network. Topics include using **CDP and LLDP** to map network topology, implementing **NTP** for time synchronization, understanding **SNMP** operations, configuring **Syslog** for logging, performing **router and switch file maintenance**, and managing **IOS images**.

---

### **🔍 10.1 Device Discovery with CDP (Cisco Discovery Protocol)**

* **Purpose:** Cisco proprietary Layer 2 protocol used to discover directly connected Cisco devices (routers, switches, IP phones, etc.) without needing Layer 3 information.
* **Operation:**

  * Devices send CDP advertisements every **60 seconds** to the multicast MAC address `01:00:0C:CC:CC:CC`.
  * Each CDP message contains: device ID, IP address, port ID, capabilities, and platform.
  * CDP works even if IP addressing is not configured.
* **Usage:** Helps in mapping network topology and troubleshooting connectivity between Cisco devices.

**🧠 Commands:**

```bash
show cdp                   # Verify global CDP status
show cdp interface          # Display CDP-enabled interfaces
show cdp neighbors          # List of directly connected Cisco devices
show cdp neighbors detail   # Detailed info (IP, platform, capabilities)
no cdp enable               # Disable CDP on an interface
cdp enable                  # Enable CDP on an interface
cdp run                     # Enable CDP globally
no cdp run                  # Disable CDP globally
```

* **Tip:** Disable CDP on untrusted or external interfaces for security reasons.

---

### **🌐 10.2 Device Discovery with LLDP (Link Layer Discovery Protocol)**

* **Purpose:** Vendor-neutral discovery protocol similar to CDP, standardized by IEEE 802.1AB.
* Works on routers, switches, and wireless devices to advertise device ID, capabilities, and port descriptions.
* LLDP frames are sent to multicast MAC `01:80:C2:00:00:0E`.

**🧠 Commands:**

```bash
lldp run                  # Enable LLDP globally
no lldp run               # Disable LLDP globally
interface g0/1
  lldp transmit           # Enable LLDP advertisement
  lldp receive            # Enable LLDP reception
show lldp                 # Verify LLDP global status
show lldp neighbors       # List directly connected devices
show lldp neighbors detail# Detailed neighbor info (IP, OS, capabilities)
```

* **Note:** Use both CDP and LLDP when mixing Cisco and non-Cisco devices in a network.

---

### **⏰ 10.3 NTP (Network Time Protocol)**

* **Purpose:** Synchronizes time across all network devices to ensure consistent timestamps in logs and configurations.
* **Port:** UDP 123.

**🏗️ Hierarchy (Stratum Levels in Detail):**

| Stratum | Description                                                      |
| ------- | ---------------------------------------------------------------- |
| 0       | High-precision reference clock (e.g., atomic, GPS)               |
| 1       | Directly connected to stratum 0; provides time to lower strata   |
| 2       | Synchronizes with a stratum 1 device and can serve lower devices |
| 3–15    | Client devices synchronized through multiple hops                |
| 16      | Device is unsynchronized (invalid time)                          |

* **Explanation:**

  * The lower the stratum number, the more accurate the time source.
  * Devices form a hierarchical structure where each level relies on the one above it.
  * Stratum 2 devices can act as backup servers for redundancy.

**🧠 Commands:**

```bash
clock set 20:36:00 nov 15 2019     # Manual time set
show clock detail                   # Display current time source
ntp server <ip>                     # Set device to sync with NTP server
show ntp associations               # Show connected NTP peers
show ntp status                     # Verify time sync and stratum level
```

---

### **📊 10.4 SNMP (Simple Network Management Protocol)**

* **Purpose:** Enables centralized monitoring, configuration, and troubleshooting of network devices.
* **Ports:** UDP 161 (manager queries), UDP 162 (agent traps).

**⚙️ Components:**

* **SNMP Manager:** Installed on a Network Management System (NMS). Sends *get* and *set* requests.
* **SNMP Agent:** Runs on network devices and collects data.
* **MIB (Management Information Base):** Database storing variables (OIDs) describing device information.

**📜 Common Operations:**

| Operation    | Description                         |
| ------------ | ----------------------------------- |
| get          | Retrieve specific variable from MIB |
| get-next     | Retrieve next variable in a table   |
| get-bulk     | Retrieve large tables (SNMPv2+)     |
| set          | Modify variable value on a device   |
| get-response | Reply to manager queries            |

**🚨 Traps:** Unsolicited alerts sent from agents to managers (e.g., link down, high CPU usage).

**🔐 Versions:**

* **v1:** Basic, insecure (plain-text community string).
* **v2c:** Adds bulk transfer and better error reporting (still insecure).
* **v3:** Secure (authentication + encryption using MD5/SHA & AES/DES/3DES).

**🔑 Community Strings:**

* **ro (read-only):** Monitor only.
* **rw (read-write):** Allows configuration changes.

**🗂️ MIB Structure (OIDs):**
Cisco OID prefix: `1.3.6.1.4.1.9`

**🧰 Example Tools:**

* **snmpget / snmpwalk:** CLI SNMP utilities.
* **PRTG / SolarWinds:** GUI-based NMS for network visualization.

---

### **🧾 10.5 Syslog**

* **Purpose:** Collects system messages from devices for centralized monitoring and troubleshooting.
* **Protocol:** UDP port 514.

**🧩 Syslog Message Format:**

```
%facility-severity-MNEMONIC: description
Example: %LINK-3-UPDOWN: Interface Gi0/0, changed state to up
```

**📉 Severity Levels:**

| Level | Name          | Description                |
| ----- | ------------- | -------------------------- |
| 0     | Emergency     | System unusable            |
| 1     | Alert         | Immediate action required  |
| 2     | Critical      | Critical condition         |
| 3     | Error         | Error condition            |
| 4     | Warning       | Warning condition          |
| 5     | Notice        | Normal but important event |
| 6     | Informational | Informational messages     |
| 7     | Debug         | Debug-level messages       |

**🏷️ Syslog Facilities:** Identify the process that generated the message.

* **LINK:** Interface status changes.
* **OSPF:** Routing protocol events.
* **IP:** IP layer notifications.
* **SYS:** Operating system-level messages.
* **IF:** Interface state events.

**🗄️ Destinations:**

* **Console line:** Real-time display.
* **Logging buffer (RAM):** Temporary storage.
* **VTY terminals:** Remote viewing.
* **Syslog server:** Centralized archiving.

**🧠 Commands:**

```bash
service timestamps log datetime    # Enable timestamps
logging <ip>                       # Configure remote syslog server
show logging                       # View stored logs
logging console warnings           # Limit displayed messages
```

---

### **💾 10.6 Router and Switch File Maintenance**

* **Purpose:** Manage IOS files, configurations, and backups using flash, NVRAM, USB, or TFTP.

**📂 File System Commands:**

```bash
show file systems      # Display all available storage
cd <filesystem>        # Change current directory
pwd                    # Display present directory
dir                     # List contents in directory
```

**💽 Backup & Restore Methods:**

1. **🧾 Tera Term (Text File):**

   * Backup: Run `show running-config` and save the output.
   * Restore: Paste configuration or use *Send file*.

2. **📡 TFTP Server:**

```bash
copy running-config tftp
copy tftp running-config
```

3. **💿 USB Drive:**

```bash
copy running-config usbflash0:
copy usbflash0:/R1-Config running-config
```

**🔓 Password Recovery Steps:**

1. Enter ROMMON mode (`break` during boot).
2. `confreg 0x2142` → Ignore startup config.
3. `reset` → Reboot router.
4. `copy startup-config running-config` → Load saved config.
5. `enable secret <newpass>` → Set new password.
6. `config-register 0x2102` → Restore normal boot.
7. `copy running-config startup-config` → Save changes.

---

### **🧠 10.7 IOS Image Management**

* **Purpose:** Manage, back up, and restore Cisco IOS images to ensure routers and switches can recover quickly after failure or upgrades.
* **Best Practice:** Keep at least one backup image in a TFTP server or USB flash.

**🗂️ Backup IOS Image:**

```bash
show flash0:                 # Check available space and current image
copy flash: tftp:            # Backup image to TFTP server
```

**🔄 Restore IOS Image:**

```bash
copy tftp: flash:            # Download IOS from TFTP server
```

**🧩 Configure Boot Sequence:**

```bash
boot system flash0:<image_name>  # Define boot image
copy running-config startup-config
reload                            # Restart router
```

**💡 Additional Notes:**

* The router checks boot system commands in NVRAM in order.
* If no valid image is found, it loads the first valid IOS in flash.
* Use a TFTP server for centralized management of IOS versions.

---
### 📘 Additional Notes for NetAcad Questions
* SNMPv3 authenticates the source of management messages.
* SNMPv3 provides secure services for authentication, encryption, and access control.
* SNMPv1 and SNMPv2 do not support encrypted messages.
* Cisco IOS supports both SNMPv2c and SNMPv3.
* SNMPv2 introduced detailed error codes and bulk data retrieval.
* SNMPv1 and v2c use community-based (plaintext) security.
* SNMPv3 ensures integrity and message authentication for interoperability.
---

### **🏁 Final Word**

Network management integrates **discovery (CDP/LLDP)**, **time synchronization (NTP)**, **monitoring (SNMP)**, **logging (Syslog)**, and **maintenance (file & image management)**. Together, they provide **visibility, consistency, and control** across enterprise networks — ensuring minimal downtime and optimal reliability. 🌐
