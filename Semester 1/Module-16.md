# 📘 CCNA - Module 16: Network Security Fundamentals

## 🔟 Objective

Learn how to recognize threats and vulnerabilities, understand common types of attacks and malware, and apply techniques and configurations to secure network devices.

---

## 16.1 🛡️ Security Threats and Vulnerabilities

### ⚠️ Types of Threats

Once a threat actor gains access to a network, four primary threats can arise:

* **Information Theft**: Unauthorized access to confidential data.
* **Data Loss and Manipulation**: Deletion or alteration of data.
* **Identity Theft**: Impersonating a user.
* **Disruption of Service**: Interrupting network availability.

> 💡 Threat actors gain access through software exploits, password guessing, or physical access.

### ⚠️ Types of Vulnerabilities

1. **Technological**: Weak protocols, OS flaws, hardware bugs.
2. **Configuration**: Default settings, weak passwords, poor setup.
3. **Policy-Based**: Poor enforcement, lack of backup plans.

> 🔹 Vulnerabilities aren't just digital. **Physical vulnerabilities** (e.g., access to routers/switches) can be just as critical.

### 🏢 Physical Threats

1. **Hardware**: Tampering, damage to devices.
2. **Environmental**: Heat, humidity, water damage.
3. **Electrical**: Spikes, brownouts, outages.
4. **Maintenance**: ESD, cable mishandling, poor labeling.

---

## 16.2 🚫 Network Attacks

### 💩 Malware Types

* **Virus**: Needs a host file; spreads by user action.
* **Worm**: Self-replicating; spreads independently.
* **Trojan Horse**: Disguised as legitimate software.
* **Ransomware**: Locks data and demands ransom.
* **Adware**: Injects unwanted advertisements.

### Network Attack Categories

* **Reconnaissance**: Scanning, ping sweeps, nslookup.
* **Access Attacks**: Unauthorized access using:

  * Brute force, sniffers, trust exploitation
  * **Man-in-the-middle (MITM)** attacks
* **Denial of Service (DoS)**:

  * Overloads system resources.
  * **DDoS**: Uses botnets (**zombies**) controlled by a **Command and Control (CnC)** system.

---

## 16.3 🔒 Network Attack Mitigations

### 🔹 Defense in Depth

A **layered security** model combining tools and policies:

* **VPN**: Secure tunneling between remote sites (e.g., two servers).
* **Firewall**: Filters traffic between zones.
* **IPS** (Intrusion Prevention System):

  * Monitors traffic in real-time.
  * Uses **digital signatures** to detect attacks.
* **ESA/WSA** (Email/Web Security Appliances):

  * Blocks malicious emails and web content.
* **AAA Server**: Manages authentication, authorization, and accounting.

### 📂 Backups

* Perform regular full/partial backups.
* Store securely offsite.
* Encrypt and validate backups.
* Use strong passwords to protect and restore backups (🔐 security consideration).

### ↩️ Updates & Patching

* Critical to mitigate worm/malware threats.
* Automate patching on all endpoints.

### 🏡 Endpoint Security

* Includes PCs, phones, servers.
* Use antivirus, firewalls, and enforce security policies.
* Network Access Control can limit device permissions.
* Ensure endpoint configurations prevent unauthorized access and data leaks.

### 🔥 Firewall Types

* **Packet Filtering**: Filters by IP/MAC.
* **Application Filtering**: Filters by app/port.
* **URL Filtering**: Blocks sites.
* **Stateful Packet Inspection (SPI)**: Allows only solicited traffic.

### 🔦 Firewall Zones & Priorities

* **Inside Zone**: Trusted LAN.
* **DMZ**: Public-facing servers.
* **Outside Zone**: Untrusted internet.
* Rules and priorities define what traffic is allowed between zones.

---

## 16.4 🔐 Device Security

### ⚖️ Cisco AutoSecure

* Automatically secures basic device settings.
* Disables unused services, enables secure defaults.

### 🔑 Password Security

* Use strong, long passphrases.
* Avoid personal data.
* Regularly change passwords.

### 🔏 Additional Measures

```plaintext
R1(config)# service password-encryption
R1(config)# security passwords min-length 10
R1(config)# login block-for 60 attempts 5 within 30
R1(config)# exec-timeout 5 0
```

### 🛡️ Enable SSH Access (6 Steps)

```plaintext
R1(config)# hostname R1
R1(config)# ip domain-name mydomain.com
R1(config)# crypto key generate rsa general-keys modulus 1024
R1(config)# username admin password cisco
R1(config)# line vty 0 4
R1(config-line)# login local
R1(config-line)# transport input ssh
```

> 🔹 SSH encrypts remote CLI access. Use it instead of Telnet (plaintext).

### 💡 Disable Unused Services

```plaintext
R1# show control-plane host open-ports
```

Or for newer IOS-XE:

```plaintext
R1# show ip ports all
```

---


## 📊 Additional Notes for NetAcad Questions

### 🧨 Threat Types

* **Data loss or manipulation**: Virus reformatting drives, altering records.
* **Information theft**: Stealing research data or user databases.
* **Identity theft**: Illegal purchases, impersonation.
* **Disruption of service**: Network overload, DoS.

### 🧪 Attack Scenarios

* **DoS**: Web server overwhelmed with malformed requests.
* **Access**: Weak FTP password leads to data leak.
* **Malware**: Fake cleanup tool damages system, infected USB.
* **Reconnaissance**: Scanning ports/IPs for vulnerability.

### 🔐 Security Devices

* **Firewall**: Controls traffic between networks.
* **AAA Server**: Authenticates and authorizes management access.
* **ESA/WSA**: Filters email and web-based threats.

### 📦 Backup Policies

* **Security**: Use encryption and strong passwords for data protection.

### 🌐 DMZ Zone

* Hosts publicly accessible services (e.g., websites).

### 🛡️ Endpoint Protection

* Use **antivirus software**, firewalls, and strong policies.

---

## 🔍 Final Word

Securing your network is a shared responsibility. Know the vulnerabilities (physical, software, policy), identify threats (malware, attacks), and apply the right tools (firewalls, IPS, SSH, backups, policies). Configure your routers and switches properly, keep your systems patched, and train users to reduce risk. With layered defense and informed practices, you’re building a safer and more resilient network.
