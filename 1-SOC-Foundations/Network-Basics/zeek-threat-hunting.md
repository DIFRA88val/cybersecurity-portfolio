# Comprehensive Lab Report: Network Security Monitoring (NSM) & Live Incident Triage via Zeek

## 1. Tactical Implementation: The Network Behavior Analysis Engine
**Zeek** is an open-source, passive network security monitoring (NSM) platform that translates raw packet captures (PCAP) into structured, tab-delimited, high-granularity forensic log files. Unlike traditional signature-based Intrusion Detection Systems (IDS), Zeek operates as a behavior analysis engine, mapping network traffic states sequentially across specialized default log files:

### Primary Forensic Log Registries:
*   **`conn.log`:** Tracks basic network transaction session states, capturing localized connection durations, service protocols, port numbers, and byte volumes [INDEX].
*   **`dns.log`:** Indexes comprehensive domain name system activity, mapping query names, transaction IDs, and explicit record types [INDEX].
*   **`http.log`:** Records Layer 7 web application data, tracking raw request paths, HTTP verbs, user-agent strings, and host domains [INDEX].
*   **`signature.log`:** Stores real-time alert triggers matching custom protocol behavioral signatures or exploit conditions [INDEX].

---

## 2. Case Study 1: Anomalous DNS Activity (DNS Tunneling Extraction)
An alert triggered flagging **Anomalous DNS Activity** across an internal asset segment. To validate this alert as a true positive, an investigator parses `dns-tunneling.pcap` logs to uncover signs of DNS Tunneling—a technique used by adversaries to encapsulate malicious command-and-control (C2) traffic inside legitimate DNS queries to bypass network firewalls.

### A. Triage Analysis Steps & CLI Syntax Queries:
1.  **Isolate IPv6 Record Volumes:** To find the total number of DNS records linked explicitly to IPv6 mappings (**AAAA records**), the analyst filters the record format column inside the DNS logging facility:
    ```bash
    cat dns.log | zeek-cut qtype answers | grep "AAAA" | wc -l
    ```
2.  **Locate Outlier Connection Durations:** To calculate the maximum connection length and find long-lived, persistence beacons, the analyst reads session details out of the connection log:
    ```bash
    cat conn.log | zeek-cut duration | sort -n | tail -n 1
    ```
3.  **Calculate Unique Domain Counts:** Adversaries executing DNS tunneling flood network interfaces with millions of unique subdomains (e.g., passing base64 chunks like `://c2-server.com`). To count unique queries, the analyst isolates the domain name column:
    ```bash
    cat dns.log | zeek-cut query | sort | uniq | wc -l
    ```
4.  **Identify the Source Threat IP:** To isolate the internal compromised asset responsible for initializing the abnormal traffic spike, the analyst cross-checks source identifiers within the connection log:
    ```bash
    cat conn.log | zeek-cut id.orig_h | sort | uniq -c | sort -nr
    ```

---

## 3. Case Study 2: Phishing Triage & Post-Compromise Binary Delivery
An alert triggered flagging a **Phishing Attempt** targeting enterprise employees. The analyst triages the network trace file to follow the attack sequence: identifying the source email sender, tracing web page redirections, tracking malicious payload downloads, and pivoting to threat intelligence engines for executable validation.

### A. Core Forensic Ingress Operations:
*   **Source Address Defanging:** Threat hunters defang malicious source indicators (e.g., converting `attacker@malicious.com` into **`attacker[.]malicious[.]com`** or wrapping IPs like **`10[.]10[.]10[.]10`**) before compiling reports to prevent accidental clickable invocation [INDEX].
*   **Malicious Domain Extraction:** Investigating the web transaction ledger (`http.log`) unmasks the specific external domain name host serving the initial exploit document drop [INDEX].
*   **VirusTotal Threat Intelligence Pivots:**
    *   *Document Classification:* Submitting the malicious document's cryptographic hash into VirusTotal identifies the exact file format family (such as a macro-enabled spreadsheet or weaponized text document) used as the initial execution vector [INDEX].
    *   *Payload Name Mapping:* Inspecting the dropped executable reveals its original compiled file name designation inside VirusTotal registries [INDEX].
    *   *Contacted Infrastructure:* Threat hunters inspect behavioral logs to locate the external defanged domain string contacted by the `.exe` payload upon execution [INDEX].
*   **Request Path Isolation:** The explicit server-side request name used to pull down the malicious binary file is isolated by searching for `.exe` string completions inside the HTTP log:
    ```bash
    cat http.log | zeek-cut uri uri_stem | grep "\.exe"
    ```

---

## 4. Case Study 3: Log4J Exploitation Attempt (CVE-2021-44228)
An alert triggered flagging a **Log4J Exploitation Attempt**. This zero-day vulnerability exploits Java logging mechanisms, allowing attackers to force servers into executing remote code by submitting jndi lookup strings. The analyst processes `log4shell.pcapng` alongside Zeek detection scripts to trace the exploitation attempt.

### A. Technical Attack Signature Audit:
*   **Signature Hit Validation:** Running the packet trace using a specialized verification script (`detection-log4j.zeek`) generates hits inside the signature tracking database [INDEX]. The analyst audits the event frequency to measure the absolute scope of the attack surface probe:
    ```bash
    cat signature.log | zeek-cut sig_id | wc -l
    ```
*   **Scanner Tool Identification:** Inspecting User-Agent metadata inside the HTTP layer reveals the exact automated scanning engine or command-line utility used by the adversary to map the environment [INDEX].
*   **Exploit Extension Extraction:** Attackers leverage JNDI redirection strings to force the target server to pull down compiled Java code files. Checking the request path reveals the file suffix extension used to execute the malicious logic on the host [INDEX].
*   **Base64 Payload Command Decoding:** Attackers wrap command strings inside Base64 obfuscation masks to bypass simple security filters. The analyst extracts the string from the custom `log4j.log` file, passes it to the command-line decoding matrix (`echo "BASE64_STRING" | base64 -d`), and decodes the exact file name created on the compromised system file path [INDEX].
