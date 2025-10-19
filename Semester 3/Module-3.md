# CCNA ENSA - Semester 3
## 🛡️ Module 3: Network Security Concepts


### 🎯 Objectives

By the end of this module you will be able to explain how vulnerabilities, threats, and exploits can be mitigated to enhance network security. You will describe the current state of cybersecurity and common vectors of data loss. You will identify and classify threat actors and the tools they use. You will describe common malware types and common network attacks, and explain how IP/TCP/UDP vulnerabilities and IP services can be exploited. You will list and explain network security best practices and common cryptographic processes used to protect data in transit.

---

# 🌍 3.1 Current State of Cybersecurity

### 🧭 Current State of Affairs

Cyber criminals possess advanced tools and skills able to disrupt critical infrastructure and business operations. Threat techniques continuously evolve — defenders must stay proactive.

**Key terms (quick reference):**

* **Assets:** anything valuable to an organization (people, devices, data, services).
* **Vulnerability:** weakness in a design, implementation, or configuration that can be exploited.
* **Threat:** potential danger to assets.
* **Exploit:** the technique or code that takes advantage of a vulnerability.
* **Mitigation:** action or control that reduces likelihood/severity of an attack.
* **Risk:** probability × impact: chance a threat will exploit a vulnerability and cause harm.

### ⚔️ Attack Vectors

An *attack vector* is a path used to gain unauthorized access. Vectors may be internal (insider threats, compromised devices) or external (internet, removable media, cloud services). Internal threats can be especially damaging due to physical and logical access.

### 💾 Data Loss (DLP) — Vectors & Consequences

**Consequences:** reputational damage, revenue loss, litigation, regulatory fines, operational cost.

**Common DLP vectors:**

* Email / social messaging interception
* Unencrypted devices (laptops, phones, drives)
* Cloud storage misconfiguration
* Removable media (USB exfiltration or loss)
* Hard-copy leakage (documents not shredded)
* Weak access controls/passwords

**Best practice:** combine technical controls (encryption, DLP tooling, access controls), operational policies, and user education.

---

# 🧑‍💻 3.2 Threat Actors

Threat actors vary by motivation, skill, and resources. Knowing the actor helps prioritize defenses.

**Categories:**

* **🤍 White Hat:** ethical testers who disclose vulnerabilities responsibly.
* **⚪ Gray Hat:** operate in ethical/ethical-grey zones; may exploit then notify.
* **🖤 Black Hat:** malicious criminals acting for personal gain or sabotage.

**Modern roles & groups:**

* **👶 Script kiddies:** inexperienced attackers using off-the-shelf tools.
* **💰 Vulnerability brokers:** find and sell zero-days.
* **🎭 Hacktivists:** politically/ideologically motivated (e.g., Anonymous).
* **💻 Cybercriminals:** organized crime networks selling malware, botnets, PII.
* **🏴‍☠️ State-sponsored:** well-resourced groups targeting national or strategic assets (e.g., Stuxnet example).

**Takeaway:** categorize threats to design controls that match capability and intent.

---

# 🧰 3.3 Threat Actor Tools & Attack Types

### 🔧 Tool Families

* **🔑 Password crackers:** John the Ripper, THC Hydra, Ophcrack.
* **📶 Wireless tools:** Aircrack-ng, Kismet.
* **🌐 Network scanners:** Nmap, Angry IP Scanner.
* **🧪 Packet crafting:** Scapy, Hping, Netcat.
* **👀 Packet sniffers:** Wireshark, Tcpdump, Ettercap.
* **🩻 Rootkit detectors / integrity checkers:** AIDE, Tripwire alternatives.
* **🧨 Fuzzers:** Skipfish, W3af.
* **🕵️ Forensics:** Sleuth Kit, EnCase, Maltego.
* **💥 Exploit frameworks:** Metasploit, sqlmap.
* **🛡️ Vulnerability scanners:** Nessus, OpenVAS.
* **🐧 Hacking OSes:** Kali Linux, BackBox.

> ⚠️ Note: Many tools are dual-use (white/black hat). Education and sandboxing are critical — unethical use is illegal.

### ⚔️ Attack Type Taxonomy

* **👂 Eavesdropping / Sniffing:** intercepting traffic to gather secrets.
* **📝 Data modification:** altering packets in transit.
* **🎭 IP spoofing:** forging source IP to masquerade or enable amplification attacks.
* **🔐 Password attacks:** brute force, dictionary, credential stuffing.
* **💣 Denial of Service (DoS/DDoS):** overwhelm resources or crash targets.
* **🧍‍♂️ Man-in-the-Middle (MITM):** intercept and potentially modify communications.
* **🔑 Compromised-key attacks:** stolen private keys allow transparent decryption.
* **🕵️ Sniffer attacks:** unencrypted traffic gives full packet visibility.

---

# 🦠 3.4 Malware — Types & Behavior

**Overview:** malware is software intended to harm or gain unauthorized access. End devices (workstations, servers, mobile) are primary targets.

**Major families & characteristics:**

* **🧬 Virus:** requires human action to propagate; attaches to files; can corrupt, exfiltrate, or spread via email.

  * Subtypes: boot sector, firmware, macro, program, script viruses.
* **🐍 Worm:** self-propagating via network vulnerabilities; aims to spread and disrupt.
* **🐴 Trojan horse:** appears useful but contains malicious payload (remote access Trojans, data-sending, destructive, proxy, FTP Trojans, AV disablers, DoS helpers, keyloggers).
* **📢 Adware / Spyware:** unwanted ads; spyware collects user info covertly.
* **💸 Ransomware:** encrypts data and demands payment for the key — backup strategy is essential.
* **👾 Rootkits:** hide presence and escalate privileges, often hard to remove.

**Mitigations:** up-to-date AV/EDR, application whitelisting, least privilege, patching, backups, user training.

---

# 🕵️‍♂️ 3.5 Common Network Attacks (Recon, Access, DoS)

### 🔍 Reconnaissance

Recon gathers information (domain, public IPs, services) before an attack. Typical steps:

1. Information query (Google, WHOIS, org site)
2. Ping sweep to find live hosts
3. Port scan to enumerate services (Nmap)
4. Vulnerability scanning (Nessus, OpenVAS)
5. Exploitation trials (Metasploit)

**Mitigation:** limit public information, firewall filtering, IDS/IPS monitoring, honeypots, rate limiting.

### 🔑 Access Attacks

Aim to gain unauthorized access: password cracking, spoofing (IP/MAC/DHCP), buffer overflows, port redirections, trust exploits. Access attacks are used to obtain credentials or persistent footholds.

### 🧠 Social Engineering

Human-targeted attacks to trick people into revealing secrets or performing actions:

* Pretexting, phishing, spear phishing, baiting, quid pro quo, impersonation, tailgating, shoulder surfing, dumpster diving.

**Mitigation:** security awareness training, verification procedures, least privilege, MFA, physical access controls.

### 💣 Denial of Service (DoS / DDoS)

* **Volume attacks:** flood bandwidth or resources
* **Malformed packets:** exploit parsing flaws to crash services
* **DDoS:** used botnets/zombies to multiply impact

**Mitigations:** filtering, rate limiting, scrubbing services (CDNs), redundant capacity, upstream provider support.

---

# 🌐 3.6 IP Vulnerabilities & Threats

### 🧩 IPv4 / IPv6 Notes

IP does not validate source IP; attackers can spoof addresses. Understanding header fields helps in detection and hardening.

### ⚠️ ICMP & IP-Level Exploits

* **ICMP** used for recon (ping), mapping, OS fingerprinting, firewall state discovery — and DoS.
* Common ICMP messages abused: echo request/reply, unreachable, mask reply, redirects, router discovery.

**Recommendation:** strict ICMP ACLs at network edge; IDS/IPS monitoring.

### 📡 Amplification & Reflection

Attacks (e.g., Smurf, DNS/NTP amplification) use small spoofed requests to open servers that respond with much larger responses to the victim.

**Mitigation:** disable IP broadcast responses; rate limit; secure DNS resolvers; avoid open resolvers.

### 🕵️ Spoofing

* **IP spoofing:** blind or non-blind, often part of DoS or session hijacking flows.
* **MAC spoofing:** internal threat allowing impersonation on local segments.

**Mitigation:** egress/ingress filtering (BCP 38), DHCP snooping, dynamic ARP inspection, port security.

---

# 🔗 3.7 TCP & UDP Vulnerabilities

### 📶 TCP Fundamentals

TCP is stateful and reliable; it uses a three-way handshake (SYN, SYN-ACK, ACK) and flags (URG, ACK, PSH, RST, SYN, FIN). This statefulness creates attack opportunities.

**TCP Attacks:**

* **SYN Flood:** many SYNs with no handshake completion exhaust server connection tables (half-open sockets).
* **RST Injection:** spoofed RST packets terminate sessions.
* **Session Hijacking:** attacker predicts sequence numbers and injects data into an established session.

**Defenses:** SYN cookies, connection rate limiting, properly tuned TCP stacks, stateful firewalls.

### 📤 UDP Characteristics & Attacks

UDP is connectionless with low overhead (used by DNS, TFTP, NFS, SNMP, media). Lacks built-in reliability or encryption.

**UDP Attacks:**

* **UDP Floods:** overwhelm hosts/networks; often use spoofed source addresses and closed ports to generate ICMP replies that burden networks.
* **Amplification via UDP Services:** similar to reflection/amplification attacks.

**Mitigations:** rate-limit, ACLs, disable unused services, monitor traffic anomalies.

---

# ⚙️ 3.8 IP Services — ARP, DNS, DHCP

### 🧭 ARP & ARP Poisoning

ARP maps IP → MAC on a local segment. Gratuitous ARP allows hosts to announce an IP/MAC mapping; attackers can poison ARP caches to perform MITM.

**ARP Poisoning Flow (Simplified):**

1. Attacker sends spoofed ARP replies to victim and gateway.
2. Victim and gateway update ARP cache, pointing to attacker MAC.
3. Attacker intercepts, forwards, or modifies traffic (passive or active).

**Mitigations:** static ARP where feasible, dynamic ARP inspection (switch-based), port security, encrypted end-to-end protocols.

### 🌍 DNS Attacks

* **Cache Poisoning:** fraudulent DNS records redirect users to malicious IPs.
* **Open Resolver Abuse:** open recursive resolvers used in amplification attacks.
* **Fast Flux / Double Flux / Domain Generation Algorithms (DGA):** techniques to hide command and control infrastructure.
* **Domain Shadowing:** attacker creates many subdomains under a compromised registrar account to serve malicious content.
* **DNS Tunneling:** exfiltrate data by encoding payloads into DNS queries.

**Controls:** DNSSEC (signing), filtering/inspection of DNS queries, monitoring long/odd queries, use authoritative resolvers, restrict recursion.

### 🧾 DHCP Attacks

Rogue DHCP server can hand out malicious configuration (gateway, DNS) causing MITM or denial of service.

**Mitigation:** DHCP snooping, port security, network access control (NAC), authenticate management.

---

# 🧱 3.9 Network Security Best Practices

### 🔒 CIA Triad

* **Confidentiality:** protect data from unauthorized disclosure (encryption, access control).
* **Integrity:** assure data is unmodified (hashing, HMAC).
* **Availability:** ensure authorized users can access services (redundancy, DoS mitigation).

### 🛡️ Defense-in-Depth (Layered Security)

No single control is sufficient. Combine:

* Device hardening (routers, switches, hosts)
* Firewalls and ASA devices
* VPNs for protected remote access
* IDS/IPS for detection and prevention
* Email and Web Security Appliances (ESA/WSA)
* AAA servers (RADIUS/TACACS+)
* Logging, SIEM, and incident response processes

### 🔥 Firewalls & IPS/IDS

* Firewalls enforce access policies between network zones.
* IDS detects suspicious patterns; IPS blocks them.
* IDS/IPS use signatures (atomic or composite) and anomaly detection.

### 📧 Content Security Appliances

* **Email Security Appliance (ESA):** filters SMTP, integrates threat feeds (e.g., Talos updates).
* **Web Security Appliance (WSA):** URL filtering, application controls, malware scanning, SSL inspection.

**Operational Tips:** keep signature and intelligence feeds updated; monitor logs; practice incident response.

---

# 🔐 3.10 Cryptography — Primitives & Usage

### 🎯 Goals of Secure Communications

Four elements:

1. **Data Integrity** — hashes (MD5, SHA family) detect changes.
2. **Origin Authentication** — HMACs, digital signatures.
3. **Data Confidentiality** — symmetric (AES, 3DES, DES legacy) and asymmetric (RSA, DH) encryption.
4. **Non-Repudiation** — digital signatures proving origin.

### 🧮 Hash Functions

* **MD5 (128-bit):** legacy; vulnerable — avoid for security use.
* **SHA-1 (160-bit):** legacy; has known weaknesses.
* **SHA-2 Family:** SHA-224/256/384/512 — recommended.

**Note:** hashing alone does not prevent tampering unless combined with a secret (HMAC) or a digital signature.

### ⚡ Symmetric Encryption

* Fast and used for bulk data (VPNs, TLS bulk cipher).
* Common: AES (128/192/256), 3DES, DES (legacy), RC series (RC4 is a stream cipher historically used in TLS — deprecated).
* Key length matters: minimum recommended is 128 bits for most use cases.

### 🔑 Asymmetric Encryption & Key Exchange

* Uses public/private key pairs (RSA, DSA, ECDH, DH).
* Slower; used for key exchange, signatures, small payloads.
* **Diffie-Hellman (DH):** enables two parties to derive a common secret without sending it directly.
* **Elliptic Curve Cryptography (ECC):** smaller key sizes with similar security.

**Common Usage Pattern:** use asymmetric crypto to exchange a symmetric session key; use the symmetric key for bulk encryption.

---

# 🧾 3.11 Summary

* Security breaches cause financial, reputational, and legal harm. Address vulnerabilities proactively.
* Understand attack vectors (internal and external) and data loss channels.
* Identify threat actors and toolsets used in real attacks.
* Malware families: viruses, worms, trojans, ransomware, rootkits, spyware.
* Network attacks fall into reconnaissance, access (including social engineering), and DoS categories.
* IP/TCP/UDP protocols have inherent weaknesses; apply appropriate filtering and device hardening.
* Protect IP services (ARP, DNS, DHCP) against spoofing, poisoning, tunneling, and rogue services.
* Practice defense-in-depth: combine firewalls, IPS/IDS, content security, AAA, encryption, and good operational hygiene.

---
## ✅ Final Word

Security is everybody’s responsibility — prevention, detection, and response must work together to protect assets, preserve trust, and keep services available.

