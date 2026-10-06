# 📦 TCP/IP Architecture, Handshake Mechanics & Common Ports

**Module:** SOC Foundations / Network Basics  
**Room Source:** TryHackMe — Packets, Frames, and Protocol Handshakes  
**Objective:** Differentiate between Network Layer packets and Data Link frames, analyze the inner headers of TCP/UDP protocols, trace connection states (Establishment, Ingestion, Termination), and map standard network service ports.

---

## 🏗️ Protocol Stack Architecture: Packets vs. Frames

While packets and frames represent small pieces of segmented data that assemble to form a complete network message, they live at different layers of the conceptual framework:

*   **Packet (Layer 3 - Network Layer):** The data unit handled at the logical routing boundary. It encapsulates raw payloads and appends an **IP Header** containing logical sourcing and destination parameters.
*   **Frame (Layer 2 - Data Link Layer):** The data unit handled at the physical network hardware layer. It takes the Layer 3 packet and wraps it in a **Hardware Header**, appending physical factory **MAC Addresses**.
*   *Mailing Envelope Analogy:* The frame acts as the outer envelope containing the physical delivery addresses. The packet acts as the internal letter text sheet. Once an intermediate router opens the envelope (destructures the frame), it reads the letter headers (packet parameters) to determine how to route the traffic onward.

### 🛡️ Critical IP Packet Header Parameters
*   **Source / Destination Address:** Standard logical IP markings defining the originating device and target endpoint.
*   **Time to Live (TTL):** A protective expiry timer decremented by each router hop. Prevents lost or misrouted packets from looping indefinitely and clogging global bandwidth limits.
*   **Checksum:** A mathematical integrity verification mechanism. If telemetry bytes shift during transit, the resulting checksum output changes, causing the receiving device to discard the corrupted payload.

---

## 🔀 Transport Tier Analysis: TCP vs. UDP Configurations

The architectural model groups the seven OSI layers into a summarized 4-layer framework: **Application, Transport, Internet, and Network Interface**.

### 1. TCP (Transmission Control Protocol) — Connection-Oriented
Designed for strict reliability and structured flow control. It reserves system resources to maintain a continuous virtual connection.
*   **Header Footprint:** Includes dynamic tracking headers like *Source/Destination Ports*, *Source/Destination IPs*, *Sequence Numbers*, *Acknowledgement Numbers*, *Checksums*, and *Control Flags* (SYN, ACK, FIN, RST).

#### 🤝 The Three-Way Handshake (Establishment Phase)
Before transmitting an application payload, endpoints use specific control flags to synchronize baseline sequences:
1.  **SYN (Synchronize):** The client transmits an Initial Sequence Number (ISN) primitive (e.g., Sequence = `0`) to initialize communication.
2.  **SYN/ACK (Synchronize/Acknowledge):** The receiving server generates its own unique tracking sequence number (e.g., `5000`) and increments the client's counter by 1 (`ACK = 1`) to confirm receipt.
3.  **ACK (Acknowledge):** The client responds with a final confirmation matching the server's tracking state (`ACK = 5001`), locking the connection channel open for active data streams.

#### 🛑 Connection Termination Framework (`FIN` / `RST`)
*   **Graceful Closure (FIN):** When transmission ends, the client transmits a `FIN` flag. The server acknowledges this check and returns its own `FIN` block. The client performs a final `ACK` to cleanly release reserved volatile memory allocations.
*   **Abrupt Terminate (RST):** A last-resort safety flag sent to instantly kill a connection due to system faults, application failures, or exhausted server resources.

### 2. UDP (User Datagram Protocol) — Connectionless
A stateless framework engineered for raw performance velocity over data integrity. It drops packets onto the network segment without verifying destination availability or establishing a formal handshake.
*   **Header Footprint:** Stripped of tracking fields. It retains basic attributes: *TTL*, *Source/Destination IP*, *Source/Destination Ports*, and the raw *Data* payload.
*   **Use Cases:** Latency-sensitive applications like video streaming, VoIP voice lines, and automated device discovery frameworks (ARP, DHCP).

---

## ⚓ The Network Port Matrix

Ports are numeric parameters ranging from **`0` to `65535`** that dictate which application or daemon receives specific inbound data streams. Ports from **`0` to `1024`** are designated as **Common Ports** mapped to universal standard rules.

| Service Protocol | Standard Port | Operational Profile & Verification Purpose |
| :--- | :--- | :--- |
| **FTP** (File Transfer Protocol) | `21` | Client-server architecture engineered for remote file ingest/download. |
| **SSH** (Secure Shell) | `22` | Secure, encrypted text-based remote shell console administration. |
| **HTTP** (HyperText Transfer) | `80` | Powers the World Wide Web; fetches standard unencrypted web text assets. |
| **HTTPS** (HTTP Secure) | `443` | Fetches web assets securely via active Layer 6 **SSL/TLS Encryption**. |
| **SMB** (Server Message Block) | `445` | Manages localized file repositories and shared network hardware (printers). |
| **RDP** (Remote Desktop Protocol) | `3389` | Full GUI-based remote system access for administration. |

*   *Note on Custom Configuration:* Services can be configured to listen on non-standard custom ports (e.g., running an internal HTTP panel on port `8080`). However, client connections must explicitly denote the target by appending a colon separator (e.g., `http://192.168.1.50:8080`).

---

## 🚩 Practical Lab Evidence & Captures

### 🧪 Target Connection Triage
*   **Target Infrastructure Destination IP:** `8.8.8.8`
*   **Target Communication Port:** `1234`
*   **Triage Step:** Opened the interactive database client tool to direct a stateless packet stream to the specified port path.
*   **Uncovered Lab Flag Payload:** `THM{PORTS_AND_PROTOCOLS}`
