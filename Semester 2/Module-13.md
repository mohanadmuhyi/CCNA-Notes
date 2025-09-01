# CCNA SRWE - Semester 2

## 📡 Module 13: WLAN Configuration


### 🎯 Objectives

In this module, you will implement a WLAN using a wireless router and a Wireless LAN Controller (WLC). You will configure WLAN for remote site deployments, set up a basic WLAN on a WLC, configure a WPA2 Enterprise WLAN using VLAN, DHCP, and RADIUS, and troubleshoot common WLAN issues.

---

## 🏠 13.1 Remote Site WLAN Configuration

### 📶 Wireless Router Features

* Includes: switch ports for wired clients, WAN port, and wireless components.
* Provides **DHCP**, **NAT**, **QoS**, and **security (WPA2/WPA3, etc.)**.
* ISP typically configures cable/DSL modem.

### 🔑 Logging into Wireless Router

1. Connect to router (default IP from documentation or printed label).
2. Enter IP in browser → Login with default credentials (commonly `admin/admin`).
3. **Change default username & password** immediately.

### ⚙️ Basic Network Setup

1. Log in → Change admin password.
2. Change **DHCP IPv4 address pool**.
3. Renew client IP (`ipconfig /renew`).
4. Log in with new IP if router IP changed.

### 📡 Basic Wireless Setup

* Set **802.11 mode** (b/g/n/ac/ax).
* Configure **SSID**.
* Choose a **channel** (avoid overlap).
* Configure **security mode** (Open, WPA2-PSK, WPA2-Enterprise).
* Set **passphrase** if required.

### 🌐 Wireless Mesh Networks

* Extends coverage beyond 45m indoor / 90m outdoor.
* Add APs with same SSID & security but different channels.
* Modern routers allow mesh setup via **mobile apps**.

### 🔄 NAT for IPv4

* Router translates **private IPs → public IPs**.
* Uses **port numbers** to track multiple sessions.
* For IPv6: devices get unique global addresses (no NAT).

### 🚦 QoS (Quality of Service)

* Prioritize **voice/video** over email/web traffic.
* Some routers allow QoS by application or port.

### 📍 Port Forwarding & Triggering

* **Port forwarding:** permanently maps external port → internal device.
* **Port triggering:** temporarily opens inbound port when outbound request is made.

---

## 🖧 13.2 Configure a Basic WLAN on WLC

### 🛠 WLC Concepts

* Uses **Lightweight APs (LAPs)** that run **LWAPP/CAPWAP** protocol.
* LAPs auto-register with WLC (no manual config).
* WLC centrally manages APs, WLANs, and security.

### ⚙️ Steps to Configure WLAN on WLC

1. **Log in** to WLC GUI.
2. Go to **WLANs > Create New**.
3. **Name SSID** (e.g., `Wireless_LAN`).
4. **Enable WLAN** and assign interface (management or VLAN).
5. Configure **Security tab** → WPA2-PSK or WPA2-Enterprise.
6. **Verify WLAN** under WLANs menu.
7. Monitor clients under **Monitor > Clients**.

### 💻 Useful WLC Commands

* `show wlan summary` → list WLANs.
* `show client summary` → list connected clients.
* `show ap summary` → list APs connected to WLC.

---

## 🔐 13.3 Configure WPA2 Enterprise WLAN on WLC

### 📡 SNMP & RADIUS

* **SNMP**: WLC forwards logs (traps) to monitoring server.
* **RADIUS**: used for AAA (Authentication, Authorization, Accounting).
* Users authenticate via **username/password** checked by RADIUS.

### ⚙️ Configure RADIUS on WLC

1. Go to **SECURITY > RADIUS > Authentication**.
2. Add **RADIUS server IP** & shared secret.
3. Apply settings.

### 🌐 VLAN Interface Configuration

1. Go to **CONTROLLER > Interfaces > New**.
2. Name VLAN (e.g., `vlan5`), assign **VLAN ID**.
3. Assign **IP address**, subnet, default gateway.
4. Assign **DHCP server IP**.
5. Apply and verify with `show interface summary`.

### 📑 DHCP Scope (WLC Internal DHCP)

1. Go to **Internal DHCP Server > DHCP Scope > New**.
2. Name scope (e.g., `Wireless_Management`).
3. Configure IP pool, default router, and enable.

### 🔒 WPA2-Enterprise WLAN Setup

1. Create WLAN (SSID + Profile name).
2. Enable WLAN → bind to VLAN interface (e.g., vlan5).
3. Go to **Security tab** → ensure **AES + 802.1X** enabled.
4. Under **AAA Servers**, select configured **RADIUS server**.
5. Apply & verify WLAN appears under WLANs.

---

## 🛠 13.4 Troubleshoot WLAN Issues

### 🧭 Troubleshooting Methodology

1. Identify problem.
2. Establish theory of probable causes.
3. Test theory.
4. Implement solution.
5. Verify functionality.
6. Document findings.

### 📱 Wireless Client Issues

* Run `ipconfig` to check IP.
* Ping known host.
* Update/reload drivers.
* Check security settings (WPA2 vs WPA3 mismatch).
* Check if device is within coverage.
* Verify AP power & cabling.

### ⚡ Performance Issues

* Older devices (802.11b/g) slow network → upgrade clients.
* Split traffic between **2.4 GHz** and **5 GHz**.
* Place APs away from obstructions.
* Use **Wi-Fi Extenders** if range is weak.

### 🔄 Firmware Updates

* Routers/APs have **upgradable firmware**.
* WLC can push firmware updates to all APs.
* On Cisco 3504: **WIRELESS > Access Points > Global Configuration > AP Image Pre-download**.

---

## ✅ 13.5 Summary

Remote sites often use **SOHO routers** with DHCP, NAT, and WLAN security. You should always change default credentials. WLCs manage multiple LAPs using LWAPP/CAPWAP. WPA2-Enterprise uses RADIUS for AAA. Troubleshooting requires a systematic approach. Firmware updates and modern clients ensure the best WLAN performance.

---

## 🏁 Final Word

This module provides you with the knowledge to configure WLANs in both small office/home office and enterprise environments. Mastering WLC configuration, WPA2 Enterprise setup, and systematic troubleshooting ensures secure, reliable, and scalable wireless networks.
