# 🕸️ LAN Topologies, Routing Infrastructure & Subnetting

**Module:** SOC Foundations / Network Basics  
**Room Source:** TryHackMe — Network Topologies & Segmentation  
**Objective:** Compare physical network designs (Star, Bus, Ring), define the functional roles of Layer 2 Switches and Layer 3 Routers, and map logical subnet segmentation parameters.

---

## 🏗️ Physical Network Topologies

In networking, **Topology** refers to the structural layout, design, and look of how computing devices are interconnected. Choosing the correct topology dictates the redundancy, cost, and fault tolerance of an enterprise environment.

### 1. Star Topology (The Modern Standard)
Devices are individually linked via point-to-point ethernet cables to a centralized networking node (like a switch or a hub).
*   **Advantages:** Highly scalable and reliable. If a single endpoint device cable breaks, only that specific node drops offline; the rest of the network operates normally.
*   **Disadvantages:** Higher overhead cost due to intense cabling requirements and dedicated hardware. It suffers from a **Single Point of Failure (SPOF)**—if the central switch crashes, the entire local network collapses.

### 2. Bus Topology (Legacy Backbone)
Devices stem off like branches from a single shared trunk connection known as a **Backbone Cable**.
*   **Advantages:** Simple layout and highly cost-efficient to deploy with minimal hardware dependencies.
*   **Disadvantages:** Prone to severe bottlenecks and data collisions if multiple hosts transmit concurrently. It lacks structural redundancy; if a break or cut occurs anywhere along the backbone cable, the entire network splits and fails.

### 3. Ring Topology (Token-Ring Loop)
Devices are chained directly to each other sequentially to form a continuous closed physical loop. Data travels in a single direction around the ring from hop to hop.
*   **Advantages:** Minimizes packet collision bottlenecks since data moves in a highly structured, controlled token sequence.
*   **Disadvantages:** Inefficient pathing because data may need to traverse multiple unrelated computers before reaching its destination. A hardware failure on any individual machine or a single cable cut breaks the loop, disrupting the entire network.

---

## 🎛️ Network Aggregation: Switches vs. Routers

To establish network performance and avoid downtime, enterprise environments deploy dedicated hardware assets designed to handle traffic streams intelligently.

*   **Switches (Layer 2 - Data Link Layer):** Designed to aggregate multiple physical devices (computers, printers) into a single local network. Switches maintain an internal hardware table mapping which device is plugged into which port. Unlike a legacy **Hub** (which blindly blasts data packets out to every port, causing high traffic noise), a switch reads the packet's target address and forwards it **exclusively to the intended port**.
*   **Routers (Layer 3 - Network Layer):** Designed to securely connect entirely different networks together and pass data packets between them. Routers execute **Routing**, which is the computational process of analyzing paths across interconnected infrastructures to deliver data across optimal pathways. Connecting multiple routers together builds **Network Redundancy** (backup paths to prevent downtime).

---

## 🍰 Logical Segmentation: Subnetting Baselines

**Subnetting** is the administrative process of slicing a single large network block into multiple smaller, isolated miniature networks called **Subnets**. 

### 📁 Operational Use Case
In a corporate enterprise, network administrators use subnets to separate departments logically (e.g., Accounting, Finance, Human Resources). This partitioning minimizes network noise, enhances security boundaries, and prevents standard users from sniffing sensitive financial telemetry.

### 🔢 The Subnet Mask & IP Allocation Matrix
Subnetting is achieved by using a 32-bit (4-byte) numerical marker called a **Subnet Mask** (ranging from `0` to `255`, such as `255.255.255.0`). Subnets break up IP usage into three distinct address profiles:

| Address Type | Functional Purpose | Real-World Operational Example |
| :--- | :--- | :--- |
| **Network Address** | Identifies the starting boundary and the actual existence of a subnet block. Cannot be assigned to a host computer. | `192.168.1.0` |
| **Host Address** | A unique, specific IP assigned directly to a functional device (endpoint, server, printer) within that subnet space. | `192.168.1.100` |
| **Default Gateway** | A special dedicated host address assigned to the inner interface of the router connecting this network to the outside world. Any data destined for an outside network is sent here first. | `192.168.1.254` (Typically uses either the first `.1` or last `.254` host address). |

---

## 🎛️ Network Orchestration Protocols: ARP & DHCP

For devices to route data traffic reliably within a local area network, automated protocols must constantly handle hardware mappings and dynamic address allocations behind the scenes.

### 1. ARP (Address Resolution Protocol)
ARP bridges the gap between Layer 3 (Network Layer - IP Addresses) and Layer 2 (Data Link Layer - MAC Addresses). It allows a device to associate its logical IP with a physical factory serial identity on a local segment.

*   **The ARP Cache (Ledger):** Every host maintains an internal cache memory table listing the IP-to-MAC maps of surrounding network assets so it doesn't have to announce itself for every single transaction.
*   **ARP Request (Broadcast):** When Device A wants to communicate with an IP but lacks its MAC footprint, it blasts a global query to **every device** on the subnet asking: *"Who owns this IP address? Tell me your physical MAC address."*
*   **ARP Reply (Unicast):** Only the specific device that owns the queried IP address responds. It transmits a direct, targeted reply back containing its physical hardware MAC address. Device A records this mapping inside its cache ledger for future processing.

### 2. DHCP (Dynamic Host Configuration Protocol)
Instead of manual input allocations (Static IPs), modern enterprise infrastructures use a **DHCP Server** to automatically lease IP addresses, subnet masks, and default gateway pathways to client assets as they connect.

The automation sequence uses the **DORA** framework:
1.  **DHCP Discover (Broadcast):** The newly connected device broadcasts a request across the network segment searching for an active DHCP server: *"Is there an address coordinator online?"*
2.  **DHCP Offer (Unicast/Broadcast):** The active DHCP server intercepts the discover block and replies with an available network allocation footprint: *"Here is an unleased IP address you can safely use."*
3.  **DHCP Request (Broadcast):** The client asset formally responds back to the server to claim the configuration: *"I accept this offer. Please lock this IP to my hardware footprint."*
4.  **DHCP ACK (Acknowledgement):** The server registers the configuration lease details in its internal asset tracking table and sends a final confirmation check: *"Confirmed. You are authorized to begin transmitting on this network."*

## 🔒 Defensive Insights: Subnetting Segregation Strategy

Beyond performance efficiency, subnetting establishes high-value perimeter defense parameters.
*   **CAFÉ EXPOSURE EXAMPLE:** Public hotspots carry high security threats from untrusted guest assets. Subnetting allows a facility to split its physical internet line into two entirely isolated logical subnet tracks:
    *   *Subnet A (Operations):* Cash registers, inventory servers, corporate point-of-sale databases, and employees.
    *   *Subnet B (Hotspot):* General public guest access.
*   **Security Outcome:** Even if an attacker joins the guest network and launches tools, the routing boundaries prevent them from seeing or pivoting into the sensitive operational workspace.
