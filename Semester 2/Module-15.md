# CCNA SRWE - Semester 2

## 🌐 Module 15: IP Static Routing

### 🎯 Objectives

The objective of this module is to configure IPv4 and IPv6 static routes, including default, floating, and host static routes.

---

## 🧭 15.1 Static Routes

### 🔹 Types of Static Routes

Both IPv4 and IPv6 support the following static routes:

* Standard static route
* Default static route
* Floating static route
* Summary static route

Command:

* **IPv4:** `ip route`
* **IPv6:** `ipv6 route`

### 🔹 Next-Hop Options

A static route can be defined by:

* **Next-hop route:** Uses next-hop IP only.
* **Directly connected static route:** Uses exit interface only.
* **Fully specified static route:** Uses both next-hop IP and exit interface.

### 🔹 IPv4 Static Route Command

```bash
Router(config)# ip route network-address subnet-mask { ip-address | exit-intf [ip-address] } [adminstrative distance 1-255]
```

### 🔹 IPv6 Static Route Command

```bash
Router(config)# ipv6 route ipv6-prefix/prefix-length { ipv6-address | exit-intf [ipv6-address] } [adminstrative distance 1-255]
```

---

## ⚙️ 15.2 Configure IP Static Routes

### 🔹 IPv4 Next-Hop Static Route

```bash
R1(config)# ip route 172.16.1.0 255.255.255.0 172.16.2.2
R1(config)# ip route 192.168.1.0 255.255.255.0 172.16.2.2
R1(config)# ip route 192.168.2.0 255.255.255.0 172.16.2.2
```

### 🔹 IPv6 Next-Hop Static Route

Before configuring IPv6 static routes, you must enable IPv6 routing with:

```bash
R1(config)# ipv6 unicast-routing
```

This enables the router to forward IPv6 packets. Without this, IPv6 routes will not function.

```bash
R1(config)# ipv6 route 2001:db8:acad:1::/64 2001:db8:acad:2::2
R1(config)# ipv6 route 2001:db8:cafe:1::/64 2001:db8:acad:2::2
R1(config)# ipv6 route 2001:db8:cafe:2::/64 2001:db8:acad:2::2
```

### 🔹 IPv4 Directly Connected Static Route

```bash
R1(config)# ip route 172.16.1.0 255.255.255.0 s0/1/0
R1(config)# ip route 192.168.1.0 255.255.255.0 s0/1/0
R1(config)# ip route 192.168.2.0 255.255.255.0 s0/1/0
```

⚠️ **Note:** Best practice is to use next-hop IP. Directly connected static routes should be used only on point-to-point interfaces.

### 🔹 IPv6 Directly Connected Static Route

```bash
R1(config)# ipv6 route 2001:db8:acad:1::/64 s0/1/0
R1(config)# ipv6 route 2001:db8:cafe:1::/64 s0/1/0
R1(config)# ipv6 route 2001:db8:cafe:2::/64 s0/1/0
```

### 🔹 IPv4 Fully Specified Static Route

Used when exit interface is multi-access. Requires next-hop IP + exit interface.

```bash
R1(config)# ip route 192.168.10.0 255.255.255.0 g0/0 172.16.2.2
```

### 🔹 IPv6 Fully Specified Static Route

Required when using **link-local addresses** as next hop.

```bash
R1(config)# ipv6 route 2001:db8:acad:10::/64 FE80::1 g0/0
```

### 🔹 Verify Static Routes

```bash
show ip route static
show ipv6 route static
show running-config | section ip route
ping
traceroute
```

---

## 🛣️ 15.3 Configure IP Default Static Routes

### 🔹 Default Static Route

* Matches **all packets** not in routing table.
* Used as **Gateway of Last Resort**.
* Works like: when a packet comes in and the router does not know its destination network (not found in the routing table), it sends it to this **next-hop IP** or **out of this exit interface**.
* `0.0.0.0` and `::/0` mean **“anything/any network”**.

### 🔹 Command Syntax

```bash
# IPv4
Router(config)# ip route 0.0.0.0 0.0.0.0 {ip-address | exit-intf}

# IPv6
Router(config)# ipv6 route ::/0 {ipv6-address | exit-intf}
```

### 🔹 Example

```bash
R1(config)# ip route 0.0.0.0 0.0.0.0 172.16.2.2
R1(config)# ipv6 route ::/0 2001:db8:acad:2::2
```

### 🔹 Verification

* **IPv4:** `show ip route static` → `*` indicates candidate default route.
* **IPv6:** `show ipv6 route static` → `::/0` used for default.

---

## 🌊 15.4 Configure Floating Static Routes

### 🔹 Floating Static Route

* Backup path used **only if primary route fails**.
* Configured with **higher administrative distance** than primary route.
* Default static route AD = **1**.

### 🔹 Command Example

```bash
# IPv4
R1(config)# ip route 0.0.0.0 0.0.0.0 172.16.2.2
R1(config)# ip route 0.0.0.0 0.0.0.0 10.10.10.2 5

# IPv6
R1(config)# ipv6 route ::/0 2001:db8:acad:2::2
R1(config)# ipv6 route ::/0 2001:db8:feed:10::2 5
```

---

## 🎯 15.5 Configure Static Host Routes

### 🔹 Host Route

* IPv4 host route: **/32** (255.255.255.255 mask).
* IPv6 host route: **/128**.

### 🔹 Automatically Installed Host Routes

* Cisco IOS installs host route (marked `L`) when IP is configured on router interface.

### 🔹 Static Host Route Example

```bash
Branch(config)# ip route 209.165.200.238 255.255.255.255 198.51.100.2
Branch(config)# ipv6 route 2001:db8:acad:2::238/128 2001:db8:acad:1::2
```

### 🔹 IPv6 Host Route with Link-Local

When using **link-local next-hop**, must specify exit interface:

```bash
Branch(config)# ipv6 route 2001:db8:acad:2::238/128 FE80::1 s0/0/0
```

---

## 📘 Additional Notes for NetAcad Questions

* Static routes can identify the next hop either by **next-hop IP address** or by specifying the **exit interface**. These are the two main methods used.
* In IPv4, using only the exit interface is most common on **point-to-point links**, since there is only one possible next hop.
* In IPv6, the destination network of a static route is always identified by an **IPv6 prefix and prefix length** (not a mask or wildcard).

---

## 🔑 Final Word

Static routing is a fundamental method for controlling network traffic. You must know how to configure next-hop, directly connected, and fully specified static routes for both IPv4 and IPv6. Default routes serve as the **gateway of last resort** (when no other match exists), while floating routes provide **reliable backup**. Finally, host routes enable traffic to be directed to a specific device. Mastering these ensures efficient traffic forwarding, network redundancy, and precise control over routing behavior.
