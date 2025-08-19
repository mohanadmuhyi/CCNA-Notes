# CCNA SRWE - Semester 2
## 🌐 Module 6: EtherChannel

### 🎯 Objectives

Troubleshoot EtherChannel on switched links.

---

## 🔹 6.1 EtherChannel Operation

### 🔗 Link Aggregation

* A single link may not provide enough bandwidth or redundancy.
* STP blocks redundant links to prevent loops.
* **EtherChannel**: Groups multiple physical Ethernet links into one logical link.

  * Provides **fault tolerance, load sharing, increased bandwidth, and redundancy**.
  * Seen by STP as **one logical link**, preventing unnecessary blocking.
  * Unlike STP backup links, EtherChannel uses **both links actively** (not just one active and one standby).
  * Provides **flow control and backup**: if one link fails, traffic continues over the remaining links.

### 🧩 EtherChannel

* Developed by Cisco for LAN switch-to-switch aggregation.
* Virtual interface created = **Port Channel**.
* Multiple physical interfaces are bundled into a single **port-channel interface**.
* The bundle looks like **one link** to upper layers.

### ✅ Advantages of EtherChannel

* Configuration consistency (configured on the port-channel, not each port).
* Uses existing ports (no need for expensive upgrades).
* Load balancing across links.
* STP sees it as one link (blocks at the bundle level, not individual ports).
* Redundancy: loss of one link doesn’t change topology.

### ⚠️ Implementation Restrictions

* Cannot mix interface types (FE + GE not allowed).
* Up to **8 ports** per EtherChannel.
* Catalyst 2960 supports up to **6 EtherChannels** (maximum per switch).
* EtherChannel group number must match on both switches and cannot already be in use on either switch.
* Configuration must match on both sides:

  * Speed & duplex
  * VLANs & trunking mode
  * Layer 2 mode (not routed ports)
* Config applied to **port-channel interface** affects all members.
* Ports must be **no shutdown** before EtherChannel can form.

### 🔄 Auto-Negotiation Protocols

* **PAgP (Cisco proprietary)**
* **LACP (IEEE 802.3ad, vendor-neutral)**
* Can also configure **static EtherChannel** (no negotiation).

### 🟦 PAgP (Port Aggregation Protocol)

* Cisco proprietary.
* Sends PAgP packets every 30s.
* Checks configuration consistency.
* **Modes:**

  * `on`: Forces channel, no negotiation (works only if both sides are on).
  * `desirable`: Actively negotiates (sends PAgP packets).
  * `auto`: Passive (responds, but doesn’t initiate).
### PAgP Mode Compatibility

| S1 Mode   | S2 Mode   | Channel Forms? |
| --------- | --------- | -------------- |
| On        | On        | ✅ Yes          |
| On        | Desirable | ❌ No           |
| On        | Auto      | ❌ No           |
| Desirable | Desirable | ✅ Yes          |
| Desirable | Auto      | ✅ Yes          |
| Auto      | Auto      | ❌ No           |

**Command Example (PAgP):**

```bash
Switch(config)# interface range f0/1 - 2
Switch(config-if-range)# channel-group 1 mode desirable
```

### 🟩 LACP (Link Aggregation Control Protocol)

* IEEE 802.3ad standard (multi-vendor).
* Similar to PAgP.
* **Modes:**

  * `on`: Forces channel, no negotiation.
  * `active`: Actively negotiates (sends LACP packets).
  * `passive`: Passive (responds, but doesn’t initiate).
### LACP Mode Compatibility

| S1 Mode | S2 Mode | Channel Forms? |
| ------- | ------- | -------------- |
| On      | On      | ✅ Yes          |
| On      | Active  | ❌ No           |
| On      | Passive | ❌ No           |
| Active  | Active  | ✅ Yes          |
| Active  | Passive | ✅ Yes          |
| Passive | Passive | ❌ No           |


**Command Example (LACP):**

```bash
Switch(config)# interface range f0/1 - 2
Switch(config-if-range)# channel-group 1 mode active
```

---

## 🔹 6.2 Configure EtherChannel

### 📝 Configuration Guidelines

* All interfaces must:

  * Support EtherChannel.
  * Use same speed & duplex.
  * Be in the same VLAN or trunk.
  * Have same allowed VLANs (for trunk).
* Configurations should be applied on **port-channel interface**, not individual ports.
* Port channel can be **access, trunk, or routed**.
* Remember: ports must be **no shutdown** before EtherChannel works.

### ⚙️ LACP Configuration Example

1. Select interfaces:

   ```bash
   Switch(config)# interface range f0/1 - 2
   ```
2. Add to EtherChannel group:

   ```bash
   Switch(config-if-range)# channel-group 1 mode active
   ```
3. Configure the port-channel interface:

   ```bash
   Switch(config)# interface port-channel 1
   Switch(config-if)# switchport mode trunk
   Switch(config-if)# switchport trunk allowed vlan 1,2,20
   ```

---

## 🔹 6.3 Verify and Troubleshoot EtherChannel

### 🔍 Verification Commands

* General port-channel status:

  ```bash
  show interfaces port-channel
  ```
* Summary of all EtherChannels:

  ```bash
  show etherchannel summary
  ```
* Specific port-channel details:

  ```bash
  show etherchannel port-channel
  ```
* Physical interface roles:

  ```bash
  show interfaces etherchannel
  ```

### ⚠️ Common Issues

* Ports not in the same VLAN or trunk.
* Different native VLANs.
* Trunking misconfigured on some ports.
* VLAN ranges don’t match.
* PAgP/LACP mode mismatch.

### 🛠️ Troubleshooting Steps

1. `show etherchannel summary` → Check if channel is down.
2. `show run | begin interface port-channel` → Inspect config.
3. Correct misconfiguration (e.g., change mode to desirable/active).
4. Verify again with `show etherchannel summary`.

⚡ Note: **Order matters** when configuring/removing EtherChannel due to STP interaction. Wrong order may cause `err-disabled` state.

---


## 🔹 PAgP vs LACP Comparison Table

| Feature             | PAgP                     | LACP                          |
| ------------------- | ------------------------ | ----------------------------- |
| Standard            | Cisco proprietary        | IEEE 802.3ad (open standard)  |
| Vendor Support      | Cisco devices only       | Multi-vendor support          |
| Modes               | on, desirable, auto      | on, active, passive           |
| Negotiation Packets | PAgP packets (every 30s) | LACP packets                  |
| Compatibility       | Cisco-only environments  | Cisco + other vendor switches |
| Typical Use         | Cisco-only networks      | Mixed-vendor environments     |

---

## 📌 Key Takeaways

* **EtherChannel = multiple links → one logical link.**
* Prevents STP from blocking redundant links.
* **Protocols:** PAgP (Cisco) & LACP (IEEE).
* Always configure **speed, duplex, VLANs, and trunking consistently**.
* Ports must be **no shutdown** for EtherChannel to form.
* EtherChannel provides **flow control and redundancy** (if one link fails, traffic keeps flowing).
* Use `show` commands to verify & troubleshoot.
* Misconfigurations = most common issue.

---

## 📘 Additional Notes for NetAcad Questions

* **Benefits of EtherChannel:** Fault-tolerance, load sharing, increased bandwidth, link redundancy.   
* Only LACP is IEEE standard, PAgP is Cisco proprietary.  
* **Initiating Mode in PAgP:** Desirable initiates negotiation.

---

## 🏁 Final Word

EtherChannel is essential for combining multiple physical links into one logical, high-speed, redundant path. It prevents STP from blocking necessary bandwidth, ensures resiliency if a link fails, and provides scalability. Understanding its operation, modes, configuration, and troubleshooting is key for efficient and robust network design.