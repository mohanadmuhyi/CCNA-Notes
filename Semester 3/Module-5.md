# CCNA ENSA - Semester 3

## 📘 Module 5: ACLs for IPv4 Configuration


### 🎯 Objectives

In this module, you will implement IPv4 ACLs to filter traffic and secure administrative access. The focus is on learning how to configure standard IPv4 ACLs, modify ACLs effectively, secure VTY access with ACLs, and configure extended ACLs for more advanced filtering.

---

## 🔹 5.1 Configure Standard IPv4 ACLs

### ✏️ Create an ACL

When configuring ACLs, it is important to plan carefully. Policies should be written in a text editor, the IOS configuration commands added, remarks included for documentation, and then pasted into the device. Afterward, the ACL should be tested thoroughly.

➡️ Each individual rule inside an ACL is called an **Access Control Entry (ACE)**. An ACL is essentially a list of ACEs.

If the **wildcard mask is omitted**, the router assumes it to be `0.0.0.0`, which means the ACL will match only that single host. For example, `access-list 1 permit 192.168.1.1` is the same as `access-list 1 permit host 192.168.1.1`.

### 🔢 Numbered Standard IPv4 ACL Syntax

```
Router(config)# access-list access-list-number { deny | permit | remark text } source [source-wildcard] [log]
Router(config)# no access-list access-list-number   ! remove ACL
```

* **Range**: 1–99 or 1300–1999
* `deny` = block traffic if condition matches
* `permit` = allow traffic if condition matches
* `remark` = documentation
* `log` = log matches
* `host` keyword = shorthand for wildcard `0.0.0.0`

### 📝 Named Standard IPv4 ACL Syntax

```
Router(config)# ip access-list standard NAME
Router(config-std-nacl)# { deny | permit | remark text } source [wildcard]
Router(config)# no ip access-list standard NAME   ! remove ACL
```

* Names are **case-sensitive** and must be unique.
* Difference: Numbered ACLs are configured directly in **global config mode** (`access-list ...`). Named ACLs enter a **special ACL configuration sub-mode** (`ip access-list standard NAME`) where ACEs are entered line by line.

### 📌 Apply a Standard IPv4 ACL

```
Router(config-if)# ip access-group { access-list-number | name } { in | out }
Router(config-if)# no ip access-group { access-list-number | name }
```

### 🔍 Verification Commands

* `show running-config` → verify ACL in config
* `show access-lists` → view ACL details
* `show ip interface` → check ACL applied to interface

### ✅ Example (Numbered ACL)

```
Router(config)# access-list 1 permit 192.168.10.10
Router(config)# access-list 1 permit 192.168.20.0 0.0.0.255
Router(config)# interface s0/1/0
Router(config-if)# ip access-group 1 out
```

### ✅ Example (Named ACL)

```
Router(config)# ip access-list standard PERMIT-TRAFFIC
Router(config-std-nacl)# permit host 192.168.10.10
Router(config-std-nacl)# permit 192.168.20.0 0.0.0.255
Router(config)# interface s0/1/0
Router(config-if)# ip access-group PERMIT-TRAFFIC out
```

---

## 🔹 5.2 Modify IPv4 ACLs

### ✏️ Two Methods

1. **Text Editor Method** → Copy ACL from the running configuration into a text file, edit it there, remove the original ACL from the router, and paste the corrected ACL back.

   * Example:

   ```
   Router# show running-config | include access-list
   (copy output)
   access-list 1 permit 192.168.10.10
   access-list 1 permit 192.168.20.0 0.0.0.255
   (edit in text editor to add or remove entries)
   Router(config)# no access-list 1
   Router(config)# access-list 1 permit 192.168.10.11
   Router(config)# access-list 1 permit 192.168.30.0 0.0.0.255
   ```

2. **Sequence Number Method** → add/delete ACEs by sequence number.

### 🛠️ Sequence Number Example

```
Router(config)# ip access-list standard PERMIT-TRAFFIC
Router(config-std-nacl)# 10 permit 192.168.20.0 0.0.0.255
Router(config-std-nacl)# no 10       ! delete rule
Router(config-std-nacl)# 10 deny host 192.168.10.11
```

### 📊 ACL Statistics

* `show access-lists` → shows matches per ACE.
* `clear access-list counters` → resets counters.
* **Note**: Implicit `deny any` is not shown unless explicitly configured.

---

## 🔹 5.3 Secure VTY Ports with a Standard IPv4 ACL

### 🔐 Steps

To secure VTY lines, create an ACL to identify allowed administrative hosts and apply it using the `access-class` command.

### Example

```
Router(config)# username ADMIN secret class
Router(config)# ip access-list standard ADMIN-HOST
Router(config-std-nacl)# permit 192.168.10.10
Router(config-line)# line vty 0 4
Router(config-line)# login local
Router(config-line)# transport input ssh
Router(config-line)# access-class ADMIN-HOST in
```

### Verification

```
Router# show access-lists
```

A successful SSH connection from 192.168.10.10 matches the permit entry, while attempts from other networks match the deny.

---

## 🔹 5.4 Configure Extended IPv4 ACLs

### 🔢 Extended ACL Syntax

```
Router(config)# access-list access-list-number { deny | permit | remark text } protocol source source-wildcard [operator port] destination destination-wildcard [operator port] [established] [log]
Router(config)# ip access-list extended NAME
Router(config-ext-nacl)# { deny | permit | remark text } protocol source source-wildcard destination destination-wildcard [operator port]
```

* **Range**: 100–199 or 2000–2699

### Parameter Explanation

* **protocol** → specifies IP, TCP, UDP, ICMP, etc.
* **source/source-wildcard** → identifies the source IP/network.
* **destination/destination-wildcard** → identifies the destination IP/network.
* **operator port** → defines conditions (eq, lt, gt, neq, range) for port filtering.
* **established** → only permits return traffic from existing TCP sessions (like answering calls from known contacts only).
* **log** → logs matches.

### Protocol & Port Filtering

* Protocols: IP, TCP, UDP, ICMP
* Ports: eq (equal), lt (less than), gt (greater than), neq (not equal), range

### ✅ Example: Filtering HTTP

```
Router(config)# access-list 100 permit tcp 192.168.10.0 0.0.0.255 any eq www
Router(config)# access-list 100 permit tcp 192.168.10.0 0.0.0.255 any eq 80
Router(config-if)# ip access-group 100 in
```

### ✅ Example: Filtering SSH

```
Router(config)# access-list 101 permit tcp host 192.168.10.10 any eq 22
Router(config)# access-list 101 deny tcp any any eq 22
Router(config)# access-list 101 permit ip any any
```

### ✅ Example: TCP Established Keyword

```
Router(config)# access-list 120 permit tcp any any established
Router(config-if)# ip access-group 120 out
```

This allows only return traffic, blocking unsolicited inbound connections.

### ✅ Example: Named Extended ACL

```
Router(config)# ip access-list extended SURFING
Router(config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 any eq 80
Router(config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 any eq 443
Router(config)# ip access-list extended BROWSING
Router(config-ext-nacl)# permit tcp any 192.168.10.0 0.0.0.255 established
Router(config-if)# ip access-group SURFING in
Router(config-if)# ip access-group BROWSING out
```

### Editing Extended ACLs

Extended ACLs can be modified using either the text editor method (removing and reapplying) or the sequence number method:

```
Router(config)# ip access-list extended SURFING
Router(config-ext-nacl)# no 10
Router(config-ext-nacl)# 10 permit tcp 192.168.30.0 0.0.0.255 any eq 80
```

### Verification

* `show ip interface` → check ACL on interface
* `show access-lists` → confirm matches
* `show running-config` → verify applied config

---

## 🏁 Final Words

In this module, you learned how to configure, apply, and verify both standard and extended ACLs. You also practiced modifying ACLs using text editor methods and sequence numbers. You explored securing remote access through ACLs applied to VTY lines, and you understood how ACLs can filter traffic based on source, destination, protocol, and port.
