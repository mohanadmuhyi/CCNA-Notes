# CCNA SRWE - Semester 2

## 📘 Module 11: Switch Security Configuration


### 🎯 Objectives

Configure switch security to **mitigate LAN attacks**.

---

## 🔐 11.1 Implement Port Security

### 📴 Secure Unused Ports

* Disable unused ports:

  ```
  Switch(config)# interface range type module/first – last
  Switch(config-if-range)# shutdown
  ```
* Reactivate when needed:

  ```
  Switch(config-if)# no shutdown
  ```

💡 *Reminder: Layer 2 is generally the weakest layer, so securing these ports is crucial.*

### 🧱 Mitigate MAC Address Table Attacks

* Attack: MAC address table overflow.
* Solution: **Port Security**

  * Limits number of MACs per port.
  * MAC addresses can be **static**, **dynamic**, or **sticky**.

### ⚙️ Enable Port Security

* Enable only on **access ports** (not dynamic auto):

  ```
  Switch(config-if)# switchport mode access
  Switch(config-if)# switchport port-security
  ```
* Verify:

  ```
  Switch# show port-security interface f0/1
  ```

### 📊 Limit & Learn MAC Addresses

1. **Max MACs allowed**:

   ```
   Switch(config-if)# switchport port-security maximum <value>
   ```
2. **Static MAC**:

   ```
   Switch(config-if)# switchport port-security mac-address <mac>
   ```

   ⚠️ *Static/manual configuration does not remember the MAC after a reload.*
3. **Dynamic MAC**:

   * Learned automatically by the switch.
   * Stored in RAM only (not saved in the running config).
   * Lost after reload; must be re-learned.
4. **Sticky MAC** (learns & saves):

   ```
   Switch(config-if)# switchport port-security mac-address sticky
   ```

   ✅ *Sticky mode remembers the MAC address even after a reload (if saved to NVRAM).*

* Save config to NVRAM to persist sticky MACs.
* Verify:

  ```
  Switch# show port-security address
  ```

### ⏳ Port Security Aging

* Automatically removes MACs.
* Command:

  ```
  Switch(config-if)# switchport port-security aging {static | time <mins> | type {absolute | inactivity}}
  ```
* **Absolute**: Secure addresses are removed after the set time, regardless of activity.
* **Inactivity**: Secure addresses are removed only if no traffic from that MAC occurs for the set period.

### 🚨 Violation Modes

* Command:

  ```
  Switch(config-if)# switchport port-security violation {shutdown | restrict | protect}
  ```
* **Shutdown** (default): Port disabled, syslog message, LED off.
* **Restrict**: Drops packets, logs, increments counter.
* **Protect**: Drops packets silently, no logs.

### ❌ Ports in Error-Disabled State

* If violation occurs → port shuts down.
* Recovery:

  ```
  Switch(config-if)# shutdown
  Switch(config-if)# no shutdown
  ```

### ✅ Verification Commands

* General:

  ```
  show port-security
  show port-security interface f0/x
  show port-security address
  show run
  ```

---

## 🟨 11.2 Mitigate VLAN Attacks

### 🚨 VLAN Attacks

* **DTP spoofing** – attacker forces trunking.
* **Rogue switch** – attacker connects switch.
* **Double-tagging** – attacker bypasses VLAN restrictions.

### 🔧 Mitigation Steps

1. Disable DTP:

   ```
   Switch(config-if)# switchport mode access
   ```
2. Disable unused ports + assign unused VLAN.
3. Manually enable trunks:

   ```
   Switch(config-if)# switchport mode trunk
   ```
4. Disable DTP on trunks:

   ```
   Switch(config-if)# switchport nonegotiate
   ```
5. Change native VLAN (not VLAN 1):

   ```
   Switch(config-if)# switchport trunk native vlan <id>
   ```

---

## 📡 11.3 Mitigate DHCP Attacks

### 🚨 DHCP Attacks

* **Starvation** – DoS by exhausting addresses.
* **Spoofing** – rogue DHCP server handing out bad IP configs.

### 🛡️ DHCP Snooping

* A security feature that allows a switch to act as a firewall for DHCP messages.
* Ensures DHCP messages only come from **trusted ports** (uplinks, servers).
* Blocks DHCP messages from **untrusted ports** (regular hosts).
* Builds a **DHCP binding table** mapping each client’s MAC ↔ IP.

### 🔧 Steps to Configure

1. Enable globally:

   ```
   Switch(config)# ip dhcp snooping
   ```
2. Enable on specific VLANs (only checks traffic within those VLANs):

   ```
   Switch(config)# ip dhcp snooping vlan <id>
   ```

   💡 *This means DHCP snooping will only inspect and control DHCP messages on the selected VLAN(s).*
3. Trust interfaces:

   ```
   Switch(config-if)# ip dhcp snooping trust
   ```
4. Limit rate on untrusted ports:

   ```
   Switch(config-if)# ip dhcp snooping limit rate <pps>
   ```

### ✅ Verification

```
show ip dhcp snooping
show ip dhcp snooping binding
```

---

## 📌 11.4 Mitigate ARP Attacks

### 🚨 ARP Attacks

* Threat actor sends fake ARP replies (poisoning).
* Redirects traffic through attacker.

### 🛡️ Dynamic ARP Inspection (DAI)

* Works together with DHCP snooping.
* Validates ARP messages against the **DHCP binding table**.
* Drops invalid ARP packets (e.g., if IP ↔ MAC mismatch).
* Can also validate source MAC, destination MAC, and IP fields in ARP.
* Protects against **ARP spoofing** and **ARP poisoning**.

### 🔧 Guidelines

* Enable DHCP snooping globally & on VLANs.
* Enable DAI on VLANs.
* Configure **uplinks as trusted**, access ports as **untrusted**.

### 🔧 Example Commands

Enable ARP inspection:

```
Switch(config)# ip arp inspection vlan <id>
```

Trust uplink:

```
Switch(config-if)# ip arp inspection trust
```

Validate ARP packets:

```
Switch(config)# ip arp inspection validate {src-mac | dst-mac | ip}
```

---

## 🌳 11.5 Mitigate STP Attacks

### 🚨 STP Attack

* Attacker spoofs BPDUs → tries to become root bridge.

### 🛡️ PortFast

* Brings access ports immediately to **forwarding** state.
* Should only be applied to **interfaces connected to end devices**.
* Commands:

  ```
  Switch(config-if)# spanning-tree portfast
  Switch(config)# spanning-tree portfast default   (global – applies to all access ports)
  ```

### 🛡️ BPDU Guard

* Shuts down port if it receives a BPDU.
* Should only be applied to **interfaces connected to end devices**.
* Commands:

  ```
  Switch(config-if)# spanning-tree bpduguard enable
  Switch(config)# spanning-tree portfast bpduguard default   (global – applies to all access ports)
  ```
* If triggered, port goes into **err-disabled** state.
* Recovery (automatic):

  ```
  Switch(config)# errdisable recovery cause bpduguard
  ```

### ✅ Verification

```
show running-config | begin span
show spanning-tree summary
show spanning-tree interface f0/x detail
```

---

## 📝 Summary

* Layer 2 is the weakest and must be secured first.
* Secure unused ports before deployment.
* Port Security prevents MAC flooding; supports static, dynamic, sticky.
* Static mode doesn’t remember MAC after reload, but Sticky does (if saved).
* Port security aging supports **absolute** and **inactivity** timers.
* Violation modes: shutdown (default), restrict, protect.
* VLAN hopping mitigated by disabling DTP, using unused VLANs, native VLAN ≠ 1.
* DHCP Snooping validates DHCP messages, builds a binding table, and can be enabled on specific VLANs.
* ARP attacks mitigated with **DAI**, which checks ARP packets against DHCP bindings.
* STP attacks mitigated with **PortFast + BPDU Guard**. Both should be applied to **end-device interfaces**. Global commands apply to all access ports.
* BPDU Guard shutdown can be auto-recovered with `errdisable recovery cause bpduguard`.

---

## 🔑 Final Word
Always remember that switch security starts at **Layer 2**. Most LAN attacks exploit weak defaults (DTP, native VLAN 1, DHCP spoofing, ARP poisoning, STP manipulation). By combining **Port Security, VLAN security practices, DHCP Snooping, DAI, and STP protections**, you create a strong first line of defense against intruders inside the LAN.
