# Comprehensive Lab Report: Network Reconnaissance, Host Discovery & Port Enumeration via Nmap

## 1. Tactical Implementation: The Asset Discovery Engine
**Nmap (Network Mapper)** is an open-source command-line framework engineered explicitly for network discovery, vulnerability auditing, and attack surface enumeration [INDEX]. It operates by transmitting customized raw IP packets across network interfaces to deduce the characteristics, operating system versions, and structural defense baselines of remote endpoints [INDEX].

### Scope Target Notation Primitives:
Before executing a reconnaissance sweep, an analyst configures precise target parameters inside the CLI input block [INDEX]:
*   **IP Range Specification (`-`):** Scopes the query across an explicit sequential address block (e.g., `192.168.0.1-10` scans all addresses from `.1` directly to `.10`) [INDEX].
*   **Subnet CIDR Notation (`/`):** Sweeps an entire logical subnetwork block (e.g., `192.168.0.1/24` instructs the engine to process all 256 addresses from `.0` to `.255`) [INDEX].
*   **Canonical Hostname:** Targets an asset using its registered domain string (e.g., `example.thm`), forcing an underlying DNS resolution lookup prior to scanning [INDEX].

## 2. Low-Level Host Discovery Subsystems (Ping Sweeps)
The **Ping Scan (`-sn`)** flag configures Nmap to discover active, online devices across a network block without attempting downstream port or service version identification [INDEX]. This provides a low-overhead methodology for mapping out active hardware assets while minimizing network noise [INDEX].

### A. Local Network Segments (Layer 2 Discovery)
When executing a ping sweep over a directly connected network segment (sharing a common local area connection or Wi-Fi link), Nmap bypasses standard internet protocol probes [INDEX]:
*   *The Ingress Mechanic:* The engine transmits raw **Address Resolution Protocol (ARP) Requests** to every address in the target subnet range [INDEX]. Any host that returns an active ARP reply is immediately logged as an online asset ("Host is up") [INDEX].
*   *Forensic Mapping Value:* Because local link layers expose raw MAC addresses, the engine automatically extracts the **Hardware MAC Address** and maps it against global vendor OUI registries [INDEX]. This lets the analyst instantly identify the device manufacturer (e.g., identifying Espressif or Tuya Smart modules) to deduce the hardware profile of the target asset [INDEX].

```text
  [ ATTESTATION MONITOR ] ═══════ ARP Request: "Who has 192.168.66.1?" ══════> [ LOCAL TARGET ]
                          <────── ARP Reply: "192.168.66.1 is-at [MAC]" ──────  (Host is Up) [INDEX]
```

### B. Remote Network Segments (Layer 3 Discovery)
When targeting a remote network block separated from the scanner by one or more intermediate gateway routers, Layer 2 ARP requests cannot cross the routing boundary [INDEX]. Nmap escalates its discovery engine to run a multi-protocol Layer 3 sweep [INDEX]:
*   *The Multi-Probe Matrix:* For every target IP, Nmap initializes a coordinated discovery string containing an **ICMP Type 8 Echo Request (Ping)**, an **ICMP Type 13 Timestamp Request**, a high-velocity **TCP SYN packet to Port 443**, and a speculative **TCP ACK packet to Port 80** [INDEX].
*   *The Ingress Triage Output:* If a live host responds to any of these layered probes (e.g., returning an ICMP Echo Reply), it is recorded as live [INDEX]. If a host is dead or behind a hard firewall perimeter, the requests drop silently, often triggering an *ICMP Destination Unreachable (Type 3)* error message from intermediate gateway routers [INDEX].
*   **List Scan Pre-Verification (`-sL`):** Pre-runs a raw enumeration that prints out the full target list block without transmitting any active packets to the targets, allowing architects to verify the boundaries of their target scope before initializing noisy network sweeps [INDEX].

---

## 3. Active Port Scanning Architecture: Mapping Listening Daemons
Once an analyst identifies active hosts, they transition to port enumeration to isolate listening network services (processes bound to specific TCP or UDP ports, such as web applications on ports 80/443 or DNS daemons on port 53) [INDEX].

### A. The TCP Connect Scan (`-sT`)
The **TCP Connect Scan** is the standard baseline port scanning method used when an account lacks administrative privileges on the local scanning machine [INDEX].
*   *The Ingress Mechanic:* The engine directs the local operating system network stack to complete a full **TCP Three-Way Handshake** (`SYN` ➔ `SYN-ACK` ➔ `ACK`) with every target port sequentially [INDEX].
*   *Triage Diagnostics (Open vs. Closed):* 
    1.  **Open Port Behavior:** The target port returns a `SYN-ACK` block, Nmap completes the handshake with an `ACK` to confirm the open port state, and then immediately tears down the connection thread using a reset packet (**`RST-ACK`**) [INDEX].
    2.  **Closed Port Behavior:** The target port instantly rejects the connection attempt by returning a **`RST-ACK`** packet, signaling to Nmap that no service is active [INDEX].
*   *SOC Defensive Caveat:* Because a full handshake is successfully established on open ports, this scan forces the target application to log the connection event, making it highly visible to monitoring engines [INDEX].

### B. The TCP SYN Stealth Scan (`-sS`)
The **TCP SYN Scan** requires elevated administrative credentials (`sudo` or root) to execute, functioning as Nmap's default high-efficiency scan archetype [INDEX].
*   *The Ingress Mechanic:* Known as a **Half-Open Scan** [INDEX]. Nmap bypasses the system network stack to generate custom raw packets, transmitting a single **`SYN`** packet to the target port [INDEX]. 
*   *The Evasion Edge:* If the target port responds with a `SYN-ACK`, Nmap logs the port state as `OPEN`. However, instead of transmitting the final `ACK` packet to complete the three-way handshake, Nmap terminates the connection instantly by shooting a **`RST`** packet across the wire [INDEX]. Because the connection loop never enters an established state, the transaction typically bypasses standard application-level logging mechanisms, reducing the footprint left behind inside target logs [INDEX].
## 4. UDP Port Auditing Architecture (`-sU`)
While the vast majority of enterprise application endpoints utilize stateful TCP communication, critical network infrastructure components—including **DNS (53), DHCP (67/68), NTP (123), and SNMP (161/162)**—rely entirely on the connectionless **User Datagram Protocol (UDP)**.

### The Operational Challenge & Triage Mechanics:
*   **The Ingress Process:** Triggered utilizing the **`-sU` flag**. Because UDP does not implement a stateful handshake framework, Nmap transmits a raw, protocol-specific UDP packet straight to the target port and listens for a response.
*   **Open vs. Closed Port States:**
    1.  **Closed Port Telemetry:** If a target UDP port is offline or closed, the remote operating system kernel intercepts the packet and drops back an **ICMP Destination Unreachable / Port Unreachable (Type 3, Code 3)** error message.
    2.  **Open Port Telemetry:** If the port is open and listening, most standard applications drop no response at all, or return a protocol-specific payload response. Because a lack of response can mimic packet-filtering firewalls, Nmap often flags unresponding UDP slots as `open|filtered`.

## 5. Scope Optimization: Targeting Specific Port Perimeters
By default, Nmap queries the **top 1,000 most common ports** registered in its services database. To limit scanning latency and reduce the footprint left behind inside target firewall logs, an analyst can restrict target parameters:

*   **Fast Mode (`-F`):** Trims the scanning matrix down to evaluate exclusively the **100 most common ports**, heavily accelerating the reconnaissance timeline.
*   **Granular Port Ranges (`-p`):** Enforces strict, user-defined port boundaries:
    *   `-p10-1024` — Restricts the query to run from port 10 straight to port 1024.
    *   `-p1-1023` — Sweeps the industry standard **Well-Known Ports** block where critical root enterprise services reside.
    *   `-p-` — Instructs the scanner to evaluate the entire physical range of **all 65,535 TCP/UDP ports** sequentially, maximizing thoroughness during a deep compromise assessment.

## 6. Advanced Fingerprinting: Operating System & Application Version Detection
To evaluate the overall security posture and build patch timelines, security auditors move beyond open port numbers to identify the exact operating system versions and software editions running on the host.

### A. Automated OS Detection (`-O`)
*   *The Mechanic:* Enforced utilizing the **`-O` flag**. Nmap transmits a battery of raw TCP and UDP packets to both open and closed ports, analyzing subtle variations in how the remote network stack handles TCP window sizing, TTL configurations, and initial sequence numbers to make an educated guess about the target OS.
*   *Forensic Interpretation:* It generates a Common Platform Enumeration (**CPE**) string string (e.g., `cpe:/o:linux:linux_kernel:5`), revealing the macro-level operating family and device types.

### B. Service Version Introspection (`-sV`)
*   *The Mechanic:* Enforced utilizing the **`-sV` flag**. Rather than blindly trusting standard port assignments (e.g., assuming port 22 is always standard SSH), Nmap initiates active protocol handshakes and transmits targeted banner-grabbing queries straight to open sockets.
*   *SOC Triage Value:* The response maps out a dedicated **VERSION** column inside the terminal display console. This isolates the precise software variant (e.g., identifying `OpenSSH 8.9p1 Ubuntu`). Security analysts use this metadata to cross-reference exposed software instances against global **Common Vulnerabilities and Exposures (CVE)** databases to flag immediate exploitation risks.

### C. Aggressive Aggregated Scans (`-A`)
*   **The Power Directive:** Appending the **`-A` flag** bundles multiple heavy exploration engines into a single automated execution pass. It triggers **OS Detection (`-O`), Service Version Scanning (`-sV`), Script Scanning (`-sC`), and automated Traceroute path enumeration** simultaneously, providing an exhaustive host evaluation.

## 7. Perimeter Evasion: Bypassing Host Discovery Rules (`-Pn`)
*   **The Problem:** By default, Nmap launches a preliminary host discovery phase (e.g., executing a ping check) before running a port scan. If an enterprise edge firewall or a local host firewall drops inbound ICMP requests silently, Nmap labels the target as offline and skips the scan entirely.
*   **The Mitigation (`-Pn`):** The **`-Pn` flag** instructs Nmap to **completely skip the host discovery phase and treat all target hosts as actively online**. This forces the engine to run full port enumeration loops directly against every target IP address, bypassing firewall drop filters to uncover listening services that would otherwise stay hidden.
## 8. Advanced Target Port Modifiers & Non-Standard Discovery (`-sU` / `-p`)
Once host boundaries are bypassed, analysts adjust their port targeting scopes to identify hidden or non-standard services:

*   **UDP Port Enumeration (`-sU`):** Scans for open UDP-bound daemons (such as DNS, DHCP, or NTP) [1.14, 1.15]. Because UDP is connectionless, open ports typically remain silent, while closed ports are identified when the target host drops an **`ICMP Destination Unreachable (Port Unreachable)`** packet back to the scanner.
*   **Target Port Restrictions (`-p`):** By default, Nmap restricts its focus to the top 1,000 most common ports [1.14]. Analysts adjust this perimeter using targeted switches:
    *   `-F` (Fast Mode) — Drops the tracking window down to scan only the top 100 most common ports.
    *   `-p1-1023` — Restricts the query explicitly to **Well-Known Ports**.
    *   `-p-` — Instructs the engine to execute an exhaustive sweep across **all 65,535 ports**.

## 9. Evasion Mechanics: Scan Timing, Parallelism & Packet Throttling
To prevent network saturation, maintain connection stability over poor links, or intentionally bypass local **Intrusion Detection Systems (IDS)**, engineers adjust Nmap's timing and packet delivery engine:

### A. The Six Serial Timing Templates (`-T0` through `-T5`)
Timing structures adjust serial packet delay intervals to control the speed and noise footprint of an active scan:

| Timing Template | Numerical Flag | Wire Operational Behavior & Retransmission Intervals |
| :--- | :--- | :--- |
| **Paranoid** | `-T0` / `-T paranoid` | Maximum stealth evasion. Nmap enforces a strict **5-minute delay** between consecutive port probes to completely fly beneath behavioral thresholds. |
| **Sneaky** | `-T1` / `-T sneaky` | Highly conservative; enforces a mandatory **15-second delay** between port checks. |
| **Polite** | `-T2` / `-T polite` | Drops serialization delays down to a baseline **0.4-second interval** per port. |
| **Normal** | `-T3` / `-T normal` | The **default execution state**. Runs dynamically as fast as the local network path allows without incurring packet drops. |
| **Aggressive** | `-T4` / `-T aggressive` | Speeds up execution windows; highly optimized for high-bandwidth, stable corporate infrastructure environments. |
| **Insane** | `-T5` / `-T insane` | Maximum velocity scan; fires simultaneous probes aggressively, risking packet drops and high log visibility. |

### B. Low-Level Throttling and Parallelism
*   `--min-parallelism` / `--max-parallelism <num>` — Manually locks down the minimum and maximum threshold of parallel port probes active simultaneously against a host group, overriding Nmap's automated adaptive loops.
*   `--min-rate` / `--max-rate <number>` — Controls packet transmission velocity strictly by enforcing a definitive **packets-per-second** transmission ceiling across the entire global scan thread.
*   `--host-timeout <time>` — Implements a hard deadline cut-off. If a sluggish or high-latency remote host fails to complete its processing loop within the specified window, Nmap drops the host to prevent the entire scan from stalling.

## 10. Live Telemetry Verbosity & Structural Output Generation
In deep security audits, tracking the real-time progression of a scan and preserving its output for documentation or programmatic log filtering is a standard operational requirement.

### A. Real-Time Processing Visibility
*   `-v` / `-vv` / `-vvv` — Increments console verbosity levels. Activating verbosity forces Nmap to print live updates as it moves through its scanning phases, unmasking open ports as soon as they are discovered rather than holding output until the final report completes.
*   `-d` (Debugging Levels up to `-d9`) — Generates low-level debugging telemetry streams. It dumps thousands of diagnostic lines detailing internal engine decisions, raw packet structures, and response timings.

### B. The Four Output Report Formats
To preserve data for reporting or downstream text parsing, Nmap supports saving scan results into distinct file structures using the **`-o` flags**:
*   **`-oN <filename>` (Normal):** Saves a clean, human-readable report file matching the standard format displayed on the terminal screen.
*   **`-oX <filename>` (XML):** Generates structured XML data files, allowing the scan results to be ingested by secondary data processing tools, web interfaces, or vulnerability management backends.
*   **`-oG <filename>` (Grepable):** Compiles a compressed, single-line per-host asset log optimized explicitly for high-efficiency command-line parsing using tools like **`grep` and `awk`**.
*   **`-oA <basename>` (All Formats):** An all-in-one automation switch. It takes a single base name and executes all three file output engines simultaneously, generating `<basename>.nmap`, `<basename>.xml`, and `<basename>.gnmap` files inside the working directory.

## 11. Consolidated Nmap Command & Flag Optimization Matrix
To support rapid reconnaissance planning, vulnerability auditing, and network tracking, the core capabilities, timing modifiers, and reporting formats covered across this module are consolidated into the structural index below:

| Operational Focus | Nmap Switch Flag | Technical Explanation & Root Privilege Requirements |
| :--- | :--- | :--- |
| **Pre-Verification** | `-sL` | **List Scan:** Enumerates all targets within the specified scope block without transmitting active packets. |
| **Host Discovery** | `-sn` | **Ping Scan:** Enforces host discovery only, skipping downstream port or service scanning entirely. |
| **Port Scanning** | `-sT` | **TCP Connect Scan:** Completes a full 3-way handshake; the default option for standard non-root users. |
| **Port Scanning** | `-sS` | **TCP SYN Stealth Scan:** Executes half-open scans; **requires root/sudo privileges** to craft raw custom packets. |
| **Port Scanning** | `-sU` | **UDP Scan:** Identifies active UDP-bound services based on missing responses or ICMP port unreachable errors. |
| **Port Scanning** | `-F` | **Fast Mode:** Caps the scan perimeter to focus strictly on the top 100 most common ports. |
| **Port Scanning** | `-p<range>` | **Port Range Selection:** Restricts the scan to targeted ports (e.g., `-p-` sweeps all 65,535 ports). |
| **Port Scanning** | `-Pn` | **Treat Hosts as Online:** Suppresses host discovery; forces full scans on assets blocking standard ICMP probes. |
| **Service Detection** | `-O` | **OS Fingerprinting:** Analyzes packet header metrics to make an educated guess regarding the remote operating system. |
| **Service Detection** | `-sV` | **Service Version Detection:** Interrogates open sockets to capture application name and software version strings. |
| **Service Detection** | `-A` | **Aggressive Engine:** Combines OS detection, version scanning, script auditing, and traceroute into a single sweep. |
| **Scan Timing** | `-T<0-5>` | **Timing Templates:** Controls serialization delays ranging from Paranoid (`-T0`, 5-min delay) to Insane (`-T5`). |
| **Scan Timing** | `--min-parallelism / --max-parallelism` | **Parallel Probe Limits:** Manually bounds the parallel active probe thread ceiling against a target host group. |
| **Scan Timing** | `--min-rate / --max-rate` | **Packet Transmission Velocity:** Enforces a rigid packet-per-second output floor and ceiling across the global scan thread. |
| **Scan Timing** | `--host-timeout` | **Deadtime Cut-Off:** Specifies the maximum duration to wait for high-latency or slow hosts before skipping them. |
| **Real-Time Output** | `-v` / `-vv` | **Verbosity Levels:** Forces the engine to print real-time progress updates and display open ports during the scan. |
| **Real-Time Output** | `-d` / `-d9` | **Debugging Output:** Dumps deep internal engine execution lines, packet structures, and calculation logs. |
| **Report Generation** | `-oN <file>` | **Normal Format:** Saves a clean, human-readable report file matching standard terminal outputs. |
| **Report Generation** | `-oX <file>` | **XML Format:** Generates structured XML data optimized for ingestion into automated vulnerability scanners. |
| **Report Generation** | `-oG <file>` | **Grepable Format:** Compiles compressed, single-line per-host outputs tailored for `grep` and `awk` scripting. |
| **Report Generation** | `-oA <base>` | **All Major Formats:** An all-in-one export switch that creates `.nmap`, `.xml`, and `.gnmap` files simultaneously. |

*   *Privilege Constraint Reminder:* Running Nmap as a standard local user without root escalation heavily limits capabilities. Without administrative rights, the tool cannot craft raw packets, forcing it to drop stealth SYN scans (`-sS`) and fall back to native, noisy TCP Connect scans (`-sT`), while blocking advanced Layer 2 ARP discovery loops entirely. Security operations engineers ensure the scanning environment is appropriately permissioned to guarantee high-accuracy asset mapping.
