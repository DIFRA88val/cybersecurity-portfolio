# 🛡️ Cybersecurity Operations Portfolio

A comprehensive portfolio documenting my hands-on labs, defensive security projects, and SOC analysis configurations.

## 👨‍💻 About Me

Ambitious Australian Citizen and aspiring SOC Analyst equipped with hands-on foundations in network security, incident response, and system administration. Backed by formal certifications from Cisco and Google, combined with continuous practical labs via TryHackMe to bridge theoretical knowledge with real-world technical execution.

* 🌐 **Location:** Melbourne, Victoria (Open to Remote / Hybrid)
* 💼 **Targeting:** Junior SOC Analyst / Tier 1 Security Analyst Roles
* ✉️ **Contact:** valerio.difrancesco88@gmail.com
* 🔗 **Connect:** [My LinkedIn Profile](https://linkedin.com)

---

## 🛠️ Technical Competencies

| Domain | Tools & Technologies |
| :--- | :--- |
| **Security Operations** | SIEM Log Analysis, Alert Triage, Incident Response Fundamentals |
| **Analysis & Detection** | Wireshark Packet Inspection, Splunk Triage, Snort (IDS concepts) |
| **Infrastructure & Filtering** | Linux CLI, Windows Event Logs, Windows CMD Telemetry, SQL Queries |
| **Core Frameworks** | CIA Triad, NIST CSF / SP 800-30, Threat Modeling, Identity Management (IAM) |
| **Automation & Dev** | Python Scripting, JavaScript Basics, Node.js Automation Baselines |
| **Offensive Security** | Penetration Testing Methodology, Gobuster Enumeration, Hydra Brute-Forcing |

---

## 📁 Practical Lab Directory

### 🟦 1. Incident Response, Telemetry & Log Analysis
* **Windows Event Log Analysis** — Investigating privilege escalation, tracking specific Event IDs, and analyzing malicious operational behavior.
* **Linux File Path & Auth Log Filtering** — Using regex and core command-line utilities (`grep`, `awk`, `sed`) to parse authentication logs and identify brute-force indicators.
* **Windows CMD Fundamentals** — Navigating system structures, unmasking hidden malware indicators, and collecting host telemetry using the native Command Prompt.
* **Linux OS Security Fundamentals** — Deploying secure SSH configurations, auditing terminal command histories, and managing multi-user access permissions inside unhardened environments.
* **Search Skills & Threat Intelligence** — Utilizing advanced search operators (Google Dorking), leveraging specialized threat infrastructure indices (`Shodan`, `VirusTotal`), and executing terminal lookups with Linux manual pages (`man`).
* **[Linux Core Fundamentals Master Matrix (Parts 1-3)](./1-SOC-Foundations/linux-fundamentals-matrix.md)** — Deep dive into system identity commands, high-efficiency log fuzzing (`grep`/`find`), system redirection streams (`>>`), session switching layouts (`su -l`), absolute octal permission math (`755`/`644`), and a security breakdown of the root file layout (`/etc`, `/var/log`, `/tmp`).
* **[Windows Architecture & Diagnostics Matrix (Parts 1-3)](./Windows-Basics/windows-fundamentals-matrix.md)** — Documenting the structural core of **NTFS journaling fields**, setting granular **DACL permissions**, uncovering hidden parameters via **Alternate Data Streams (ADS)**, implementing **User Account Control (UAC)** split-token isolation, tracking network sockets with **Resource Monitor**, and auditing baseline applications via **MSConfig**.
* **[Windows Commands Reference](./1-SOC-Foundations/Windows-Basics/windows-commands-reference.md)** — Documenting advanced environment path validation configurations (`set`), deep host asset discovery parameters (`systeminfo`), forensic socket mapping arrays (`netstat -abon`), recursive system path exploration (`dir /a /s`), targeted threat filtering workflows (`tasklist /FI`), and automated hardware verification arrays (`sfc /scannow`).
* **[PowerShell Automation Matrix](./1-SOC-Foundations/Windows-Basics/powershell-automation-matrix.md)** — Investigating object-oriented shell architectures, auditing custom system commands utilizing the standardized **Verb-Noun Syntax Schema**, managing secure module installations via **GPG verification rules**, building complex data pipelines with **comparison operators and filtering parameters (`Where-Object`, `Select-String`)**, collecting volatile dynamic telemetry (**Processes, Services, and Active TCP Port Sockets**), unmasking hidden malware parameters via **Alternate Data Streams (ADS)**, and engineering enterprise remote triage sweeps utilizing **`Invoke-Command` remote script block deployment**.
* **[Linux Shell Environments & Scripting](./1-SOC-Foundations/linux-shells-and-scripting.md)** — Investigating modern interactive shell variations (**Bash vs. Fish vs. Zsh**), analyzing **Shebang header parameters (`#!`)**, enforcing user space execution rights (`chmod +x`), building automated data loops, and engineering resilient, multi-variable authentication scripts utilizing conditional statements (`if/elif/else`) and logical conjunctions (`&&`).
* **[Network Traffic Dissection & Wireshark Forensics](./1-SOC-Foundations/Network-Basics/wireshark-forensics-matrix.md)** — Implementing tri-pane wire forensics (Packet List, Details, and Bytes panes), mapping custom permanent protocol coloring rules, auditing capture metadata properties, dissecting multi-layer OSI encapsulation models from Layer 1 Frames down to Layer 7 payloads, executing regex pattern string searches for attack signatures, extracting malicious network drop-files using **Export Objects**, normalizing captures into UTC chronological timelines, and triaging network anomalies using the **Wireshark Expert Info Severity Matrix (`Chat`, `Note`, `Warn`, `Error`)**.
* **[Command-Line Packet Analysis & Tcpdump Matrix](./1-SOC-Foundations/Network-Basics/tcpdump-forensics-matrix.md)** — Implementing high-velocity network interface sniffing (`tcpdump -i any`), optimizing data logging via raw file writing (`-w`), suppressing DNS lookup bottlenecks (`-nn`), tracking specific layer-4 ports and protocols (`port 53`, `icmp`), building automated frame-counting verification loops (`wc -l`), engineering advanced bitwise header byte offset queries (`ether[0]`, `ip[0]`), isolating specialized **Layer 4 TCP Control Flags (`tcp-syn`, `tcp-rst`, `tcp-fin`)**, and unmasking payload data structures utilizing parallel **Hexadecimal and ASCII double-pane decoding switches (`-X`)**.

### 🟦 2. Identity Management & Enterprise Directory Security
* **[Active Directory Domain Services (AD DS) Infrastructure](./Identity-Management/active-directory-basics.md)** — Documenting centralized identity scaling models, auditing **Security Principals (Users, Group Profiles, and Machine Accounts)**, implementing the principle of least privilege through **Organizational Unit (OU) Control Delegation**, establishing rigid **GPO Hardening Templates** (Prohibiting Control Panel Access and enforcing **Inactivity Session Lockouts**), and managing network authentication protocols (**Kerberos vs. NetNTLM**) alongside multi-domain forest trust architectures.

### 🟦 3. Networking Architecture & Perimeter Defense
* **[ISO/OSI 7-Layer Networking Architecture](./1-SOC-Foundations/Network-Basics/osi-model-framework.md)** — Documenting the comprehensive **7-Layer OSI Reference Architecture**, analyzing layer-3 logical addressing configurations (**IPv4 CIDR mappings and RFC 1918 Private Ranges**), tracing the execution of the stateful **TCP Three-Way Handshake (`SYN`, `SYN-ACK`, `ACK`)**, documenting downward **Data Encapsulation lifecycles**, and executing manual layer-7 application banner grabs using raw teletype network streams (**`telnet` protocol over Ports 7, 13, and 80**).
* **[Networking Essentials Matrix](./1-SOC-Foundations/Network-Basics/networking-essentials-matrix.md)** — Documenting automated asset allocation lifecycles using the **DHCP "DORA" Sequence**, parsing Layer 2 address translation frames via **Address Resolution Protocol (ARP)**, executing tactical path enumeration using **ICMP Type 11 Time Exceeded routing behavior (`traceroute`)**, analyzing convergence protocols (**OSPF, BGP, RIP**), and dissecting perimeter connection tracking workflows utilizing **Stateful NAT Translation Tables**.
* **[Network Core Protocols Manual](./1-SOC-Foundations/Network-Basics/network-core-protocols.md)** — Documenting **Layer 7 DNS record types (`A`, `AAAA`, `CNAME`, `MX`)** and tracking query lookups via `tshark`, auditing asset ownership using **WHOIS directory trails**, parsing unrendered web application headers and **HTTP/HTTPS verbs**, evaluating dual-channel data segregation over **FTP**, and dissecting cleartext communication risks across legacy email routing infrastructures (**SMTP, POP3, and IMAP**).
* **[Network Secure Protocols & TLS Matrix](./1-SOC-Foundations/Network-Basics/network-secure-protocols.md)** — Documenting the historical evolution from cleartext transmission states to **Transport Layer Security (TLS 1.3)** boundaries, implementing **Public Key Infrastructure (PKI) digital certificate verification**, parsing unrendered **HTTPS encrypted application data blocks**, analyzing secure terminal authentication controls (**OpenSSH, asymmetric key verification, and X11 visual forwarding**), mapping secure file moving constraints (**SFTP vs FTPS over TCP Port 990**), evaluating enterprise network encapsulation tunnels (**VPN Virtualization vs Privacy**), and executing localized **Wireshark TLS Pre-Master-Secret key decryption forensics**.


### 🟦 4. Application Security & Infrastructure Baselines
* **Web Infrastructure & Requests** — Dissecting URL syntax anatomy, parsing request/response transactional headers, and auditing server status codes.
* **Introduction to Web Development & Client-Side Flaws** — Analyzing frontend source code disclosure, dissecting DOM execution, and documenting input sanitization vulnerabilities like HTML Injection.
* **Full-Stack Enterprise Web Architecture** — Evaluating edge distribution infrastructure (Load Balancers, CDNs, WAFs), auditing web server daemons, and tracking static vs. dynamic backend boundaries.
* **Data Representation & Encoding** — Investigating machine-level structures including binary, hexadecimal, ASCII matrices, multi-byte translation anomalies, and regional character set variations.
* **Database Infrastructure & SQL Filtering** — Relational table architecture management and engineering filtered/sorted data queries (`SELECT`, `WHERE`, `ORDER BY`).

### 🟦 5. Automation & Scripting
* **Python Automation & Scripting** — Developing functional tools, managing environment variables, and tracing conditional logic flows (`if/elif/else`) to build automation baselines.
* **JavaScript Foundations & Node.js** — Investigating client-side vs. server-side execution frameworks and utilizing Node.js for terminal task automation.

### 🟦 6. Threat Modeling & Governance
* **NIST SP 800-30 Risk Assessment Simulator** — Executing quantitative risk calculations, calculating ALE/SLE metrics, and implementing corporate auditing workflows.
* **The Foundations of Security (CIA Triad)** — Implementing the core pillars of security risk posture alignment, analyzing compromise models, and establishing defensive priority triage mindsets.
* **Symmetric Encryption & Cryptography** — Dissecting plaintext/ciphertext transformations, analyzing legacy shift ciphers, and evaluating symmetric key distribution limitations.

### 🟦 7. Offensive Security & Penetration Testing
* **Introduction to Offensive Security & Ethical Hacking** — Dissecting pentesting terminology (Red Teaming, Scope, Vulnerabilities, Exploits) and utilizing automated tools like `gobuster` for web content discovery and directory enumeration.

### 🟦 8. Defensive Security Architecture & Asset Visibility
* **Introduction to Defensive Operations & Asset Mapping** — Analyzing enterprise environments through architectural mapping methodologies. Applying the fundamental operational lifecycles of **Prevention, Detection, Mitigation, Analysis, and Response** to secure client infrastructure boundaries.
* **Blue Team Infrastructure Security** — Mapping conceptual asset topologies to real-world infrastructure equivalents, establishing log baseline architectures, and identifying anomalous indicators across employee endpoints, web services, and firewall perimeters to maintain the **CIA Triad**.

---

## 📈 Professional Roadmap & Training

* 🏆 **Google Cybersecurity Professional Certificate** — Completed
* 🏆 **Cisco Networking Basics** — Completed
* 🏆 **Google IT Support Professional Certificate** — Completed
* ⚡ **TryHackMe Pre-Security & SOC Level 1** — *In Progress*


