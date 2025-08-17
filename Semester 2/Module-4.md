# CCNA SRWE - Semester 2

## 🖧 Module 4: Inter-VLAN Routing

### 🎯 Objectives

This module focuses on understanding and configuring **Inter-VLAN Routing**, implementing **Router-on-a-Stick**, configuring **Layer 3 Switch Inter-VLAN Routing**, and troubleshooting common **Inter-VLAN Routing issues**.

---

## 🔹 4.1 Inter-VLAN Routing Operation

### 📌 What is Inter-VLAN Routing?

* VLANs separate Layer 2 networks.
* Devices in different VLANs **cannot communicate** without a router or Layer 3 switch.
* **Inter-VLAN routing** forwards traffic between VLANs.

**Options for Inter-VLAN Routing:**

1. **Legacy Inter-VLAN Routing**

   * Router with multiple physical interfaces.
   * Each interface connects to a switch port in a VLAN.
   * ❌ Not scalable (requires one interface per VLAN).

2. **Router-on-a-Stick**

   * Single router interface handles multiple VLANs via **subinterfaces**.
   * Configured with **802.1Q trunking**.
   * ✅ Good for small/medium networks.
   * ❌ Doesn’t scale beyond \~50 VLANs.
   * 📝 Note: Router-on-a-stick creates a **sub virtual interface** for each VLAN using only one cable between the router and switch. You can’t make the router port a trunk, so instead you configure subinterfaces with the command `encapsulation dot1Q vlan-id`. Also, you **cannot create subinterfaces on a shutdown interface**.

3. **Layer 3 Switch with SVIs**

   * Modern scalable solution.
   * Uses **Switched Virtual Interfaces (SVIs)**.
   * ✅ Faster, hardware-based routing.
   * ✅ Supports EtherChannel for bandwidth.
   * ❌ More expensive.
   * 📝 Note: Difference between **Layer 2** and **Layer 3** switches → Layer 2 switches only forward frames and can have **one SVI (management only)**, while Layer 3 switches can create **multiple SVIs** and perform routing.

---

## 🔹 4.2 Router-on-a-Stick Inter-VLAN Routing

### ⚙️ Steps to Configure

#### 🖥 Switch Configuration

1. **Create VLANs**

```
Switch(config)# vlan 10
Switch(config-vlan)# name Staff
Switch(config)# vlan 20
Switch(config-vlan)# name Students
Switch(config)# vlan 99
Switch(config-vlan)# name Management
```

📝 Note: You can create a VLAN without naming it. If you assign an interface to a non-existent VLAN, it will be created automatically.

2. **Configure Management Interface**

```
Switch(config)# interface vlan 99
Switch(config-if)# ip address 192.168.99.2 255.255.255.0
Switch(config-if)# no shutdown
```

3. **Assign Access Ports**

```
Switch(config)# interface f0/6
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
```

4. **Configure Trunking**

```
Switch(config)# interface f0/1
Switch(config-if)# switchport mode trunk
```

📝 Note: Trunk ports enhance the **802.1Q protocol**, which tags packets with VLAN IDs and allows all VLANs to pass.

#### 📡 Router Configuration (Subinterfaces)

```
R1(config)# interface g0/0/1.10
R1(config-subif)# encapsulation dot1q 10
R1(config-subif)# ip address 192.168.10.1 255.255.255.0

R1(config)# interface g0/0/1.20
R1(config-subif)# encapsulation dot1q 20
R1(config-subif)# ip address 192.168.20.1 255.255.255.0

R1(config)# interface g0/0/1.99
R1(config-subif)# encapsulation dot1q 99
R1(config-subif)# ip address 192.168.99.1 255.255.255.0

R1(config)# interface g0/0/1
R1(config-if)# no shutdown
```

📝 Note: If you want to change the native VLAN between router and switch, add `native` to the `encapsulation dot1q -vlan id-` on the router and configure the same native VLAN on the switch trunk.

### ✅ Verification Commands

From PCs:

* `ipconfig`
* `ping <other VLAN host>`

On Router/Switch:

```
show ip route
show ip interface brief
show interfaces trunk
```

---

## 🔹 4.3 Inter-VLAN Routing using Layer 3 Switches

### ⚙️ Steps to Configure

**Combined Configuration Steps (with commands and notes):**

1. **Make Port Routable (Optional)**

* Use if connecting to another Layer 3 device.

```
Switch(config)# interface g0/1
Switch(config-if)# no switchport
```

2. **Create VLANs**

```
Switch(config)# vlan 10
Switch(config)# vlan 20
```

3. **Create SVIs (Default Gateways for PCs)**

```
Switch(config)# interface vlan 10
Switch(config-if)# ip address 192.168.10.1 255.255.255.0
Switch(config-if)# no shutdown

Switch(config)# interface vlan 20
Switch(config-if)# ip address 192.168.20.1 255.255.255.0
Switch(config-if)# no shutdown
```

4. **Assign Access Ports**

```
Switch(config)# interface f0/6
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
```

5. **Configure Trunk Ports (if inter-switch or router connection)**

```
Switch(config)# interface f0/1
Switch(config-if)# switchport mode trunk
```

6. **Encapsulation dot1Q (if trunking is used)**

* Automatically handled on switches, but ensure native VLAN matches across links.

7. **Enable IP Routing**

```
Switch(config)# ip routing
```

📝 Notes:

* Steps **1, 5, and 6** are **optional**, only needed if the Layer 3 switch connects to other L3 devices or if trunking is configured.
* Steps **2, 3, 4, 7** are **mandatory** for enabling inter-VLAN routing on the Layer 3 switch.
* In **Layer 3 switches** you can create **multiple SVIs**, but in **Layer 2 switches** only one (management).

### 🌐 Routed Port Configuration (Optional)

* Use this if connecting the Layer 3 switch to a router or another L3 device. Optional unless external routing is required.

```
Switch(config)# interface g0/1
Switch(config-if)# no switchport
Switch(config-if)# ip address 10.10.10.2 255.255.255.0
Switch(config-if)# no shutdown
```

---

## 🔹 4.4 Troubleshooting Inter-VLAN Routing

### 🔎 Common Issues & Fixes

1. **Missing VLANs**

   * Create VLANs again.
   * Verify:

     ```
     show vlan brief
     show interfaces switchport
     ping
     ```

2. **Switch Trunk Port Issues**

   * Ensure port is trunk-enabled.
   * Verify:

     ```
     show interface trunk
     show running-config
     ```

3. **Switch Access Port Issues**

   * Wrong VLAN assignment or disabled port.
   * Verify:

     ```
     show vlan brief
     show interfaces switchport
     ```

4. **Router Misconfiguration**

   * Wrong subinterface or IP.
   * Verify:

     ```
     show ip interface brief
     show interfaces | include Gig|802.1Q
     ```

---

## 📝 Key Takeaways

* Inter-VLAN routing connects VLANs using **Router-on-a-Stick** or **Layer 3 Switch SVIs**.
* **Router-on-a-Stick** uses subinterfaces with 802.1Q trunking.
* **Layer 3 switches** provide hardware-based, faster routing.
* Troubleshooting focuses on VLAN existence, trunk ports, access ports, and subinterface configs.

---

## 📘 Additional Notes for NetAcad Questions

* To verify that subinterfaces are in the routing table, use: `show ip route`.
* To check the list of VLANs and their assigned ports, use: `show vlan`.
* To verify the status of an access port and its access VLAN, use: `show interfaces interface-id switchport`.
* To verify the status and IP address of all interfaces in a condensed format, use: `show ip interface brief`.

---

## ✅ Final Words

This module provided a complete understanding of **Inter-VLAN Routing**, covering Router-on-a-Stick, Layer 3 Switch routing with SVIs, configuration commands, troubleshooting methods, and key verification commands. These skills ensure smooth communication between VLANs and prepare you to diagnose and fix common issues effectively.
