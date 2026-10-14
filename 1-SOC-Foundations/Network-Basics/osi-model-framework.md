# Comprehensive Lab Report: ISO/OSI 7-Layer Networking Architecture & Data Encapsulation

## 1. Structural Networking Posture: The OSI Model Blueprint
The **Open Systems Interconnection (OSI) model** is a theoretical, standardized framework developed by the International Organization for Standardization (ISO) to define computer network communications [INDEX]. In defensive security operations, a precise understanding of each layer is mandatory; it enables analysts to diagnose structural vulnerabilities, analyze network attack signatures, and explicitly map terms like "Layer 3 routing anomalies" or "Layer 7 web application firewall (WAF) mitigations" [INDEX].

```text
       [ ENCAPSULATION FLOW ]                   [ DATA UNIT ]
  Layer 7: Application Layer      ──────────➔     Raw Data
  Layer 6: Presentation Layer     ──────────➔     Raw Data
  Layer 5: Session Layer          ──────────➔     Raw Data
  Layer 4: Transport Layer        ──────────➔     Segments (TCP) / Datagrams (UDP)
  Layer 3: Network Layer          ──────────➔     Packets (IP)
  Layer 2: Data Link Layer        ──────────➔     Frames (MAC)
  Layer 1: Physical Layer         ──────────➔     Bits (0s and 1s)
```

## 2. Layer 1: The Physical Layer
The Physical Layer handles the structural mechanical and electrical hardware links connecting devices across a network perimeter [INDEX]. It dictates how raw binary streams (0s and 1s) are translated into transmittable physical signals [INDEX].

### Technical Transmission Mediums:
*   **Wired Boundaries:** Standard copper twisted-pair configurations (e.g., Ethernet Category 5e/6 cables matching the **802.3** specification standard) or high-bandwidth Optical Fiber pipelines transmitting modulated light waves [INDEX].
*   **Wireless Boundaries:** Local radio frequency spectra traversing dedicated Wi-Fi bands (including the standard **2.4 GHz, 5 GHz, and modern 6 GHz** channels) [INDEX].
*   *SOC Incident Note:* Physical layer security covers hardware-rooted vulnerabilities, including unauthorized physical tap splices on fiber runs, rogue physical network devices inserted into data jacks, and signal-jamming disruption campaigns directed at wireless bands.

## 3. Layer 2: The Data Link Layer
The Data Link Layer establishes the protocol-level agreements required to execute data transfers between adjacent nodes residing on the **same network segment** sharing a common communications medium (e.g., endpoints connected to a local layer-2 network switch) [INDEX].

### Hardened Addressing Architecture (The MAC Address)
Communications at Layer 2 are directed natively via a unique, hardware-rooted physical identity key called a **Media Access Control (MAC) Address** [INDEX]. 
*   **Structure:** Composed of 6 bytes (48 bits) expressed in hexadecimal characters separated by colons (e.g., `02:B7:DF:1D:0D:99`) [INDEX].
*   **Vendor Attribution Trariage:** The three leftmost bytes (the initial 24 bits) represent the **Organizationally Unique Identifier (OUI)** assigned to identify the specific hardware network interface card manufacturer [INDEX].
*   *Forensics Inspection Triage:* In a network frame capture (e.g., analyzing Ethernet traffic inside Wi-Fi or local area connections via Wireshark), every frame explicitly displays a **Source MAC Address** and a **Destination MAC Address** embedded in the structural header [INDEX]. Security engineers track MAC anomalies to spot MAC address spoofing, unmask rogue local entities, or catch Man-in-the-Middle (MITM) local poisoning vectors.

## 4. Layer 3: The Network Layer
While Layer 2 manages localized frame deliveries, the Network Layer handles communication mapping, logical addressing, and data forwarding **between entirely separate networks** [INDEX].

### Core Logical Routing Primitives:
*   **Protocols:** Driven primarily by the **Internet Protocol (IP)**, accompanied by diagnostic tools like the **Internet Control Message Protocol (ICMP)** [INDEX]. It also encompasses encrypted transit boundaries like Virtual Private Networks (**IPSec** and **SSL/TLS VPN** technologies) [INDEX].
*   **The Routing Engine:** Layer 3 routers dynamically analyze destination IP headers to determine the most efficient, low-cost architectural path to forward a network packet across global gateway hops [INDEX].
*   *SOC Analysis Focus:* Incident responders analyze Layer 3 headers to verify source/destination IP routing footprints, block external malicious command-and-control (C2) servers, and investigate anomalous ICMP sweep scans used by threat actors for network discovery.
## 5. Architectural Variations: The 5-Layer Internet Protocol Stack
While the ISO/OSI model relies on a strict seven-layer architecture, modern networking textbooks and engineering frameworks (such as *Computer Networking: A Top-Down Approach* by Kurose and Ross) compress the upper abstractions into a practical **5-Layer Internet Protocol Stack** [INDEX]. This model merges the Application, Presentation, and Session layers into a single operational entity while explicitly maintaining the physical layers [INDEX]:
1.  **Application Layer** (Aggregates OSI Layers 5, 6, and 7) [INDEX]
2.  **Transport Layer** (OSI Layer 4) [INDEX]
3.  **Network Layer** (OSI Layer 3) [INDEX]
4.  **Link Layer** (OSI Layer 2) [INDEX]
5.  **Physical Layer** (OSI Layer 1) [INDEX]

## 6. Layer 3 Logical Addressing: IPv4 Subnetting & Command Line Triage
To establish unambiguous, unique host identification across separate networks, the TCP/IP suite mandates assigning a logical network identifier called an **Internet Protocol (IP) Address** to every connected device [INDEX]. 

### A. The Anatomy of an IPv4 Address
*   **Bit Structure:** An IPv4 address is composed of **4 octets (32 bits total)** [INDEX]. Each octet (8 bits) represents a decimal range spanning from **0 to 255** (e.g., `192.168.66.89`) [INDEX]. Mathematically, this 32-bit ceiling (`2^32`) caps global unique IPv4 allocations to approximately 4 billion addresses [INDEX].
*   **Reserved Structural Boundaries:** 
    *   *Network Address:* The absolute lowest address boundary within a subnet block (terminating in binary `00000000` or `.0` in standard class-C notation) is strictly reserved to identify the network segment itself [INDEX].
    *   *Broadcast Address:* The absolute highest address boundary within a subnet block (terminating in binary `11111111` or `.255` in standard class-C notation) is strictly reserved to broadcast data packets to all active hosts on that network segment simultaneously [INDEX].

### B. Command-Line Host Configuration Triage
Analysts interrogate network interface cards natively to dump local IPv4 addresses, broadcast routes, and subnet masks utilizing system-specific terminal utilities [INDEX]:
*   **Windows Environment:** `ipconfig` [INDEX]
*   **POSIX/Linux Environments:** `ifconfig` or the modern object wrapper `ip address show` (abbreviated as **`ip a s`**) [INDEX].

```text
  [ Terminal Output Ingestion: "ip a s" / "ifconfig" ]
  ┌───────────────────┬───────────────────┬─────────────────────────────────┐
  │ Local IPv4 IP     │ Subnet Prefix     │ Broadcast Domain Mapped Route   │
  ├───────────────────┼───────────────────┼─────────────────────────────────┤
  │ 192.168.66.89     │ /24 (255.255.255) │ 192.168.66.255                  │
  └───────────────────┴───────────────────┴─────────────────────────────────┘
```
*   *CIDR Notation Impact (/24):* A subnet mask defined as `255.255.255.0` is expressed in Classless Inter-Domain Routing (CIDR) notation as **`/24`** [INDEX]. This indicates that the leftmost 24 bits (the initial 3 digits/octets) represent the static network identifier that remains immutable across the subnet, isolating valid client address availability strictly from `.1` to `.244` [INDEX].

### C. Enterprise Architecture Isolation: RFC 1918 Private Addressing
To preserve global public IP limits, systems segment local corporate infrastructure networks from the public internet using three explicit **Private IP Address Ranges** defined under **RFC 1918** [INDEX]:
1.  **`10.0.0.0     ➔  10.255.255.255`** (`10/8` prefix allocation block) [INDEX]
2.  **`172.16.0.0   ➔  172.31.255.255`** (`172.16/12` prefix allocation block) [INDEX]
3.  **`192.168.0.0  ➔  192.168.255.255`** (`192.168/16` prefix allocation block) [INDEX]
*   *SOC Perimeter Rule:* RFC 1918 private addresses are strictly unroutable over the public internet [INDEX]. For internal devices to communicate externally, an edge perimeter router must perform **Network Address Translation (NAT)** to map the internal private traffic onto a single valid public IP routing gateway [INDEX].

## 7. Layer 4: The Transport Layer Control Plane
The Transport Layer moves beyond host-to-host routing to provide end-to-end communication constraints directly between separate active software applications running on distinct systems [INDEX]. This layer utilizes **Port Numbers (ranging from 1 to 65535)** to identify and map traffic down to specific software processes [INDEX].

### A. User Datagram Protocol (UDP - Connectionless Operations)
UDP is a stripped-back, high-velocity transport protocol optimized for speed over delivery confirmation [INDEX].
*   **The Mechanic:** Functions as a connectionless layer-4 protocol [INDEX]. It transmits datagram packages instantly without establishing an active state link with the destination host, missing any built-in mechanism to confirm if the remote endpoint successfully ingested the data [INDEX].
*   *Analogy Alignment:* Comparable to sending standard mail via a local post office without delivery confirmation [INDEX]. It sacrifices structural error-correction loops to optimize packet throughput speeds, making it ideal for DNS queries, video streaming, and volatile log forwarding streams [INDEX].

### B. Transmission Control Protocol (TCP - Connection-Oriented Operations)
TCP is a connection-oriented transport framework engineered explicitly to guarantee absolute reliability, data re-ordering, flow control segmentation, and mistake correction over transmission streams [INDEX].
*   **The Mechanic:** Requires initiating an explicit, stateful cryptographic connection loop between endpoints before any standard application data fields are permitted to pass across the socket wire [INDEX].
## 8. Stateful Connection Establishment: The TCP Three-Way Handshake
To guarantee reliable, structured data transmission over un-isolated physical mediums, the Transmission Control Protocol (TCP) assigns an incremental **Sequence Number** to every transmitted data octet [INDEX]. This allows the receiving host to reconstruct fragmented streams in the correct order and instantly flag duplicated or dropped packets [INDEX]. The receiver confirms successful packet receipt by sending back an **Acknowledgment Number** matching the exact next expected data octet byte string [INDEX].

Before any upper-layer application metadata is permitted to traverse the network, TCP forces a stateful connection initialization loop called the **Three-Way Handshake** using specific packet control headers [INDEX]:

```text
  [ CLIENT PROFILE ]                                       [ SERVER PROFILE ]
          │                                                        │
          ├─────────────── 1. SYN (Initial Seq #) ────────────────>┤
          │                                                        │
          <─────────────── 2. SYN-ACK (Server Seq # + Client ACK) ─┤
          │                                                        │
          ├─────────────── 3. ACK (Acknowledge Server SYN) ───────>┤
          │                                                        │
     [ESTABLISHED]                                           [ESTABLISHED]
```

1.  **The SYN Packet (Synchronize):** The client initiates the connection sequence by sending a packet with the **SYN** flag set [INDEX]. This packet transmits the client's randomly generated Initial Sequence Number (ISN) to coordinate tracking boundaries [INDEX].
2.  **The SYN-ACK Packet (Synchronize-Acknowledge):** The target server intercepts the request and responds with a packet holding both the **SYN** and **ACK** flags [INDEX]. This packet acknowledges the client's sequence metric and transmits the server's own randomly chosen Initial Sequence Number [INDEX].
3.  **The ACK Packet (Acknowledge):** The client captures the server's response and transmits a final packet with the **ACK** flag set [INDEX]. This acknowledges receipt of the server's tracking metrics, moving the socket status on both hosts into an `ESTABLISHED` state ready to transmit application data [INDEX].

## 9. Data Processing Infrastructure: Encapsulation & The Packet Lifecycle
Network communication operates through a strict downward data wrapping process known as **Encapsulation** [INDEX]. This mechanism requires each discrete layer of the network stack to treat incoming data from the layer above as an unparsed chunk of text, append its own functional control headers (and sometimes trailers), and pass the newly wrapped data package to the layer below [INDEX].

### The Downward Encapsulation Progression:
1.  **Application Data Payload:** The user inputs plain-text strings or interactive data into an application interface (e.g., writing an email or entering a web search query), which formats the data according to application layer protocol schemas [INDEX].
2.  **Transport Segment/Datagram (Layer 4):** The transport layer (TCP or UDP) ingests the raw data payload and appends a layer-4 header specifying the **Source and Destination Ports** to create a **Segment** (TCP) or **Datagram** (UDP) [INDEX].
3.  **Network Packet (Layer 3):** The network layer ingests the transport segment and appends an IP header specifying the absolute **Source and Destination IP Addresses** to structure a routable **Packet** [INDEX].
4.  **Data Link Frame (Layer 2):** The link controller (such as an Ethernet or Wi-Fi interface card) wraps the network packet inside a localized header and trailer specifying the physical **Source and Destination MAC Addresses**, producing a **Frame** ready to be encoded into physical binary bits [INDEX].
*   *The Decapsulation Inversion:* Upon reaching the target server, this entire lifecycle operates in reverse. Each layer strips away its corresponding control headers to validate routing logic before passing the internal payload upward, eventually exposing the raw application data to the target process [INDEX].

## 10. Low-Level Protocol Querying & Manual Application Auditing (`telnet`)
While modern endpoint software obscures layer-7 transactions behind graphical interfaces, security analysts use raw terminal tools like the **Teletype Network (TELNET)** protocol engine to open unencrypted TCP channels directly to remote listening socket interfaces [INDEX]. This enables investigators to map out server properties, run manual service configurations, and extract plaintext headers [INDEX].

### A. Testing the Echo Subsystem (Port 7)
The Echo server bounces back any text string passed across the connection [INDEX]. It is used to test connectivity, compute latency, and audit basic data stream integration:
```text
Command: telnet <Target_IP> 7
Response: Connected to <Target_IP>.
Input: Testing Connection
Output: Testing Connection
```

### B. Dynamic Temporal Baseline Queries (Port 13)
The Daytime service returns the host's authoritative operating system calendar registry metrics and immediate local time before dropping the network thread automatically [INDEX]. It provides an immediate baseline for auditing host time-drift variances during log forensics:
```text
Command: telnet <Target_IP> 13
Response: Connected to <Target_IP>.
Output: Thu Jun 20 12:36:32 PM UTC 2024
Connection closed by foreign host.
```

### C. Manual HTTP Application Ingress Probing (Port 80)
Analysts troubleshoot web servers and verify application layer headers by opening a direct telnet link to a web service listening on port 80 [INDEX]. They manually type out an unencrypted **HTTP/1.1** request string to perform a banner grab:
```text
Command: telnet <Target_IP> 80
Response: Connected to <Target_IP>.
Input: 
GET / HTTP/1.1
Host: telnet.thm
[Press Enter Twice]

Output:
HTTP/1.1 200 OK
Content-Type: text/html
Server: Apache/2.4.41 (Ubuntu)
[...]
```
*   *SOC Hardening Advisory:* Legacy application protocols such as Telnet, Echo (Port 7), and Daytime (Port 13) transmit data entirely in plaintext without cryptographic session isolation [INDEX]. This exposure leaves data channels vulnerable to credential sniffing and traffic modification, making their active presence a severe security risk that should be permanently disabled across enterprise perimeters [INDEX].
