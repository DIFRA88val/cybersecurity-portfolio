# 🪟 Windows Command Line (CMD) Lab Reference

**Room Source:** TryHackMe — Windows Command Line Basics
**Objective:** Navigate files, hunt hidden system indicators, read file parameters, and gather basic system details using text-based command interfaces.

---

## 🎯 Core Navigation & File Discovery

### 1. `cd` (Change Directory)
Moves the shell session to a specified folder location.
* **Syntax:** `cd [path]`
* **Example:** `cd C:\Users\Administrator\Desktop`

### 2. `dir` (Directory Listing)
Lists the visible files, directories, and timestamps inside the current working path.
* **Syntax:** `dir`

### 3. `dir /a` (List All / Hidden Attributes)
Forces the file system to display **all** directory content, explicitly overriding system or hidden file attributes often used by threat actors to conceal unauthorized malware tools.
* **Syntax:** `dir /a`

### 4. `dir /s` (Recursive Search)
Searches recursively through the current directory and all available nested subfolders to locate assets.
* **Syntax:** `dir /s`
* **Example:** `dir task_brief.txt /s`

---

## 📊 Host Telemetry & Network Diagnostics

### 1. `whoami` (Identity Verification)
Displays the exact user context and domain privileges of the account currently executing commands within the terminal session.
* **Syntax:** `whoami`

### 2. `systeminfo` (Host Profile & Auditing)
Extracts a comprehensive overview of the local machine architecture, including the precise OS version, network card details, and a complete list of installed **Security Hotfixes/Patches**.
* **Syntax:** `systeminfo`

### 3. `ipconfig` (Network Configuration)
Gathers local network interface parameters, exposing the device's assigned IPv4/IPv6 addresses, subnet masks, and default gateways.
* **Syntax:** `ipconfig`

---

## 🚩 Flag Captures & Retrospective

* **Target File:** `task_brief.txt`
* **Discovery Method:** Used `dir /s task_brief.txt` to find the hidden path, followed by `type task_brief.txt` to print the string layout directly inside the Command Prompt window.
* **Captured Flag:** `TASK-BRIEF-FOUND`
