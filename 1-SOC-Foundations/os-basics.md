# 🖥️ TryHackMe Pre-Security: OS Basics

## Course Progress: Module 3 — Windows Workstation Baseline (Completed)

Operational logs documenting Windows system architecture triage, core graphical interfaces, configuration settings, and defensive system tools using Windows Server 2019.

---

## 🪟 Windows System Architecture & Triage Controls

### 1. Account Authentication Levels
*   **Administrator:** Highly privileged system account with unrestricted baseline rights to perform system configurations, script installations, and user permission tracking.
*   **Standard User:** Accounts provisioned for everyday production tasks, isolated from making unauthorized system-wide alterations.
*   **Guest Account:** Deeply restricted temporary context profile with minimal localized access parameters.

### 2. Core Administrative Controls
*   **Task Manager:** Primary monitoring tool used to evaluate active runtime system infrastructure processes, tracking CPU/RAM allocations to pinpoint anomalous behavior.
*   **Windows Security & Firewall:** Central defensive dashboard managing automated file scanners, real-time file system integrity monitoring, and port filtering controls to restrict unauthorized network flows.
*   **Control Panel & Settings:** Management consoles used to query device attributes, modify patch deployment, and audit local security policy configuration variables.

---

## 🔍 Module 3: Hands-On Workstation Audit

### 🖥️ TryHatMe Station Environment Profile
*   **Workstation OS:** Windows Server 2019 Standard
*   **Device Identification Name:** THM-WINSERVER
*   **Key Path Discovered:** `C:\Users\Administrator\Desktop\TryHatMe Onboarding`
*   **Primary Active Process Monitored:** Scan Viruses
*   **Antivirus / Personal Firewall Status:** Working

### 📁 Lab Takeaways
Successfully established an initial security baseline on a target enterprise machine. Managed file directory verification, audited active running system services via Task Manager, and inspected Windows Defender indicators to verify comprehensive endpoint isolation and defensive posture.
