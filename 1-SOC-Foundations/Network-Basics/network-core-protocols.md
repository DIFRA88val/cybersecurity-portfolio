# Comprehensive Lab Report: Network Core Protocols & Domain Infrastructure Auditing

## 1. Domain Name System (DNS) Core Architecture
The **Domain Name System (DNS)** functions as a core directory database layer responsible for translating human-readable string addresses (e.g., `www.example.com`) directly into machine-routable public IP addresses. This eliminates the operational requirement for end-users or systems to manually track individual host IP maps.

### Protocol Layer & Port Configurations:
DNS operates at the **Application Layer (Layer 7)** of the ISO/OSI model. By default architectural convention, standard DNS network transactions rely heavily on the high-speed **UDP Port 53** pipeline. However, if a payload packet size boundaries exceed standard datagram limits, or during server-to-server zone transfers, DNS automatically drops back to the stateful **TCP Port 53** transport channel to guarantee packet delivery.

### Structural DNS Record Classifications:
*   **A Record (Address):** Maps a localized hostname directly to one or more IPv4 addresses (e.g., resolving `example.com` straight to `172.17.2.172`).
*   **AAAA Record (Quad-A):** Maps a hostname directly to its corresponding modern 128-bit IPv6 address string. 
    *   *System Security Note:* The "AAAA" notation prevents structural confusion with alternate enterprise standards (such as AAA framework services tracking *Authentication, Authorization, and Accounting* metrics).
*   **CNAME Record (Canonical Name):** Maps an alias domain string directly to another true canonical domain name, forwarding resolution tracking down the line (e.g., pointing `www.example.com` natively to `example.com`).
*   **MX Record (Mail Exchange):** Specifies the authoritative mail server host responsible for receiving and processing inbound email payloads targeting the domain space.

## 2. Terminal-Based DNS Introspection & Network Packet Traversal
Security analysts query active DNS servers from a local shell terminal interface using utilities like **`nslookup`** to verify resolution pathways and isolate falsified mapping configurations.

### A. Manual Record Query Diagnostics
Executing `nslookup` interrogates local or remote nameservers, extracting the distinct IPv4 and IPv6 target allocations mapped to the canonical domain name object:
```text
user@TryHackMe$ nslookup www.example.com
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
Name:   www.example.com
Address: 93.184.215.14
Address: 2606:2800:21f:cb07:6820:80da:af6b:8b2c
```

### B. Network Traffic Packet Analysis (`tshark` Logging)
When auditing live network captures to track down malicious beaconing lookups or data exfiltration attempts, an analyst isolates DNS transaction IDs and query types using tools like `tshark`:
```text
user@TryHackMe$ tshark -r dns-query.pcapng -Nn
1 0.000000000 192.168.66.89 → 192.168.66.1 DNS 86 Standard query 0x2e0f A www.example.com OPT
2 0.059049584 192.168.66.1 → 192.168.66.89  DNS 102 Standard query response 0x2e0f A www.example.com A 93.184.215.14 OPT
3 0.059721705 192.168.66.89 → 192.168.66.1 DNS 86 Standard query 0x96e1 AAAA www.example.com OPT
4 0.101568276 192.168.66.1 → 192.168.66.89  DNS 114 Standard query response 0x96e1 AAAA www.example.com AAAA 2606:2800:21f:cb.
```
*   *SOC Analysis Mechanics:* Packet logs unmask a distinct sequential verification process. Packets 1 and 2 capture the local client host (`192.168.66.89`) initializing an explicit request tracking transaction identifier `0x2e0f` for the IPv4 **A record**, immediately receiving the response string mapping to the target IP `93.184.215.14`. Packets 3 and 4 track the subsequent parallel lookup executing for the IPv6 **AAAA record** under transaction identifier `0x96e1`.

---

## 3. Asset Attribution Triage: WHOIS Domain Registry Records
When a corporate entity or developer purchases a domain name, the authoritative registrar requires accurate contact and administration data from the registrant. This metadata is collected and publicly stored within centralized global lookup ledgers via the **WHOIS** directory service.

### A. Forensic Footprint Enumeration
Using the terminal utility **`whois`**, a cyber threat intelligence (CTI) analyst parses public records to uncover key domain lifecycle variables and locate administrative footprints:
*   **Creation & Expiration Timestamps:** Tracks the exact historical deployment date and current ownership timelines of an asset.
*   **Registrar Abuse Metrics:** Exposes the managing registrar (e.g., GoDaddy.com, LLC) and details explicit abuse reporting channels, including phone lines and email registers, to expedite the taking down of malicious domains.
*   **Registrant Contact Trails:** Logs phone numbers, physical corporate mailing addresses, and administrative names linked to the asset owner.

### B. Defensive Obfuscation: Privacy Proxy Implementations
Sophisticated adversaries and privacy-conscious organizations deploy specialized proxy services to decouple their public metadata fields from WHOIS tracking lines:
```text
Domain Name: [REDACTED].COM
Updated Date: 2017-07-05T16:02:43Z
Creation Date: 1993-04-02T00:00:00Z
Registrar Registration Expiration Date: 2026-10-20T14:56:17Z
Registrar: GoDaddy.com, LLC
Registrant Name: Registration Private
Registrant Organization: Domains By Proxy, LLC
Registrant Street: DomainsByProxy.com
```
*   *Threat Hunting Caveat:* When a malicious domain hides behind an automated proxy shield (e.g., *Domains By Proxy, LLC*), the public registrant metadata fields are rewritten to display generic third-party proxy values. During an active security incident, an analyst must bypass this initial mask by reviewing historic WHOIS database records or filing explicit legal sub-poenas with the registrar to uncover the underlying registrant details.
## 4. Web Application Ingress: HTTP & HTTPS Communication Mechanics
When a web browser connects to a remote server, it communicates utilizing the **Hypertext Transfer Protocol (HTTP)** or its cryptographically hardened variant, **Hypertext Transfer Protocol Secure (HTTPS)**. This structural framework operates over the stateful **TCP transport layer (Layer 4)**, facilitating request-response transactions between clients and web daemons.

### A. Core Operational Methods (Verbs)
Web browsers issue standardized structural directives to alter or request resources on a remote backend web server:
*   **`GET`:** Instructs the remote server to retrieve and return a specific data asset, such as an HTML file, script, or image resource.
*   **`POST`:** Transmits user-supplied data payloads (e.g., submitting authorization forms or uploading binaries) to the server for processing inside a backend application context.
*   **`PUT`:** Uploads a targeted resource to instantiate a new file or completely overwrite existing data at a defined path destination.
*   **`DELETE`:** Commands the remote web daemon to wipe a specified file, resource, or directory path off the server storage partition.

### B. Port Architecture & Wire Forensics Dissection
*   **Socket Standards:** Unencrypted HTTP traffic relies natively on **TCP Port 80** (and alternative testing slots like `8080`), whereas TLS-encrypted HTTPS traffic leverages **TCP Port 443** (and testing slots like `8443`).
*   **Wireshark Stream Analysis:** While standard browsers render processed graphical views to the end-user, a network forensic analyst inspects raw packet captures (PCAPs) to parse unrendered layer-7 metadata. 
    *   *Forensic Visibility:* Dissecting an HTTP transaction stream unmasks critical environmental footprints. This includes client-side request details (such as the browser's raw User-Agent string) and server-side responses (such as explicit **Server Software Versions** like Apache or Nginx, content formatting tags, and **`Last-Modified` timestamps**). Investigators use these indicators to map host software and build attack timelines.

```text
  [ CLIENT WORKSTATION ] ═══════ GET /index.html HTTP/1.1 ══════> [ APACHE WEB SERVER ]
                         <═══════ HTTP/1.1 200 OK (Server Version) 
```

---

## 5. Storage Access Protocols: File Transfer Protocol (FTP) Architecture
The **File Transfer Protocol (FTP)** is an application-layer framework optimized explicitly for high-velocity data moving and remote file system administration. Unlike web protocols, it splits communication tasks across two separate connections.

### A. Dual-Channel Separation Mechanic
FTP operates using an isolated dual-channel connection layout to manage data transfers cleanly:
*   **The Control Channel (TCP Port 21):** The primary command-and-control connection. The client connects to TCP port 21 on the server to handle session authentication, pass system commands, and receive server response codes.
*   **The Data Channel (Dynamic TCP Ports):** Whenever a file transfer or directory listing is requested, the client and server negotiate a completely separate network link over an alternative temporary TCP port to stream the file data, closing the data channel as soon as the transaction ends.

### B. Protocol Translation & Interactive Commands
When a local client uses an interactive terminal client (`ftp`), the plain-text commands input by the operator are translated natively into raw FTP protocol headers before traveling across the control channel wire:

| User Input Command | Raw Control Channel Header | Technical Action Executed by the Server |
| :--- | :--- | :--- |
| `anonymous` | **`USER anonymous`** | Submits a public unauthenticated login string to log in without local credentials. |
| `[Blank Input]` | **`PASS`** | Forwards null credential verification parameters to bypass the password vault. |
| `ls` | **`LIST`** | Commands the server to spin up a temporary data connection channel to return file lists. |
| `type ascii` | **`TYPE A`** | Shifts the data connection translation engine into 7-bit ASCII text rendering mode. |
| `get coffee.txt` | **`RETR coffee.txt`** | Instructs the server to retrieve and stream the targeted file binary across the data link. |
| `put flag.txt` | **`STOR flag.txt`** | Commands the server to store an uploaded client file directly onto its local disk partition. |

### C. SOC Incident Response & Security Risk Advisory
*   **Plaintext Exposure Vulnerability:** Standard FTP control channels pass authentication parameters (`USER` and `PASS`) across the wire in unencrypted, clear text.
*   *Threat Hunting Impact:* Any malicious entity positioned on the same network segment can sniff the control channel packets to extract administrative domain passwords. Security operations engineers treat legacy FTP deployments as a critical compliance liability, systematically deprecating them across enterprise perimeters in favor of encrypted transport alternatives like **SFTP (SSH File Transfer Protocol)** or **FTPS (FTP over SSL/TLS)**.
## 6. Email Routing Infrastructure: Simple Mail Transfer Protocol (SMTP)
The **Simple Mail Transfer Protocol (SMTP)** is a foundational application-layer protocol designed explicitly to govern how mail clients transmit data payloads to mail servers, and how mail servers relay those message packages to each other across networks [INDEX].

### A. Protocol Mechanics & In-line Handshake Primitives
By default structural convention, standard unencrypted SMTP communications listen on **TCP Port 25** [INDEX]. When a client initializes a connection, it negotiates the mail transfer envelope using explicit text-based directives [INDEX]:
*   **`HELO` / `EHLO`:** Initiates the interactive SMTP session, announcing the client host identity string to the receiving server [INDEX].
*   **`MAIL FROM:`** Outlines the explicit sender envelope tracking parameter (e.g., `<user@client.thm>`) [INDEX].
*   **`RCPT TO:`** Defines the targeted recipient email address routing destination (e.g., `<strategos@server.thm>`) [INDEX].
*   **`DATA`:** Notifies the server that all subsequent text lines represent the actual body content and structural headers of the message payload [INDEX].
*   **`.` (Period on a standalone line):** Serves as an absolute termination token, instructing the mail daemon that the message input has closed and should be queued for delivery [INDEX].
*   **`QUIT`:** Gracefully drops the active TCP session thread [INDEX].

### B. Interactive SMTP Mail Delivery Triage via Terminal
Analysts can simulate or test corporate mail routing loops by opening a raw telnet pipe to TCP port 25 to trace the transaction log directly [INDEX]:
```text
user@TryHackMe$ telnet 10.49.178.18 25
220 example.thm ESMTP Exim 4.95 Ubuntu
HELO client.thm
250 example.thm Hello client.thm
MAIL FROM: <user@client.thm>
250 OK
RCPT TO: <strategos@server.thm>
250 Accepted
DATA
354 Enter message, ending with "." on a line by itself
From: user@client.thm
To: strategos@server.thm
Subject: Telnet email

Hello. I am using telnet to send you an email!
.
250 OK
QUIT
```

---

## 7. Mail Retrieval Paradigms: POP3 vs. IMAP Architecture
While SMTP manages the outward transmission and delivery pipeline, separate retrieval protocols are required to handle how client endpoints securely pull stored messages down from the remote mail server vaults [INDEX].

### A. Post Office Protocol Version 3 (POP3 - Download & Purge Operations)
POP3 functions as a simple retrieval engine designed to down-load email messages from a centralized server to a single local mail client application [INDEX].
*   **The Tracking Mechanic:** Listens on **TCP Port 110** by default [INDEX]. POP3 behaves similarly to a physical local mailbox; by historical design, it minimizes server storage requirements by downloading messages to the local endpoint disk partition and immediately deleting the copies off the remote server [INDEX]. This model creates severe data isolation gaps if a user attempts to track their mail across multiple separate devices [INDEX].
*   **Core Control Directives:**
    *   `USER` / `PASS` — Submits the unencrypted identity credentials required to authenticate into the mailbox vault [INDEX].
    *   `STAT` — Requests a quick state summary detailing the total message count and aggregated byte weight of the vault [INDEX].
    *   `LIST` — Enumerates all available message indices alongside their respective physical capacities [INDEX].
    *   `RETR <Index>` — Pulls the entire raw content header and message body stream matching the target index integer down to the terminal view [INDEX].

```text
  [ POP3 STREAM - PORT 110 ] ➔ USER strategos ➔ PASS ➔ STAT ➔ RETR 3 ➔ Deletes from Server
```

### B. Internet Message Access Protocol (IMAP - Multi-Device Synchronization)
IMAP serves as a modern retrieval framework engineered explicitly to sustain persistent, stateful mailbox synchronization across multiple diverse client endpoints simultaneously (e.g., matching a mobile device, laptop, and web client) [INDEX].
*   **The Tracking Mechanic:** Listens natively on **TCP Port 143** [INDEX]. Unlike POP3, IMAP retains all primary master copies of the email messages securely inside the remote server storage space, tracking stateful changes (e.g., marking a message as read, moving directories, or archiving threads) across all connected clients in real time [INDEX].
*   **Core Control Directives:**
    *   `LOGIN <user> <pass>` — Authenticates the user profile into the active state session [INDEX].
    *   `SELECT <folder>` — Connects the terminal scope to a specific target mailbox folder (e.g., `SELECT inbox`) [INDEX].
    *   `FETCH <Index> body[]` — Extracts the precise content blocks, text lines, and routing structures of a specific message while keeping the file stored on the server [INDEX].

```text
  [ IMAP STREAM - PORT 143 ] ➔ LOGIN strategos ➔ SELECT inbox ➔ FETCH 3 body[] ➔ Syncs Across Fleet
```

### C. SOC Incident Response & Cleartext Wire Exposure Warning
*   **The Cryptographic Deficit:** Legacy baseline deployments of SMTP (Port 25), POP3 (Port 110), and IMAP (Port 143) transmit all protocol headers, user names, administrative passwords, and data payloads across the network wire in **clear, unencrypted plain text** [INDEX].
*   *Forensic Capture Analysis:* A network packet analysis run via Wireshark exposes these parameters to any malicious entity executing a local network sniffing capture [INDEX]. Analysts look for these unencrypted sessions to catch credential leaks or isolate unhardened legacy software deployments [INDEX]. Modern enterprise architectures systematically enforce encrypted transport security layers—including **SMTPS (Port 465)**, **POP3S (Port 995)**, and **IMAPS (Port 993)**—to wrap these transactions inside secure TLS tunnels and block unauthorized traffic interception [INDEX].
## 8. Consolidated Enterprise Protocol Port Matrix
To support rapid traffic filtering, alert triage, and network forensic inspection, the default layer-4 transport protocols and port mappings covered across this module are consolidated into the tracking matrix below:

| Application Layer Protocol | Transport Protocol (L4) | Default Port Number | Primary Operational Target |
| :--- | :--- | :--- | :--- |
| **TELNET** | TCP | **23** | Legacy cleartext remote terminal command execution [INDEX]. |
| **DNS** | UDP / TCP | **53** | Domain name to logical IP address resolution mapping [INDEX]. |
| **HTTP** | TCP | **80** | Unencrypted web page retrieval and data delivery [INDEX]. |
| **HTTPS** | TCP | **443** | TLS-encrypted secure web browser traffic tunnels [INDEX]. |
| **FTP** | TCP | **21** | Command and session control channel for file transfers [INDEX]. |
| **SMTP** | TCP | **25** | Plaintext corporate email routing and server relay loops [INDEX]. |
| **POP3** | TCP | **110** | Cleartext email message retrieval (Download-and-Purge model) [INDEX]. |
| **IMAP** | TCP | **143** | Cleartext multi-device synchronized mailbox retrieval [INDEX]. |

*   *SOC Triage Note:* Identifying traffic bound to unencrypted text protocols—such as TELNET (23), HTTP (80), FTP (21), SMTP (25), POP3 (110), or IMAP (143)—is an immediate high-value indicator during network monitoring [INDEX]. Except where explicitly isolated within specialized legacy development networks, modern secure enterprise perimeters mandate forcing encrypted wrapper alternatives (SSH, HTTPS, SFTP, SMTPS, POP3S, IMAPS) to enforce confidentiality and block packet-sniffing exploits across the network wire [INDEX].
