# 🧰 CCNA - Module 2: Basic Switch & End Device Configuration

## 🎯 Objective  
Set up the **initial configuration** on switches and end devices: names, passwords, IP addresses, and basic connectivity — using Cisco IOS (CLI).

---

## 2.1 ⚙️ Cisco IOS Access

### 💻 What is an OS?
All devices (PCs, switches, routers) need an **Operating System (OS)** to function.

- **Shell** – The user interface (CLI or GUI)
- **Kernel** – Communicates between software and hardware
- **Hardware** – Physical components like CPU, memory, ports

### 🖼️ CLI vs GUI

| Interface | Used For          | Examples           | Pros                     | Cons             |
|----------:|------------------|--------------------|--------------------------|------------------|
| **GUI**   | User interaction  | Windows, macOS     | Friendly, visual         | May crash/freeze |
| **CLI**   | Admin control     | Cisco IOS          | More stable, precise     | Steeper learning curve |

📌 *Cisco devices are mostly configured via CLI for stability and control.*

---

### 🔌 Access Methods

| Method   | Secure? | Use Case                                      |
|----------|--------|-----------------------------------------------|
| Console  | ✅      | Direct physical connection (initial setup)    |
| SSH      | ✅      | Encrypted remote CLI access                   |
| Telnet   | ❌      | Insecure remote CLI (plaintext, legacy only)  |

### 🧰 Terminal Emulation Tools
- **PuTTY**
- **Tera Term**
- **SecureCRT**

---

## 2.2 🧭 IOS Navigation

### 🔐 Command Modes

| Mode                  | Prompt         | Purpose                          |
|-----------------------|----------------|----------------------------------|
| User EXEC             | `>`            | Basic monitoring                 |
| Privileged EXEC       | `#`            | Full access                      |
| Global Configuration  | `(config)#`    | Device-wide settings             |
| Line Config           | `(config-line)#` | Console/VTY (SSH/Telnet) access |
| Interface Config      | `(config-if)#` | Interface (e.g. port) settings   |

### 🔄 Mode Transitions

```bash
enable                    # User → Privileged
configure terminal        # Privileged → Global Config
line console 0            # Global → Line Config
interface vlan 1          # Global → Interface Config
exit                      # Go back one mode
end / Ctrl+Z              # Exit all config modes → Privileged EXEC
```

---

## 2.3 🧱 Command Structure

### 📐 Syntax Rules

```
command [optional] {required} <argument>
```

- `bold` → typed exactly as shown
- *italics* → user-entered values
- `[ ]` → optional
- `{ }` → required
- `{x | y}` → choose one

📌 Example:
```bash
ping 192.168.10.5
```

### 🆘 Help & Completion

- `?` → shows available options
- `Tab` → auto-completes command
- Command feedback helps correct syntax mistakes

---

### ⚡ Hotkeys & Shortcuts

| Key             | Function                                        |
|----------------|-------------------------------------------------|
| `Tab`          | Auto-complete command                          |
| `Up / Down`    | Scroll command history                         |
| `Ctrl + C`     | Abort command / return to exec mode            |
| `Ctrl + Z`     | End config mode → Privileged EXEC              |
| `Ctrl + Shift + 6` | Break process (e.g., stop ping)           |
| `Space`        | Next screen when `--More--` appears            |
| `Enter`        | Next line when `--More--` appears              |

---

## 2.4 🖥️ Basic Device Configuration

### 🏷️ Set Hostname

```bash
Switch(config)# hostname MySwitch
Switch(config)# no hostname         # Reset to default
```

### 🔒 Set Passwords

#### Console Password (User EXEC)
```bash
Switch(config)# line console 0
Switch(config-line)# password cisco
Switch(config-line)# login
```

#### Privileged EXEC Password
```bash
Switch(config)# enable secret class
```

#### VTY Lines (SSH/Telnet)
```bash
Switch(config)# line vty 0 15
Switch(config-line)# password letmein
Switch(config-line)# login
```

🧠 *Cisco switches usually support up to 16 VTY lines (0–15).*

### 🔐 Encrypt Passwords
```bash
Switch(config)# service password-encryption
```

### 📢 Banner Message

```bash
Switch(config)# banner motd # Unauthorized access is prohibited #
```

📌 The `#` is just a delimiter — use any non-conflicting character.

---

## 2.5 💾 Save Configurations

### 🧠 Running vs Startup Config

| File            | Location | Description                            |
|-----------------|----------|----------------------------------------|
| `running-config`| RAM      | Active config, lost on reboot          |
| `startup-config`| NVRAM    | Saved config, used on boot             |

### 🧾 Save Config

```bash
Switch# copy running-config startup-config
```

### 🔄 Rollback Options

- Undo unsaved changes:
```bash
Switch# reload
```

- Erase saved config:
```bash
Switch# erase startup-config
Switch# reload
```

### 📁 Save Config to Text File (Tera Term / PuTTY)

1. Start logging in terminal emulator
2. Run:
```bash
show running-config
```
3. Stop logging & save file

---

## 2.6 🌐 Ports and Addresses

### 🌍 IP Addressing

| Type   | Bits | Format                  |
|--------|------|--------------------------|
| IPv4   | 32   | 192.168.1.1              |
| IPv6   | 128  | 2001:0db8:85a3::8a2e:... |

- **Subnet Mask** defines network vs host part
- **Default Gateway** allows internet routing

---

### 🔌 Interfaces & Media

Network media options:
- Twisted Pair (Ethernet)
- Fiber Optic
- Wireless
- Coaxial

Factors to consider:
- Speed and bandwidth
- Distance
- Environment
- Cost

---

## 2.7 ⚙️ Configure IP Addressing

### 🧍 Manual Config (Windows)

1. Control Panel → Network Settings  
2. Adapter → Properties  
3. Choose IPv4 → Set:
   - IP address
   - Subnet mask
   - Default gateway

### 🤖 Automatic via DHCP

1. Go to same place
2. Choose:
   - "Obtain IP address automatically"
   - "Obtain DNS automatically"

🧠 IPv6 uses DHCPv6 and SLAAC (Stateless Auto Config)

---

### 🧩 Configure Switch Virtual Interface (SVI)

```bash
Switch(config)# interface vlan 1
Switch(config-if)# ip address 192.168.1.10 255.255.255.0
Switch(config-if)# no shutdown
```

🧠 This gives your switch a management IP for SSH/remote access.

---

## 2.8 🔍 Verify Connectivity

### 📡 Use `ping`

```bash
Switch# ping 192.168.1.100
```

- Success = IP, cables, and config all OK
- Failure? → Check cabling, IPs, shutdown state

---


## 🧠 Quick Tips

- 🔐 Use **SSH** not Telnet in real networks  
- 💾 Always `copy run start` to save your config  
- 📡 Set `no shutdown` on interfaces to bring them up  
- ⚠️ Forgot a password? You’ll need to recover via console

---
