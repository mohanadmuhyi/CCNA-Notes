# CCNA ENSA - Semester 3
## 🌐 Module 14: Network Automation


### 🎯 Module Objective

This module focuses on explaining the purpose and characteristics of **network virtualization** and **automation** in modern networks. It introduces the concepts of automation, data formats, APIs, REST and RESTful APIs, configuration management tools, and Intent-Based Networking (IBN) with Cisco DNA Center. By the end of this module, you will understand how automation and programmability simplify network management, improve efficiency, and enable intelligent network behavior through Cisco’s DNA Center.

---

## ⚙️ 14.1 Automation Overview

### 🤖 Automation Everywhere

Automation is all around us — from self-service checkouts to self-driving cars.

**Benefits of Automation:**

* Machines work 24/7 without breaks.
* Provides consistent output.
* Enables quick data collection and analysis.
* Reduces human risk in dangerous environments.
* Smart devices can adapt behavior for efficiency and safety.

### 🧠 Thinking Devices

* Devices that act based on input or environment are called **smart devices**.
* Smart devices use **network automation tools** for programming and decision-making.

---

## 💾 14.2 Data Formats

### 📘 Concept

Data formats are structures for storing and exchanging data. Common formats include:

* **JSON (JavaScript Object Notation)**
* **XML (eXtensible Markup Language)**
* **YAML (YAML Ain’t Markup Language)**

Each format has specific **syntax**, **structure**, and **key/value rules**.

### 🧩 JSON Format

* Human-readable and widely used in APIs.
* Uses braces `{}` for objects and brackets `[]` for arrays.
* Written as **key/value pairs**.

**Syntax Rules:**

* Keys are strings in double quotes.
* Values can be strings, numbers, arrays, booleans, or objects.
* Multiple pairs separated by commas.
* Arrays use `[ ]` and are ordered lists.

**Example:**

```json
{
  "addresses": [
    {"ip": "172.16.0.2", "netmask": "255.255.255.0"},
    {"ip": "172.16.0.3", "netmask": "255.255.255.0"}
  ]
}
```

### 🧾 YAML Format

* Superset of JSON, more human-friendly.
* Uses **indentation**, not brackets or commas.
* Key-value pairs separated by a colon `:`.
* Lists use hyphens `-`.

**Example:**

```yaml
ietf-interfaces:interface:
  name: GigabitEthernet2
  enabled: true
  ietf-ip:ipv4:
    address:
      - ip: 172.16.0.2
        netmask: 255.255.255.0
      - ip: 172.16.0.3
        netmask: 255.255.255.0
```

### XML Format

* Similar to HTML; uses custom tags.
* Data is enclosed in `<tag>value</tag>`.

**Example:**

```xml
<interface>
  <name>GigabitEthernet2</name>
  <enabled>true</enabled>
  <ipv4>
    <address>
      <ip>172.16.0.2</ip>
      <netmask>255.255.255.0</netmask>
    </address>
  </ipv4>
</interface>
```

---

## 🔗 14.3 APIs (Application Programming Interfaces)

### 🧠 Concept

* An **API** allows applications to interact and exchange data/services.
* Works like a **waiter**: takes a request and delivers a response.

### 🍽️ API Types

1. **Open/Public APIs** – accessible to everyone (often need a free key).
2. **Internal/Private APIs** – for internal use within a company.
3. **Partner APIs** – shared between trusted partners (e.g., airlines and travel sites).

### 🌍 Types of Web Service APIs

| Type     | Data Format     | Strength                |
| -------- | --------------- | ----------------------- |
| SOAP     | XML             | Established, structured |
| REST     | JSON, XML, YAML | Most common, flexible   |
| XML-RPC  | XML             | Simple, legacy          |
| JSON-RPC | JSON            | Simple, modern          |

---

## 🌐 14.4 REST (Representational State Transfer)

### 💡 REST Basics

* REST APIs use **HTTP/HTTPS** for communication.
* Operate via **requests** and **responses**.
* RESTful means it follows REST principles:

  * **Client-Server** separation.
  * **Stateless** communication.
  * **Cacheable** for performance.

### 🧰 RESTful HTTP Methods (CRUD)

| Method    | Operation |
| --------- | --------- |
| POST      | Create    |
| GET       | Read      |
| PUT/PATCH | Update    |
| DELETE    | Delete    |

### 🌎 URI, URN, and URL

* **URI (Uniform Resource Identifier):** General identifier for a resource. Example → `https://example.com/book.html`.
* **URN (Uniform Resource Name):** Identifies a resource by name, not location. Example → `urn:isbn:0451450523`.
* **URL (Uniform Resource Locator):** Specifies the location of the resource. Example → `https://www.example.com/images/logo.png`.

### 🔍 Parts of a URL

A typical URL consists of:

1. **Protocol/Scheme:** Defines communication method (e.g., `https`, `ftp`).
2. **Hostname:** The domain name or IP (e.g., `www.example.com`).
3. **Path:** Specifies the location or file (e.g., `/author/book.html`).
4. **Fragment:** Optional identifier within a page (e.g., `#page155`).

**Full Example:** `https://www.example.com/author/book.html#page155`

### 🔢 Details of a Query String

Queries are used to send information to the server within the URL. They contain:

1. **Format:** Defines data type requested (e.g., `outFormat=json`).
2. **Key:** Authorization token or API key (e.g., `key=123ABC`).
3. **Parameters:** Custom data for the request (e.g., `from=San+Jose,CA&to=Monterey,CA`).

### 🌐 Example RESTful Request

```
https://www.mapquestapi.com/directions/v2/route?key=KEY&from=San+Jose,CA&to=Monterey,CA&outFormat=json
```

* **API Server:** mapquestapi.com
* **Resource:** directions API
* **Query:** contains key, format, and parameters.

### 🧪 RESTful API Tools

* **Web Browser:** run GET requests directly.
* **Postman:** test REST APIs.
* **Python:** automate API calls.
* **Network OS:** NETCONF, RESTCONF for network configuration.

---

## 🧠 14.5 Configuration Management Tools

### 🧍‍♂️ Traditional Configuration

* Previously done manually via CLI.
* Time-consuming and error-prone.

### 🧩 Modern Network Automation

* Uses APIs and protocols (REST, JSON, XML, Python, etc.) to manage devices at scale.

### 🧱 Agentless Architecture (Ansible)

* **Agentless** means no software agent is installed on managed devices.
* The **controller** connects remotely (e.g., via SSH or HTTPS) and executes configuration changes.
* Reduces complexity, increases security, and makes deployment easier.
* Example: **Ansible** communicates directly without agents, ideal for heterogeneous networks.

### 🛠️ Automation vs Orchestration

* **Automation** → Tool performs a task automatically.
* **Orchestration** → Coordinates multiple automated tasks into workflows.

### ⚒️ Popular Tools

| Tool          | Language      | Agent       | Controller    | Output   |
| ------------- | ------------- | ----------- | ------------- | -------- |
| **Ansible**   | Python + YAML | Agentless   | Any device    | Playbook |
| **Chef**      | Ruby          | Agent-based | Chef Master   | Cookbook |
| **Puppet**    | Ruby          | Both        | Puppet Master | Manifest |
| **SaltStack** | Python        | Both        | Salt Master   | Pillar   |

---

## 🧬 14.6 Intent-Based Networking (IBN) & Cisco DNA Center

### 🧠 Intent-Based Networking

* Next-gen model built on **Software-Defined Networking (SDN)**.
* Converts **business intent → network policy → automated configuration**.
* Uses **AI**, **analytics**, and **automation**.

**Three Core Functions:**

1. **Translation** – Converts business intent into technical policies.
2. **Activation** – Deploys these policies across the network.
3. **Assurance** – Continuously verifies network behavior matches intent.

### 🕸️ Network Infrastructure as Fabric

* **Overlay:** Logical virtual connections (e.g., CAPWAP, IPsec).
* **Underlay:** Physical topology and hardware connections.

### 🧩 Cisco Digital Network Architecture (DNA)

* Implements the IBN model using Cisco’s **DNA Center**.
* Continuously collects and analyzes data to ensure network security and performance.

**Cisco DNA Solutions:**

| Solution          | Description                                     | Benefit                       |
| ----------------- | ----------------------------------------------- | ----------------------------- |
| **SD-Access**     | Unified LAN/WLAN fabric for secure segmentation | Fast, secure access           |
| **SD-WAN**        | Cloud-managed WAN connections                   | Better performance, agility   |
| **DNA Assurance** | AI-based monitoring and troubleshooting         | Predicts issues, speeds fixes |
| **DNA Security**  | Real-time threat analysis & visibility          | 360° network protection       |

### 🖥️ Cisco DNA Center Platform

Acts as the **central controller** and **analytics hub** for the enterprise network.

**Main Areas:**

* **Design:** Build the network model.
* **Policy:** Define and enforce rules.
* **Provision:** Deploy new devices.
* **Assurance:** Monitor and verify network performance.
* **Platform:** Integrate APIs and multi-vendor systems.

### 🔌 DNA Center Platform APIs

* Provide **programmatic access** for integration with IT systems.
* Allow **automation, analytics, and multi-vendor** device management.
* Support REST-based API calls for provisioning, monitoring, and configuration.

### 🧩 DNA Center Design and Provision

* **Design:** Model entire networks including buildings, sites, and devices.
* **Provision:** Quickly deploy new devices or update existing ones using templates.
* Simplifies mass provisioning and ensures consistency.

### 📜 DNA Center Policy and Assurance

* **Policy:** Create and enforce access policies reflecting business intent.
* **Assurance:** Provides **AI-driven insights**, monitoring, and troubleshooting of connected devices.

### 🧠 DNA Center Troubleshooting

* Uses advanced analytics to identify **root causes** and **suggest fixes**.
* Predicts and prevents network problems using **machine learning**.
* Provides real-time alerts, health scores, and recommendations.

---

## 📝 14.7 Summary

* Automation reduces human intervention using tools and scripts.
* Smart devices react to data via automation tools.
* JSON, XML, and YAML are key data formats in automation.
* APIs allow communication between systems (Open, Internal, Partner).
* RESTful APIs use HTTP methods to perform CRUD operations.
* URL contains protocol, host, path, and optional fragments and query parameters.
* Configuration management tools (Ansible, Chef, Puppet, SaltStack) simplify large-scale automation.
* Agentless tools like **Ansible** streamline automation without requiring local agents.
* Intent-Based Networking (IBN) transforms business goals into automated network configurations.
* Cisco DNA Center provides unified automation, assurance, and analytics for enterprise networks.
* DNA Center Platform APIs enable integration, automation, and intelligent troubleshooting.

---

## ✅ Final Word
Network automation represents the future of scalable and secure networking. By mastering automation tools, data formats, APIs, and Cisco DNA Center technologies, engineers can design, deploy, and maintain intelligent networks that adapt dynamically to business needs.

