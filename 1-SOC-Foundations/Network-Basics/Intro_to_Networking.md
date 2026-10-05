# 🌐 Introduction to Networking & Host Addressing

**Module:** SOC Foundations / Network Basics  
**Room Source:** TryHackMe — What is Networking?  
**Objective:** Define public vs. private network boundaries, analyze IPv4/IPv6 address architectures, audit physical MAC address tracking, and inspect security exposures related to MAC Spoofing and ICMP network mapping.

---

## 🏗️ Relational Topology: Private vs. Public Networks

A network is formed whenever two or more technical assets are connected to share data. The internet functions as a massive public network comprised of billions of interconnected smaller perimeters called **Private Networks**.

*   **Private Networks:** Localized, restricted network zones (e.g., a corporate home office or internal server room). Devices inside this boundary use **Private IP Addresses** to communicate with each other locally.
*   **Public Networks (The Internet):** Global routing zones. Any internal data traveling outward to the internet passes through a gateway router and is identified by a single **Public IP Address** assigned by an Internet Service Provider (ISP).

---

## 🆔 Host Identification: Network Layer vs. Data Link Layer

Devices use two primary layers of identification to locate each other and pass data systematically:

### 1. The IP Address (Internet Protocol)
Acts as a fluid, permeable network identity (similar to a device's "name" or dynamic mailing address).
*   **IPv4 Standard:** A 32-bit addressing scheme divided into four **Octets** (e.g., `192.168.1.77`), supporting up to 4.29 billion distinct addresses. 
*   **IPv6 Standard:** A modern 128-bit hexadecimal addressing format designed to resolve the global IPv4 address exhaustion crisis, capable of supporting over 340 undecillion addresses.

### 2. The MAC Address (Media Access Control)
Acts as a permanent, immutable physical identity assigned directly at the factory to the device’s **Network Interface Card (NIC)** (similar to a hardware serial number or fingerprint).
*   **Structure:** A 12-character hexadecimal string separated by colons (e.g., `50:3E:AA:E8:3B:64`). The first six characters define the specific manufacturer hardware profile (OUI), and the final six characters serve as the unit's unique serial number.

---

## 🛡️ Security Vectors: MAC Spoofing Tactics

Because MAC addresses are hardcoded into physical motherboards, many public networks (like guest hotel Wi-Fi portals or automated firewall filters) blindly trust specific MAC addresses to enforce access controls or billing requirements. 

*   **MAC Spoofing:** An offensive technique where an unauthorized workstation alters its software configuration settings to match the exact MAC address of a trusted or paid asset. 
*   **Exploitation Scenario:** If Device A passes authentication to bypass a captive network portal, an attacker can spoof Device A’s MAC address. The firewall dynamically mistakes the attacker for the authorized target, allowing them to intercept data or bypass payment controls.

---

## 🛠️ Network Diagnostic Utilities: ICMP Ping Mapping

**Ping** is a fundamental utility used to verify end-to-end host reachability and connection reliability between two devices.
*   **Protocol Backbone:** Operates entirely via **ICMP (Internet Control Message Protocol)**.
*   **Mechanism:** The source terminal transmits an **ICMP Echo Request** packet out to a destination IP or domain. If the target host is active, it processes the request and transmits an **ICMP Echo Reply** back to the sender, measuring round-trip propagation times in milliseconds (ms).
*   **Syntax:** `ping [IP_Address_or_URL]`
*   *Example Execution:* `ping 8.8.8.8`

---

## 🚩 Practical Lab Evidence & Captures

### 🧪 Host Asset Inventory Audit
*   **Target Machine 1 Hostname:** `DESKTOP-KJE57FD`
    *   *Local Subnet IP:* `192.168.1.77` (Assigned via DHCP)
    *   *Hardware MAC Address:* `EC:5C:68:C3:7E:51`
*   **Target Machine 2 Hostname:** `CMNatic-PC`
    *   *Local Subnet IP:* `192.168.1.74` (Assigned via DHCP)
    *   *Hardware MAC Address:* `50:3E:AA:E8:3B:64`

### 🕵️ MAC Spoofing & ICMP Routing Capture
*   **Lab Scenario:** Bypassing captive network controls on a public hotel Wi-Fi environment by duplicating the hardware signature of an authenticated network asset.
*   **Execution Step:** Modified the network adapter signature of `CMNatic-PC` to mirror the target MAC string `EC:5C:68:C3:7E:51`.
*   **Diagnostic Step:** Executed a live network query to Google's public primary DNS host via `ping 8.8.8.8`.
*   **Uncovered Lab Flag Payload:** `THM{NETWORKING_FOUNDATIONS}`
