# 🛡️ Network Perimeter Defense: Firewalls, Port Forwarding, VPNs & VLANs

**Module:** SOC Foundations / Network Basics  
**Room Source:** TryHackMe — Extending Your Network & Network Hardware  
**Objective:** Implement external traffic traversal via Port Forwarding, analyze firewall packet inspection modes (Stateful vs. Stateless), examine secure Virtual Private Network (VPN) tunneling protocols, and document Layer 2/3 device virtualization (VLANs).

---

## 🚪 Network Traversal: Port Forwarding

By default, services hosted inside a private network area (such as a local web server running on `192.168.1.10:80`) are isolated within an **Intranet**. Devices on external networks cannot discover or reach them.

*   **Port Forwarding:** The process of mapping an external public port on a gateway router to a specific private IP address and port inside the local network segment.
*   **Operational Flow:** When an outside client sends a request to the router’s public interface (`82.62.51.70:80`), the router intercepts the packet and translates the destination parameters, steering the traffic inward to the designated internal asset (`192.168.1.10:80`).

---

## 🧱 Perimeter Hardening: Firewall Architectures

While Port Forwarding dictates whether a port is reachable, a **Firewall** acts as border security, inspecting incoming and outgoing packet headers to drop or permit traffic based on a predefined Access Control List (ACL).

### 📊 Comparative Analysis: Stateful vs. Stateless Firewalls

| Security Attribute | 🧠 Stateful Packet Inspection (SPI) | 📜 Stateless Packet Inspection (ACL) |
| :--- | :--- | :--- |
| **Inspection Scope** | Analyzes the entire connection context, tracking active communication states across handshakes. | Evaluates individual packets statically, isolated from prior network actions. |
| **Resource Usage** | **High:** Requires significant volatile memory to map active connection state tables dynamically. | **Low:** Minimal overhead; lightning-fast execution speed across deep packet volumes. |
| **Defensive Style** | Blocks entire hosts dynamically if anomalous behavior or half-open handshakes are logged. | Drops individual packets explicitly matching static rule files (e.g., block Port 80 traffic). |
| **Primary Weakness** | Can become resource-exhausted under massive connection stress. | Dumb; easily bypassed if rules do not cover all vector variations explicitly. |
| **Optimal Use Case** | Hardening critical internal data lanes and tracking application transactions. | Mitigating high-volume volumetric attacks like Distributed Denial-of-Service (DDoS). |

---

## 🚇 Encrypted Transit: Virtual Private Networks (VPN)

A **VPN** establishes an encrypted communication pathway called a **Tunnel** over the public internet, grouping geographically separate assets into a single shared virtual private network space.

### 🛡️ Core Security Benefits
*   **Privacy & Sniffing Defense:** Encrypts data headers and payloads completely. Even if an attacker sniffs packets on an insecure guest Wi-Fi connection, the data remains unreadable without the corresponding cryptographic keys.
*   **Anonymity Constraints:** Hides internal traffic from Internet Service Providers (ISPs). The absolute level of anonymity depends entirely on the provider's logging framework and privacy compliance standards.

### 📜 VPN Tunneling Technologies
*   **PPP (Point-to-Point Protocol):** Provides foundational authentication and encryption parameters using private key infrastructure. It is inherently non-routable on its own.
*   **PPTP (Point-to-Point Tunneling Protocol):** Wraps PPP data inside IP headers to let it exit local routing gates. It features widespread device support and simple setups, but carries weak, legacy cryptographic standards.
*   **IPSec (Internet Protocol Security):** Implements strong encryption directly into the network architecture tier. While complex to structure initially, it offers high-grade protection for corporate site-to-site bridges.

---

## 🎛️ Local Layer Hardware Virtualization & Segmentation

Modern corporate infrastructures rely heavily on the precise separation of network equipment roles to maximize availability and isolation:

### 1. Layer 2 vs. Layer 3 Switches
*   **Layer 2 Switch:** Operates strictly via hardware **MAC Addresses**. It is solely responsible for receiving incoming data frames and forwarding them to the specific endpoint mapped to its physical port layout.
*   **Layer 3 Switch:** A hybrid device featuring advanced capabilities. It handles Layer 2 frame switching natively, but includes built-in routing logic to route IP packets between separate internal subnets without relying on an external router.

### 2. VLANs (Virtual Local Area Networks)
VLANs allow network administrators to split a single physical switch into multiple separate logical network zones.
*   *Enterprise Isolation Scenario:* The **Sales Department** and the **Accounting Department** plug into the exact same physical switch hardware, but are assigned to separate VLAN tags.
*   *Security Outcome:* Both environments share the same outbound internet gateway line, but cannot speak to or sniff data from one another. This containment drastically reduces the internal attack surface during a malware breakout.

---

## 🚩 Practical Lab Evidence & Captures

### 🧪 Firewall Filtering Configuration Task
*   **Target Vulnerable Host IP:** `203.0.110.1`
*   **Remediation Action:** Hardened rule sets to intercept malicious traffic (red packet array) while preserving valid queries (green packet array).
*   **Rule Execution:** Dropped inbound anomalies focused on TCP Port `80`.

### 🕵️ Network Simulator Tracking Matrix
*   **Lab Scenario:** Analyzing data frame encapsulation steps across a multi-hop router mesh topology.
*   **Action Path:** Sent a direct TCP packet initialization sequence across network segments from `Computer 1` to `Computer 3`.
*   **Uncovered Network Architecture Flag:** `THM{SIMULATE_THE_NETWORK}` *(Note: Double check this exact string on your local lab page screen to ensure perfect validation match).*
