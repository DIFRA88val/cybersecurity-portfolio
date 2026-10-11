# Lab Report: Become a Defender (Blue Team Basics)

## 1. Objective & Scope
The objective of this laboratory exercise was to transition from an offensive testing mindset to a defensive architectural stance (**Blue Team Operations**). The goal was to map an enterprise infrastructure perimeter using a city abstraction, establish baseline structural visibility, and deploy targeted security controls to enforce the **CIA Triad** (Confidentiality, Integrity, Available).

## 2. Infrastructure Asset Mapping
To defend an environment, a security analyst must maintain comprehensive asset inventory visibility. The infrastructure parameters were evaluated and mapped to their defense functional tiers:

*   **Employee Endpoints (Workstations/Devices):** Front-line operational assets. Subject to social engineering, malware insertion, and credential harvesting.
*   **Enterprise Web Server:** Public-facing interface hosting applications. Primary target for exploitation, scanning (`gobuster`), and unauthenticated access attempts.
*   **Mail Server:** Inbound/outbound organization communication node. High-risk pathway for malicious links or attachment deployment.
*   **Network Firewall Perimeter:** The primary gateway regulating external traffic ingress/egress.

## 3. Defense Matrix & Risk Mitigation Strategies
Following structural mapping, security controls were dynamically implemented to mitigate realistic threat trajectories:

| Asset Classification | Potential Threat Vectors | Applied Defensive Control Plane |
| :--- | :--- | :--- |
| **Employee Devices** | Malicious script execution, missing patches | Antivirus Deployment & Automated Patch Configuration Management |
| **Web Server** | Directory traversal, unauthorized access | Strict inbound traffic constraints & Encrypted Communication (HTTPS) |
| **Mail Server** | Phishing, credential harvesting link delivery | Robust Spam Filtering Engine & Automated Attachment Ingestion Scanning |
| **Firewall Perimeter** | Unauthorized discovery probes, threat traversal | Hardened Traffic Access Control Lists (ACLs) & Known-Bad IP Blacklisting |

## 4. Fundamental Blue Team Operations Summary
By adhering to core structural principles—**Threat Anticipation, Attack Awareness, Risk Prioritization, and Continuous Adaptation**—this exercise successfully demonstrated how a layered, defense-in-depth model breaks active attack chains and stops localized compromises from pivoting into full network-wide security failures.

*   **Infrastructure Architecture Mapping Flag:** `THM{VISIBILITY_IS_KEY_FOR_DEFENSE}`
*   **Active Layered Defense Deployment Flag:** `THM{LAYERED_DEFENSE_REDUCES_RISK}`
