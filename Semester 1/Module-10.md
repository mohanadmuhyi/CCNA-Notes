# 📘 CCNA - Module 10: Basic Router Configuration

## 🌟 Objective

Learn how to configure basic settings on routers and switches, including hostnames, passwords, interfaces, default gateways, and static routes.

---

## 10.1 🔌 Initial Device Configuration

### 💾 Access Methods

* **Console**: Direct connection to the device (via console cable).
* **Telnet / SSH**: Remote access over the network (requires IP configuration).

### 🛠️ Basic Device Setup

```plaintext
Router> enable                 ← enter privileged EXEC mode
Router# configure terminal     ← enter global config mode
Router(config)# hostname R1    ← set hostname to R1
R1(config)# exit               ← exit config mode
```

---

## 10.2 🔐 Secure Access

### Set Console and Enable Passwords

```plaintext
R1(config)# line console 0
R1(config-line)# password cisco
R1(config-line)# login
R1(config-line)# exit

R1(config)# enable secret class
```

### Secure vTY Lines for Telnet/SSH

```plaintext
R1(config)# line vty 0 4
R1(config-line)# password cisco
R1(config-line)# login
R1(config-line)# transport input ssh
R1(config-line)# exit
```

### Encrypt All Passwords

```plaintext
R1(config)# service password-encryption
```

---

## 10.3 🌐 Interface Configuration

### Assign IP Address and Bring Interface Up

```plaintext
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit
```

> 🔎 Always specify the IP version:

```plaintext
R1(config-if)# ipv6 address 2001:db8::1/64
```

### View Interface Status

```plaintext
R1# show ip interface brief
R1# show ipv6 interface brief
R1# show interfaces
R1# show ip interface
R1# show ipv6 interface
```

---

## 10.4 🚪 Switch IP Configuration

Switches operate at Layer 2 and need a **management IP address** to be accessed remotely.

### Configure VLAN Interface and Default Gateway

```plaintext
Switch(config)# interface vlan 1
Switch(config-if)# ip address 192.168.1.2 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if)# exit

Switch(config)# ip default-gateway 192.168.1.1
```

> 💡 The default gateway is required for the switch to communicate outside its own subnet.

---

## 10.5 🚏 Basic Static Routing

### 📌 Configure a Static Route

```plaintext
R1(config)# ip route [DESTINATION_NETWORK] [SUBNET_MASK] [NEXT_HOP_IP]
```

**Example:**

```plaintext
R1(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

> 🔎 You can also configure a static route using the **exit interface** instead of the next hop IP:

```plaintext
R1(config)# ip route 192.168.2.0 255.255.255.0 gigabitEthernet 0/1
```

> ✨ This approach is particularly useful when the next-hop IP is not yet reachable or known.

### 🚪 Configure a Default Route (Gateway of Last Resort)

```plaintext
R1(config)# ip route 0.0.0.0 0.0.0.0 [NEXT_HOP_IP]
```

**Example:**

```plaintext
R1(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.1
```

### 🔍 View the Routing Table

```plaintext
R1# show ip route
R1# show ipv6 route
```

> Codes: `C` - connected, `S` - static, `S*` - default, `O` - OSPF, `R` - RIP

---

## 🔍 Final Word

A strong foundation in configuring routers and switches is essential for any network technician. From setting hostnames to establishing remote access and configuring static routes, these skills enable the deployment and management of functional networks. Master these commands and understand when and why to use them — it’s your first step toward real-world networking proficiency.

---
