# 🐧 Linux Operating System Security Reference Guide

**Room Source:** TryHackMe — Operating System Security
**Objective:** Secure remote host authentication, navigate Linux directory flags, verify user privileges, and investigate poor credential handling paradigms across multiple local target profiles.

---

## 🎯 Core Remote Management Commands

### 1. `ssh` (Secure Shell)
Establishes an encrypted, remote command-line connection to a target system across a network.
* **Syntax:** `ssh username@target_ip`
* **Example:** `ssh sammie@10.48.161.238`

### 2. `whoami` (Identity Verification)
Prints the active, logged-in username tied to the current shell environment session.
* **Syntax:** `whoami`

### 3. `ls` (List Directory Contents)
Displays all non-hidden files and folder structures inside the current working directory block.
* **Syntax:** `ls`

### 4. `cat` (Concatenate / Read File)
Prints the entire plain-text contents of a file directly onto the terminal screen interface.
* **Syntax:** `cat [filename]`
* **Example:** `cat draft.md`

### 5. `history` (Command Audit)
Prints a sequential, chronological log of all past terminal commands executed by the current user context.
* **Syntax:** `history`

---

## 🔒 Lateral Movement & Privilege Switching

When auditing multi-user targets or attempting to verify weak passwords for known system users (such as `johnny` or `linda`), two standard secure entry pathways exist:

### 1. Direct SSH Remote Guessing
Used to establish a fresh remote terminal session directly into a targeted alternative user context from outside the machine host.
* **Syntax:** `ssh johnny@10.48.161.238`

### 2. Local `su` (Switch User)
Used when already logged into a shell session to switch instantly to another local account's configuration workspace by verifying their credentials locally.
* **Syntax:** `su - username`
* **Example:** `su - johnny`

---

## 🚩 Extracted Telemetry Log

* **Target Directory Contents:** `country.txt`, `draft.md`, `icon.png`, `password.txt`, `profile.jpg`
* **Security Insight (`draft.md` Analysis):** *"Reusing passwords means that your password for other sites becomes exposed if one service is hacked."*
## 🕵️ Incident Investigation & Privilege Escalation

### 1. Initial Credential Leak (`password.txt`)
An audit of unhardened file objects inside the `sammie` directory revealed a plaintext list of vulnerable organizational credentials:
* **Command:** `cat password.txt`
* **Discovered Credential:** `johnny` account password mapped to `dragon`

### 2. Privilege Pivoting to Johnny
Using local lateral movement techniques, the analyst pivoted to the user account profile to investigate their terminal behaviors:
```bash
# Switch session context to johnny using the leaked credential
su - johnny
# Input password: dragon
```

### 3. Exploiting Operational Footprints (`history` Audit)
Once logged in as `johnny`, the user's terminal execution history was audited to scan for accidental privilege leaks or miskeyed inputs:
```bash
# Display chronological terminal executions
history
```
* **Discovery:** The user mistakenly typed the server's master administrative credentials directly into the shell prompt instead of a standard system directive.
* **Root Password Compromised:** `letmeROOT`

### 4. Root Elevation & Final Asset Retrieval
Using the compromised administrative string, the analyst escalated to superuser access permissions to retrieve the system flag payload:
```bash
# Escalate execution privileges to the root master context
su - root
# Input password: letmeROOT

# Print the final system target asset
cat /root/flag.txt
```
* **System Flag Payload:** `THM-OS-SECURED`
