# 🖥️ TryHackMe Pre-Security: OS Basics

## Course Progress: Module 3 Complete (100%)

Operational logs documenting Windows Server 2019 baseline configurations alongside Linux command-line architecture, file navigation, system metrics extraction, and configuration filtering.

---

## 🪟 Part 1: Windows Workstation Audit
*   **Administrative Controls:** Navigated Windows Server 2019 architecture, triaged processes via Task Manager, and verified endpoint isolation policies within Windows Defender Firewall.
*   **Directory Architecture:** Audited default deployment frameworks and absolute path directories (`C:\Users\Administrator\Desktop\TryHatMe Onboarding`).

---

## 🐧 Part 2: Linux CLI & Architecture Mastered

### 1. File Navigation & Directory Discovery
*   `pwd` / `cd`: Isolated absolute directory paths and executed horizontal path navigation.
*   `ls -al`: Listed hidden system files, mapped ownership permissions, and tracked file modification timestamps.
*   `cat`: Read raw configuration payloads directly from the standard input stream.

### 2. Advanced Search & Diagnostics
*   `find ~ -name <filename>`: Queried file structures natively from the user home directory space down through nested hierarchies.
*   `whoami`: Extracted current terminal active execution context privilege profile.
*   `uname -a`: Queried the underlying kernel platform architecture (`tryhackme` hostname, architecture types).
*   `df -h`: Analyzed disk infrastructure parameters (`/dev/root` utilization blocks).

### 3. Practical Configurations Triaged
*   `cat /etc/os-release`: Located and parsed baseline Linux operating sys
