# CCNA ENSA - Semester 3
## 🌐 Module 13: Network Virtualization

### 🎯 Objectives

Explain the purpose and characteristics of network virtualization.

---

## ☁️ 13.1 Cloud Computing

### ☁️ Cloud Overview

Cloud computing enables:

* Access to data anywhere, anytime.
* Streamlined IT operations by subscribing to only needed services.
* Reduced cost, energy use, and physical infrastructure.
* Rapid scalability to handle growing data demands.

### ⚙️ Cloud Service Models (NIST SP 800-145)

* **SaaS (Software as a Service):** Cloud provider delivers software and apps over the Internet.
* **PaaS (Platform as a Service):** Provides access to development tools and environments.
* **IaaS (Infrastructure as a Service):** Provides virtualized computing resources (servers, storage, networking).
* **ITaaS (IT as a Service):** Extends cloud services by providing IT support and management.

### ☁️ Cloud Deployment Models

* **Public Cloud:** Services available to the general public (e.g., AWS, Azure, Google Cloud).
* **Private Cloud:** Dedicated to a single organization.
* **Hybrid Cloud:** Combines private and public clouds for flexibility.
* **Community Cloud:** Shared by organizations with similar needs (e.g., healthcare compliance).

### 🏢 Data Center vs Cloud Computing

* **Data Center:** On-premises or leased physical facility for computing and storage.
* **Cloud Computing:** On-demand access to shared virtualized resources hosted in data centers.

---

## 🖥️ 13.2 Virtualization

### 🔄 Cloud Computing and Virtualization

* Virtualization is the **foundation** of cloud computing.
* It separates the **operating system (OS)** from the **hardware**, allowing multiple virtual machines (VMs) to run on one device.

### 💽 Dedicated Servers (Before Virtualization)

* Each service (web, email, etc.) had its own physical server.
* Problems:

  * **Single point of failure** when hardware fails.
  * **Underutilization** of resources (idle servers).
  * **Server sprawl**: excessive physical servers wasting space and energy.

### ⚡ Server Virtualization

* Combines multiple servers into fewer physical devices.
* Enables **multiple OSs** on a single machine.
* Uses **redundancy** to prevent downtime.
* Managed by a **hypervisor**, which creates and runs virtual machines.

### ✅ Advantages of Virtualization

* Lower hardware and energy costs.
* Faster provisioning and prototyping.
* Higher uptime and disaster recovery.
* Legacy system support.

### ⚙️ Abstraction Layers

Hardware → Firmware → OS → Services.

* **Hypervisor** sits between firmware and OS to host multiple VMs.

### 💿 Type 2 Hypervisors

* Installed **on top of an OS** (e.g., VirtualBox, VMware Workstation).
* Easier setup, no dedicated management software required.

---

## 🌐 13.3 Virtual Network Infrastructure

### 💿 Type 1 Hypervisors

* Installed **directly on hardware** (bare metal) — e.g., VMware ESXi, Hyper-V.
* Provide higher **performance**, **scalability**, and **robustness**.

### 🛠️ Management Console

* Used to manage multiple hypervisors and VMs.
* Can handle **failover**, **server consolidation**, and **over-allocation** (assigning more virtual resources than physically exist).
* Example: **Cisco UCS Manager**.

### 🕸️ Network Virtualization Challenges

* Traditional networks struggle with VM **mobility** and **dynamic traffic**.
* Two types of traffic:

  * **East-West:** Between VMs within the same data center.
  * **North-South:** Between data center and external networks.

### 🔁 Network Virtualization Solutions

* Use **QoS** and **security policies** for dynamic flows.
* Network devices can also be virtualized:

  * **Subinterfaces, VLANs, VRFs** (Virtual Routing and Forwarding) allow multiple virtual routers on one device.

---

## 🧠 13.4 Software-Defined Networking (SDN)

### 🧩 Planes of Operation

Every network device has **three planes**:

* **Control Plane:** The brain of the device. Handles routing protocols, ARP tables, and topology decisions.
* **Data Plane (Forwarding Plane):** Moves packets according to control plane rules. Operates at high speed.
* **Management Plane:** Allows administrators to configure and monitor devices (SSH, HTTPS, SNMP, etc.).

### ⚙️ Cisco Express Forwarding (CEF)

* A **Layer 3 switching technology** that enables packets to be forwarded directly in the data plane without CPU involvement.
* In SDN, this concept evolves — the control plane moves to a **centralized controller**, leaving data forwarding to the device.

### 🧠 SDN Concept

* **Definition:** SDN is the separation of the control plane and data plane, managed by a central controller.
* The controller acts as the brain, making routing and policy decisions for all network devices.
* Devices become simple data-forwarding elements, improving scalability and automation.

### 🧱 SDN Benefits

* Simplifies configuration and management.
* Increases flexibility and agility.
* Allows centralized security and policy control.
* Enables automation and programmability.

### 🧰 SDN Components

* **OpenFlow:** Protocol that allows the controller to define how switches handle traffic.
* **OpenStack:** Orchestration platform for cloud environments, automating provisioning of virtual resources.
* **I2RS:** Interface to Routing System, providing access to routing information.
* **TRILL, FabricPath, SPB:** Technologies improving Layer 2 scalability and redundancy.

### 🧩 APIs in SDN

* **Northbound APIs:** Connect SDN controllers to applications and management systems.
* **Southbound APIs:** Connect SDN controllers to network devices (OpenFlow is the most common example).

### 🧭 Traditional vs SDN Architecture

* **Traditional:** Control + Data + Management planes all exist on each device.
* **SDN:** Control plane centralized → controller decides how each device should forward traffic.
* **Result:** Simplified operations, consistent policy enforcement, easier scalability.

---

## 🧩 13.5 Controllers

### 🕹️ SDN Controller Functions

* Defines **data flows** between devices.
* Verifies all flows according to network policies before permitting traffic.
* Populates **flow tables**, **group tables**, and **meter tables** in switches:

  * **Flow Table:** Matches packets and applies specific actions.
  * **Group Table:** Manages multiple flows with shared actions.
  * **Meter Table:** Controls traffic rate and performance.

### 🧠 Cisco ACI (Application Centric Infrastructure)

* **Cisco ACI** extends SDN with a **hardware-based architecture** for large-scale data centers.
* It integrates cloud computing, virtualization, and data center management.
* **Core Idea:** Separate the **application policy** from the **infrastructure**, allowing automatic configuration and scalability.

### 🔑 ACI Core Components

1. **Application Network Profile (ANP):**

   * A logical container for applications and their network policies.
   * Contains **Endpoint Groups (EPGs)** that define which workloads can communicate.
2. **Application Policy Infrastructure Controller (APIC):**

   * Centralized brain of ACI.
   * Translates application requirements into network configurations.
   * Provides programmability and centralized control.
3. **Cisco Nexus 9000 Switches:**

   * Provide the physical network fabric.
   * Designed for **high-speed**, **application-aware** switching.
   * Work directly with APIC to enforce ACI policies.

### 🌿 Spine-Leaf Topology

* **Spine switches:** Connect all leaf switches.
* **Leaf switches:** Connect to servers, storage, and spines.
* Every device is only **one hop away** from every other device.
* Ensures **low latency** and **high performance**.

### 🧠 SDN Types

1. **Device-based SDN:** Applications program network devices directly.
2. **Controller-based SDN:** A central controller manages all network devices and flows (e.g., OpenDaylight).
3. **Policy-based SDN:** Adds an abstraction layer for automation (e.g., Cisco APIC-EM).

### 🧰 Cisco APIC-EM (Enterprise Module)

* A **software-based controller** for enterprise and campus networks.
* Simplifies network automation and management.
* Provides tools such as:

  * **Topology Discovery** – visualize network devices and connections.
  * **Path Trace Tool** – shows traffic flow between nodes and ACL behavior.
  * **Policy Configuration** – allows admins to enforce network-wide policies.

### 🔍 APIC-EM Path Trace Example

* Shows how traffic passes through devices and ACLs.
* Highlights denied or conflicting entries.
* Enables easy troubleshooting and security enforcement.

---

## 🧪 13.6 Summary

* Cloud computing provides scalable, cost-effective resources.
* Virtualization is the core of cloud technology.
* Type 1 = bare-metal hypervisor; Type 2 = hosted hypervisor.
* Network virtualization enables flexible, scalable infrastructures.
* SDN separates control/data planes for automation.
* Cisco ACI and APIC-EM simplify network management and policy deployment.
* ACI introduces hardware-level policy control, while APIC-EM focuses on enterprise software automation.

---

## 🧾 Additional Notes for NetAcad Questions

* **Hybrid Cloud** represents two or more distinct clouds (private, public, etc.) connected through a single architecture, each remaining unique.
* **Community Cloud** is designed for a specific industry or group (e.g., healthcare or education) with shared compliance and policy requirements.
* A **Hypervisor** is software, firmware, or hardware that provides an abstraction layer on top of the physical hardware, allowing multiple VMs to operate.
* Major advantages of virtualization include:

  * Requires **less equipment**.
  * **Faster provisioning** of servers and services.
  * **Increased server uptime** and reliability.
* A **Type 2 Hypervisor** runs on top of an existing OS to create and manage virtual machines.
* **Type 1 Hypervisor** requires a **management console** for VM management and does **not prevent over-allocation** (it allows memory sharing between instances).
* **East-West traffic** refers to communication between virtual servers in the same data center.


* **Control Plane:** Processes routing protocols and topology information using the CPU; makes forwarding decisions.
* **Data Plane:** Forwards packets; uses specialized processors; consists of the internal switch fabric.
* **Management Plane:** Used for device configuration and monitoring (SSH, SNMP, HTTPS).
* **SDN (Software-Defined Networking):** A new architecture that simplifies and automates network management by centralizing control.
* **SDN Controller:** Logical entity that manages how the data plane of switches and routers handle network traffic.
* **Southbound APIs:** Define how the controller communicates with network devices to program forwarding behavior.

* **Flow Table:** Matches packets to flows and defines actions.
* **Meter Table:** Regulates traffic rate and performance actions.
* **Controller-based SDN:** Centralized control for all devices in the network.
* **Device-based SDN:** Each device is programmable individually or through local applications.
* **Policy-based SDN:** Adds an abstraction layer to simplify advanced tasks using GUIs and built-in automation (no coding needed).

---

### 🏁 Final Word

Network virtualization is the foundation of modern networking. By understanding SDN and ACI, engineers can build agile, programmable, and policy-driven infrastructures that support both enterprise and cloud-scale environments.
