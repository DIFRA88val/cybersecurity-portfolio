# Comprehensive Lab Report: Network Traffic Dissection & Packet Forensics via Wireshark

## 1. Tactical Implementation: The Analyst's Triage Engine
**Wireshark** functions as an open-source, cross-platform layer-2 through layer-7 network packet analysis and capture inspection engine. In defensive operations, Wireshark is not utilized as an automated Intrusion Detection System (IDS); it is a passive diagnostic tool that relies strictly on the analyst's structural protocol knowledge to hunt out anomalies. 

### Strategic Defensive Use Cases:
*   **Infrastructure Diagnostic Triage:** Pinpointing localized network load failure points, layer-4 congestion loops, and hardware bottlenecking events.
*   **Security Anomaly Discovery:** Isolating unauthorized rogue endpoints, tracing abnormal application port usages, and mapping stealthy network scans.
*   **Malware Forensic Ingress Analysis:** Dissecting low-level protocol parameters, tracking response code variances, and pulling raw payload data matrices out of suspicious wire captures.

## 2. Interface Topology & Core GUI Architecture
The Wireshark user interface organizes comprehensive packet telemetry data feeds into specialized functional navigation panels:

### A. Primary Control Bars
*   **Main Menu / Toolbar Utility:** Consolidates advanced processing shortcuts used by investigators to filter, sort, summarize, export, and merge packet streams.
*   **Display Filter Bar:** The centralized data parsing engine. Facilitates real-time, logic-based query executions to extract targeted packet sets from extensive capture arrays.
*   **Capture Interface Plane:** Maps active hardware and software interfaces (such as `eth0`, `ens33`, and localized `lo` loopbacks), identifying immediate traffic sniffing entry points.

### B. The Tri-Pane Triage Workspace
When a packet capture file (PCAP/PCAPNG) is loaded into the engine, data fields are organized across a synchronized three-pane forensic display:

```text
  ┌────────────────────────────────────────────────────────────────────────┐
  │ 1. PACKET LIST PANE (Chronological Summary Ledger)                     │
  │    [ No. | Time | Source IP | Destination IP | Protocol | Length | Info ]│
  ├────────────────────────────────────────────────────────────────────────┤
  │ 2. PACKET DETAILS PANEL (Deep Protocol Layer Subsystem Dissection)     │
  │    [+] Frame Layer ➔ [+] Ethernet II ➔ [+] IPv4 Header ➔ [+] TCP Ports  │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 3. PACKET BYTES PANE (Low-Level Hexadecimal / Decoded ASCII Memory Map)│
  │    0000  cc 5e f8 02 21 a7 44 df  65 d8 fe 6c 08 00 45 00  . ^ . ! . D . e . l . . E .│
  └────────────────────────────────────────────────────────────────────────┘
```

1.  **Packet List Pane:** A real-time chronological ledger detailing high-level summary rows for every frame. Selecting a distinct item dynamically populates the lower panels with its underlying attributes.
2.  **Packet Details Panel:** Formats deep protocol layer structures into interactive, nested trees (unwrapping Layer 2 frames, Layer 3 IP paths, Layer 4 TCP control blocks, and Layer 7 payloads).
3.  **Packet Bytes Pane:** Displays a raw memory map containing the exact **Hexadecimal and Decoded ASCII** bytes traversing the interface wire. Clicking a specific structural field in the details tree automatically highlights its corresponding byte location in this pane to guide byte-level inspections.

## 3. Passive Forensic Telemetry & Permanent Coloring Arrays
Wireshark implements an integrated structural packet-coloring mechanism to help analysts visually categorize protocols, spot session errors, and isolate high-priority traffic anomalies instantly.
*   **Permanent Preference Rules:** Configurations are mapped to the active preference profile using the `View ➔ Coloring Rules` pipeline. These rules persist across active sessions, allowing threat hunters to map custom display filters (e.g., coloring unencrypted credential transmissions or specific anomalous port triggers) to distinct permanent background highlights.
*   **Temporary Conversation Filters:** Ephemeral color mappings used during live session analysis to track and isolate the traffic stream of a single network conversation without altering permanent profile baselines.

## 4. Capture Ingress & PCAP Lifecycle Management
*   **Traffic Sniffing Execution:** Network sniffing is controlled via active toolbar primitives: the *Shark Button* kicks off immediate live frame collection on a selected interface; the *Stop Button* halts incoming packet buffers; and the *Restart Button* flushes active memory rings to spin up a new capture session.
*   **PCAP/PCAPNG File Consolidation (`File ➔ Merge`):** Enables analysts to merge external packet traces with a currently loaded capture. Wireshark systematically aligns timestamps from both files, generating a unified, chronological master forensic file required to piece together complex cross-network attacks.
*   **Capture Properties Integrity Auditing:** Security engineers use the path `Statistics ➔ Capture File Properties` to view vital capture metadata, including the absolute **Cryptographic File Hash**, exact hardware capture interfaces, and baseline frame delivery metrics necessary to verify file integrity and prioritize incident response analysis.
## 5. Protocol Dissection & Multi-Layer Encapsulation Mapping
**Protocol Dissection** is the forensic process where Wireshark decodes raw binary network telemetry into human-readable protocol fields matching the layers of the OSI model [INDEX]. For a typical web application transaction packet (e.g., HTTP over TCP), the engine dissects the frame down into seven distinct inspection layers [INDEX]:

*   **Layer 1 (The Frame / Physical):** Evaluates physical-layer parameters, including frame numbers, exact capture timestamps, arrival date records, and raw bit lengths [INDEX].
*   **Layer 2 (Source/Destination MAC):** Maps physical hardware identities and Vendor OUI properties from the Data Link layer [INDEX].
*   **Layer 3 (Source/Destination IP):** Extracts logical network addressing fields and tracks the **Time-to-Live (TTL)** integer to evaluate routing hops [INDEX].
*   **Layer 4 (Protocol Transport):** Details TCP or UDP sequence trackers, window sizing, and source/destination port allocations [INDEX].
*   *Protocol Errors Sub-Layer:* An administrative extension tracking split TCP segments requiring reassembly before the complete application data can be read [INDEX].
*   **Layer 5/7 (Application Protocol):** Parses explicit high-level protocols such as HTTP, FTP, or SMB to track metadata values like **Entity Tags (e-tags)** [INDEX].
*   **Application Data Payload:** Extracts the underlying file or markup language content transferred natively over the network wire [INDEX].

## 6. Advanced Data Hunting, Packet Isolation & Labeling Controls
When parsing extensive network traces to pinpoint indicators of compromise (IoCs), analysts use content-based searching and indexing shortcuts to cut down on data noise [INDEX]:

### A. Pattern-Matching Exploration Methods (`Edit ➔ Find Packet`)
Investigators execute precise data-hunting queries across specified search horizons (Packet List, Packet Details, or Packet Bytes panes) utilizing four distinct input formats [INDEX]:
*   **Display Filter:** Sifts through the capture using standard programmatic networking parameters [INDEX].
*   **Hex Value:** Scans the raw memory bytes pane to locate specific hexadecimal signatures [INDEX].
*   **String Matching:** Sweeps decoded ASCII text strings (case-insensitive by default) to locate readable keywords or system indicators [INDEX].
*   **Regular Expressions (Regex):** Executes complex regex pattern-matching strings to hunt down advanced indicators like structured data leaks or variable command scripts [INDEX].

### B. Analytical Bookmarking Frameworks
*   **Marking Packets (`Edit ➔ Mark Packet`):** Overrides standard coloring rules to highlight chosen frames in solid black [INDEX]. This serves as an immediate visual anchor for further analysis or as a way to isolate a scoped set of suspicious packets for bulk exporting [INDEX]. 
    *   *Session Constraint:* Marked indicators are strictly ephemeral and will clear as soon as the file session closes [INDEX].
*   **Packet Comments (`Edit ➔ Packet Comment`):** Allows an investigator to write permanent documentation directly into individual packet structures [INDEX]. Unlike marking, packet comments save natively within `.pcapng` file metadata, ensuring the notes persist across sessions to guide peer analysts during cross-tier incident response [INDEX].

## 7. Forensic Extraction & Chronological Normalization
*   **Targeted Scoped Exporting (`File ➔ Export Specified Packets`):** Allows an analyst to prune a massive capture file down by saving exclusively marked or filtered packets into a separate, lightweight PCAP file, stripping away redundant network noise to simplify external analysis [INDEX].
*   **Layer-7 File Extraction (`File ➔ Export Objects`):** Extracts whole files transferred over the wire from active network streams [INDEX]. For security operations, this is a high-value capability used to pull downloaded binaries, documents, or scripts directly out of supported protocol flows (**HTTP, SMB, IMF, TFTP, and DICOM**) to submit them for automated sandbox malware analysis [INDEX].
*   **UTC Chronological Normalization (`View ➔ Time Display Format`):** By default, Wireshark logs time as seconds elapsed since the start of the capture [INDEX]. Incident responders shift this view to **UTC Date and Time of Day** to align network events with external logs (such as Windows Event IDs or firewall alerts), allowing them to construct an accurate master chronological attack timeline [INDEX].

## 8. Incident Analysis & Expert Info Severity Triage
The **Expert Info** engine operates as a protocol diagnostics checker that automatically parses traffic lines for structural rule violations, out-of-order handshakes, and abnormal communication states [INDEX]. While helpful for rapid triage, analysts treat these alerts as automated suggestions that must be manually verified to screen out false positives [INDEX].

### Wireshark Expert Severity Matrix:

| Expert Severity Tier | Visual Color Marker | Automated Tactical Assessment & Telemetry Group [INDEX] |
| :--- | :--- | :--- |
| 🔵 **Chat** | Blue | Information logs documenting standard, healthy application workflows [INDEX]. |
| 🟢 **Note** | Cyan | Logs notable, non-malicious anomalies such as standard application error codes [INDEX]. |
| 🟡 **Warn** | Yellow | Warnings alerting the analyst to unusual error codes, protocol problem statements, or retransmission storms [INDEX]. |
| 🔴 **Error** | Red | Critical infrastructure issues, malformed network packets, or checksum failures requiring immediate forensic review [INDEX]. |

*Administrative Triage Pipeline:* Analysts click the lower-left status bar shortcut or navigate to **`Analyze ➔ Expert Information`** to open a unified summary console displaying packet indices, group designations, protocols, and total occurrence tracking statistics [INDEX].
## 9. Structural Traffic Isolation: Capture vs. Display Filtering
To effectively triage massive data capture files, an investigator must distinguish between the two primary packet filtering architectures supported by Wireshark:
*   **Capture Filters:** Implemented prior to initiating network sniffing. These filters instruct the packet engine to strictly ingest packets that match the defined filter statement, dropping all other frames at the hardware interface layer to save system memory.
*   **Display Filters:** Applied post-sniffing or against an existing PCAP file. These filters alter the active user display pane, rendering only the targeted packets while hiding background network noise without modifying the underlying raw capture file.

The foundational rule of graphical packet forensics states: *"If you can click on an object field, you can filter, isolate, and copy its corresponding value."*

## 10. Advanced Graphical Analysis & Conversation Triage
Rather than manually writing complex syntax strings for introductory investigations, analysts utilize the contextual right-click interface menu to execute precise data isolation and formatting routines:

### A. Context-Driven Query Generation
*   **Apply as Filter:** Automatically generates and executes an absolute display filter query isolating the specific parameter or packet attribute selected by the analyst.
*   **Prepare as Filter:** Constructs the target syntax query string directly inside the Display Filter Bar but delays execution until the analyst hits Enter or chains additional conditional logical operators (`and` / `or`).
*   **Conversation Filter:** Automatically isolates an entire layer-3 or layer-4 network stream. Unlike basic filtering, it instantly maps out the source and destination IP addresses alongside the active communication port blocks to reveal the full conversation flow.
*   **Colorize Conversation:** Applies a vibrant, session-specific background color to all packets linked to a specific conversation. This allows investigators to trace interactive data streams visually while scrolling through the main packet list pane without hiding background frames.

### B. Custom Data Layouts (`Apply as Column`)
*   `Analyze ➔ Apply as Column` — Extracts a nested data field or protocol attribute straight from the Packet Details Panel and instantiates it as a distinct vertical column inside the Packet List Pane. 
    *   *SOC Triage Application:* Highly useful for tracking specific repeating parameters (e.g., extracting HTTP User-Agent strings or TLS Cipher Suites) to audit their prevalence or spot statistical anomalies instantly across thousands of consecutive packets.

## 11. Application-Layer Session Reconstruction (`Follow Stream`)
Network communication is naturally fragmented across discrete packet segments at the transport layer. To analyze traffic from the perspective of an application or a user transaction, analysts reconstruct data streams cleanly:

*   `Analyze ➔ Follow [TCP/UDP/HTTP] Stream` — Assembles fragmented packets sequentially to reconstruct the raw application-layer data exchange. It renders the entire session transaction inside an independent viewing console.
    *   *Visual Interpretation:* Traffic originating from the client terminal is highlighted in **Red**, while responses originating from the destination server are highlighted in **Blue**.
    *   *Forensic Value:* Crucial for exposing unencrypted text-based protocol operations. This interface allows threat hunters to read plaintext login strings, intercept active terminal commands, or review extracted web pages exactly as they appeared to the user.
    *   *Session Recovery Note:* Following a stream automatically applies a localized display filter limiting the workspace to that explicit conversation thread. Analysts click the **"X" button** on the right side of the filter bar to drop the constraint and restore visibility across all packets.

## 12. Primitive Display Filter Syntax Queries
When manual right-click filters are insufficient, analysts construct direct, logic-based query statements inside the filter bar to isolate targeted protocol groups:

### A. Protocol-Based Ingress Sweeps
Typing a plain protocol name isolates traffic bound to that distinct suite, removing all secondary system noise:
```text
arp     ➔ Displays exclusively Address Resolution Protocol frames.
http    ➔ Isolates unencrypted Layer 7 web transactions.
ftp     ➔ Filters for File Transfer Protocol control connections.
```

### B. Symmetrical Layer-4 Port Identification
To extract traffic passing over specific operational channels, investigators target layer-4 ports using strict relational operators:
```powershell
tcp.port == 80     # Restricts the pane to standard HTTP web traffic connections.
udp.port == 53     # Isolates baseline DNS resolution lookup streams.
```

### C. Layer-3 Address Isolation
To hunt for all traffic involving a suspected compromised endpoint, defenders sweep the network layer for a specific IP mapping:
```powershell
ip.addr == 192.168.1.2   # Returns any packet where the IP is the source OR destination.
```
