# Comprehensive Lab Report: Command-Line Packet Analysis & Network Sniffing via Tcpdump

## 1. Tactical Implementation: The CLI Sniffing Engine
**Tcpdump** is a lightweight, command-line network packet analyzer built on top of the native **`libpcap`** library (ported to Windows environments as `winpcap`) [INDEX]. Written entirely in C and C++, it has served as a foundational infrastructure security utility since the early 1990s [INDEX].

### Strategic Defensive Advantages:
*   **Optimal Speed & Performance:** Executes minimal code parsing loops, allowing high-velocity frame captures on production systems without exhausting volatile memory allocations [INDEX].
*   **Headless Environment Availability:** Operates entirely from a CLI terminal shell, eliminating the requirement for a heavy Graphical User Interface (GUI) engine [INDEX].
*   **Forensic File Portability:** Captures raw wire conversations seamlessly and packages the network frames into standard `.pcap` formats, allowing analysts to securely download files for deep-dive parsing inside Wireshark later [INDEX].

## 2. Ingress Interface Selection & Capture Management
Running `tcpdump` without arguments verifies installation parameters but floods the screen with system noise [INDEX]. Analysts must execute precise command-line arguments to scope the triage window effectively [INDEX]:

*   `tcpdump -i <interface>` — Binds the sniffer engine to a specific connections point [INDEX]. Investigators identify available software and hardware interface targets (such as `eth0` or `ens5`) using the terminal network utility wrapper `ip address show` (abbreviated as **`ip a s`**) [INDEX]. To sniff traffic across all combined communication lanes simultaneously, engineers execute:
    ```bash
    sudo tcpdump -i any
    ```
*   `tcpdump -c <count>` — Restricts frame ingestion to a hard numerical boundary, automatically closing the collection thread once the threshold is satisfied (e.g., `-c 5` halts capturing immediately after the fifth packet is recorded) [INDEX]. Bypassing this flag keeps the sniffer active indefinitely until manually broken via a `Ctrl + C` terminal hardware interrupt [INDEX].

## 3. Storage Optimization & I/O Lifecycle Primitives
*   `tcpdump -w <filename.pcap>` (Write File) — Diverts live traffic ingestion directly onto a static physical storage file [INDEX]. This completely suppresses the real-time display of text lines scrolling down the terminal window, maximizing write speeds and saving computing cycles during large-scale network logging tasks [INDEX].
*   `tcpdump -r <filename.pcap>` (Read File) — Instructs the engine to read, parse, and display network frames straight out of an existing packet capture log [INDEX]. This primitive allows threat hunters to apply retroactive filters over an historical attack file without recreating live network conditions [INDEX].

## 4. Telemetry Format Controls & Domain Resolution Suppression
By default architectural design, `tcpdump` attempts to reverse-resolve network values to print friendly DNS domain fields and protocol names [INDEX]. In incident response scenarios, this layout creates processing latency and obfuscates critical raw technical indicators [INDEX].

### A. Raw Notation Switches
*   `-n` — Suppresses network address translation loops, forcing the console to print absolute numerical **IP addresses** immediately [INDEX].
*   `-nn` — Disables both DNS lookups and protocol port mappings (e.g., preventing TCP port `80` from being renamed to `http`, or port `22` to `ssh`), preserving pure numerical attributes for exact log forensic audits [INDEX].

### B. Verification Triage Syntax Breakdown
```bash
sudo tcpdump -i ens5 -c 5 -n
```
*   *Live Wire Output Interpretation:*
    ```text
    08:55:18.989213 IP 10.10.117.2.22 > 10.11.81.126.53378: Flags [P.], seq 2888580014:2888580210...
    ```
    The analyzer engine captures precise UNIX execution timestamps, identifies the layer-3 routing parameters, maps the active TCP control flags (such as the `[P.]` Push-Acknowledgment indicator), records raw sequence numbers, and logs the explicit byte payload lengths cleanly [INDEX].

*   `-v` / `-vv` / `-vvv` (Verbose Output Execution) — Increments packet data decoding depth [INDEX]. Appending verbose flags instructs the extraction parser to display underlying layer-3 IP attributes including **Time-to-Live (TTL)** metrics, unique identification headers, fragment flags, and structural encapsulation options [INDEX].

---

## 5. Berkley Packet Filtering (BPF): Host Isolation Mechanics
To extract data points from extensive background chat and eliminate system noise, investigators leverage targeted **Host Filter Statements** [INDEX]:

*   `tcpdump host <IP_or_Hostname>` — Restricts packet ingestion strictly to traffic lines where the designated entity acts as either the source or destination endpoint [INDEX].
    ```bash
    sudo tcpdump host example.com -w http.pcap
    ```
*   `tcpdump src host <IP_or_Hostname>` — Scopes the capture field down exclusively to packets originating *from* the target source identity [INDEX].
*   `tcpdump dst host <IP_or_Hostname>` — Isolates incoming traffic traveling *to* the targeted destination perimeter network socket [INDEX].
## 6. Granular Traffic Scoping: Port, Protocol & Compound Boolean Logic
When triaging enterprise-scale captures containing thousands or millions of frames, a security analyst uses precise protocol and channel restrictions to isolate anomalous data patterns rapidly:

### A. Layer 4 Port-Based Filtration
*   `tcpdump port <Port_Number>` — Isolates incoming and outbound traffic traversing a specific system channel. For example, running `sudo tcpdump -i ens5 port 53 -n` instantly captures all raw **DNS queries and responses**, unmasking IPv4 **A record lookups** and IPv6 **AAAA record lookups** across the segment.
*   `tcpdump src port <Port_Number>` — Scopes traffic lines down exclusively to frames originating *from* the specified source socket.
*   `tcpdump dst port <Port_Number>` — Restricts tracking parameters strictly to packets traveling *to* the designated target application port.

### B. Protocol Identification Sweeps
The packet engine parses frame configurations natively to isolate traffic matching explicit protocol keywords:
*   Keywords include: `ip`, `ip6`, `udp`, `tcp`, and `icmp`.
*   *Triage Value:* Executing `sudo tcpdump -i ens5 icmp -n` drops background noise to isolate **ICMP echo requests and replies** (ping signatures) or identify **ICMP time exceeded in-transit messages** (traceroute indicators) to spot scan routines.

### C. Compound Boolean Operators
Multiple search dimensions are chained together to build advanced network forensic search queries:
*   **`and` (Conjunction):** Matches strictly when both criteria evaluate as true (e.g., `tcpdump host 1.1.1.1 and tcp` isolates TCP traffic involving that exact host).
*   **`or` (Disjunction):** Returns frames matching either of the designated parameters (e.g., `tcpdump udp or icmp`).
*   **`not` (Negation):** Reverses evaluation parameters to filter out specific noisy protocol groups (e.g., running `tcpdump not tcp` removes standard stream noise to highlight UDP, ICMP, and local ARP frames).

## 7. Forensic Data Extraction & Line-Counting Automation Pipeline
Analyzing static capture logs does not require root privileges. Analysts read raw packet blocks using the **`-r` flag** and pipe the output to shell utilities to perform high-velocity statistical audits:

```bash
tcpdump -r traffic.pcap src host 192.168.124.1 -n | wc
```
*   *SOC Analysis Impact:* The command extracts all numerical frames originating from a suspicious host IP address. By piping the text stream into the **Word Count (`wc`)** utility, the system calculates the absolute total number of matching frames instantly, providing a rapid approach for mapping connection volumes, detecting denial-of-service (DoS) attempts, or checking brute-force login frequencies.
*   *Size Filtering Parameters:* Long-form captures can be filtered based on the physical size of individual frames using:
    *   `greater <LENGTH>` — Isolates packets with a layer-3 weight greater than or equal to the defined byte boundary.
    *   `less <LENGTH>` — Retains exclusively lightweight packets sitting beneath or equal to the designated byte limit.

## 8. Advanced Packet Inspection: Bitwise Operations & Header Byte Offsets
For advanced threat hunting or custom attack signature creation, defenders use the **`pcap-filter`** engine to analyze specific binary bits inside individual protocol headers.

### A. The Primitive Bitwise Logic Gate
Binary operations interrogate data at the bit level (0s and 1s), parsing headers to evaluate security criteria:
*   **`&` (Bitwise AND):** Yields a 1 exclusively if both input bit streams match as 1. Used to mask out unaligned bits and isolate specific flag fields.
*   **`|` (Bitwise OR):** Yields a 1 if either of the input bits is flagged as 1.
*   **`!` (Bitwise NOT):** A logical inverter that flips bits (turning a 1 to 0 and a 0 to 1).

### B. Structural Header Byte Offset Syntax
To inspect low-level data configurations inside any networking header (such as ARP, IP, or TCP), analysts utilize the raw byte slice syntax schema:
```text
proto[expr:size]
```
*   **`proto`:** Declares the target architecture layer (e.g., `ip`, `ip6`, `tcp`, `udp`, `icmp`, `ether`).
*   **`expr`:** Establishes the exact **Byte Offset** to evaluate, where an integer of `0` targets the absolute first byte of the header array.
*   **`size`:** Pinpoints the precise scope of bytes to read (accepting 1, 2, or 4 bytes, defaulting to 1 byte if omitted).
## 9. Contextual Bitwise Offset Masking Examples
To fully master the low-level byte slice query structure (`proto[expr:size]`), investigators leverage precise mathematical comparisons using bitwise operators to unmask protocol options or layer-2 destination groups [INDEX]:

*   **Multicast Hardware Ingress Discovery (`ether[0] & 1 != 0`):**
    *   *Forensic Mechanic:* Extracts the absolute first byte (offset 0) of the Layer 2 Ethernet frame header and executes a bitwise `AND` comparison against the binary integer `1` (`00000001`) [INDEX]. If the least significant bit resolves as non-zero, the filter evaluates as true [INDEX]. This allows defenders to isolate **Multicast Ethernet traffic** targeting a synchronized group of devices simultaneously [INDEX].
*   **IP Header Options Auditing (`ip[0] & 0xf != 5`):**
    *   *Forensic Mechanic:* Pulls the first byte of the Layer 3 IP header (which packs the Version and Internet Header Length fields together) and applies a bitwise `AND` mask using the hexadecimal value `0xf` (`00001111`) [INDEX]. Because a standard, unmanipulated IPv4 header holds an absolute length value of exactly 5 words (20 bytes), any output that does **not equal 5** (`!= 5`) instantly flags an IP packet carrying non-standard, advanced routing options or protocol anomalies [INDEX].

## 10. Targeting Layer 4 Connection States via TCP Flags
Rather than calculating complex manual byte offset values for common tasks, the `pcap-filter` engine provides built-in text-based primitives to query the specialized control flags field directly using the **`tcp[tcpflags]`** array [INDEX]:

### Explicit State Isolation Queries:
*   `tcp[tcpflags] & tcp-syn != 0` — Isolates incoming synchronization attempts to track down connection flooding or network mapping loops [INDEX].
*   `tcp[tcpflags] & tcp-ack != 0` — Filters for standard acknowledgement acknowledgments to audit established communication channels [INDEX].
*   `tcp[tcpflags] & tcp-fin != 0` — Tracks standard session teardown routines [INDEX].
*   `tcp[tcpflags] & tcp-rst != 0` — Catches forced session reset packets, a critical indicator when hunting for active port scanning campaigns or network path disruptions [INDEX].
*   `tcp[tcpflags] & tcp-push != 0` — Filters for immediate buffer push triggers running across data streams [INDEX].

---

## 11. Low-Level Terminal Output Rendering & Dissection Switches
When reading static file captures (`-r`) or monitoring a live network link (`-i`), defenders apply specialized output manipulation switches to customize formatting, unmask packet payload text, or dump raw binary buffers [INDEX, INDEX]:

### A. Summary and Structural Layout Modifiers
*   **`-q` (Quick Brief Output):** Suppresses comprehensive protocol decoding lines to print a dense, single-line telemetry summary [INDEX]. It limits display fields purely to the timestamp log, source/destination layer-3 routing properties, and layer-4 port mappings [INDEX].
*   **`-e` (Display Link-Level Header):** Forces the print layout to extract and display the physical **Source and Destination MAC Addresses** alongside the ethertype flags, enabling immediate Layer 2 tracking and spoofed interface auditing [INDEX].

### B. Payload Decoding & Data Representation Modifiers
*   **`-A` (Display Packets as ASCII Text):** Translates raw payload bytes directly into standard ASCII alphanumeric characters [INDEX]. This provides an optimized method for scanning plain-text protocol streams (such as unencrypted HTTP GET methods or cleartext credentials) straight from the console line [INDEX].
*   **`-xx` (Display Packets as Hexadecimal Byte Maps):** Prints an exhaustive, octet-by-octet hexadecimal byte layout tracking the entire structure of the packet, including all low-level Layer 2 and Layer 3 header fields [INDEX]. This is a vital resource when checking compressed, encrypted, or non-English data streams where ASCII rendering fails [INDEX].
*   **`-X` (Combined Hex and ASCII Layout):** The ultimate double-pane dissection view [INDEX]. It splits the terminal console output into a synchronized parallel layout, printing the raw hexadecimal structural blocks on the left side while rendering the decoded ASCII text alignment on the right side to maximize byte-level visibility [INDEX].

```text
  [ tcpdump -X Forensic Dissection Interface View ]
  18:59:59.979771 IP 104.18.12.149.https > g5000.45248: Flags [P.], length 25
        0x0000:  4500 004d fbd8 4000 3506 d229 6812 0c95  E..M..@.5..)h...  <── [IP Header] [INDEX]
        0x0010:  c0a8 4259 01bb b0c0 a0b1 037c aa3b 357d  ..BY.......|.;5}  <── [TCP Sockets] [INDEX]
        0x0020:  8018 0010 f905 0000 0101 080a 189a 310d  ..............1.
        0x0030:  ebfa 6b2e 1703 0300 146a 8f33 1832 e6a2  ..k......j.3.2..  <── [TLS Payload] [INDEX]
```
