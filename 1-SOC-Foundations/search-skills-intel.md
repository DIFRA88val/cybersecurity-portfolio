# Lab Report: Search Skills & Open-Source Intelligence (OSINT)

## 1. Objective & Core Concepts
The objective of this module is to establish precise strategies for filtering information overload during threat investigations, asset discovery, and incident response. Rather than relying on standard search engines, this lab covers advanced syntax manipulation, specialized intelligence repositories, and native command-line reference documentation.

## 2. Advanced Search Operators & Shodan Filters
To narrow down specific internet-connected infrastructure and exposed asset surfaces, specialized OSINT engines leverage focused filter structures:

*   **country:<code\>** — Restricts diagnostic lookup boundaries to a specific geographic country code (e.g., `country:IE`).
*   **port:<number\>** — Isolates assets exposing specific network port channels or active service ranges (e.g., `port:22`).
*   **org:<identifier\>** — Scopes external infrastructure query footprints to a specific corporate network or Autonomous System Number (ASN) identifier.
*   **hostname:<domain\>** — Forces explicit string matching against specialized host configurations or target domain names.

## 3. Threat Intelligence, Vulnerability Engines & Research Triage

| Investigative Engine | Core Function & Incident Response Application |
| :--- | :--- |
| **Shodan** | Continuously scans and maps public network interfaces, discovering exposed IoT equipment, industrial control controls, and active system banners. |
| **VirusTotal** | Aggregates detection streams from over 70 distinct antivirus engines and web filters to establish an analytical consensus on untrusted files, URL patterns, domains, or cryptographic hashes. |
| **CVE Program** | The industry-standard reference system assigning standardized keys (`CVE-YEAR-NUMBER`) to track, document, and cross-reference authenticated code vulnerabilities globally. |
| **Exploit-DB / GitHub** | Archives execution blueprints, Proof of Concept (PoC) scripts, and scanner tools used to reproduce or patch vulnerabilities. *Requires strict triage to verify malicious intent before executing components.* |

## 4. Local Technical Reference: Linux Man Pages
When navigating unknown CLI environments or analyzing endpoint utility configurations, the internal manual infrastructure provides built-in documentation directly from the console interface.

*   **Execution Command:** `man <command>`
*   **Example Case (`man nc`):** Renders the functional syntax parameters, supported protocol flags, and operational parameters for the arbitrary network connection utility Netcat (`nc`).

## 5. CVSS Risk Prioritization Framework
When evaluating emerging threats, Security Operations Centers (SOCs) leverage the Common Vulnerability Scoring System (CVSS) matrix to classify urgency and establish immediate patch management roadmaps based on:
*   **Impact:** The total scope of compromise or operational damage the flaw can generate.
*   **Complexity:** The difficulty or technical barriers an attacker faces to successfully trigger the exploit code.
*   **Availability:** The real-world likelihood and readiness of functional exploit material.
