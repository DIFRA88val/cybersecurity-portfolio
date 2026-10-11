# Comprehensive Lab Report: Windows Architecture & File System Internals (Parts 1-3)

## 1. Storage Infrastructure & New Technology File System (NTFS) Architecture
Modern enterprise Windows deployments utilize the **New Technology File System (NTFS)** as the primary storage layout framework, displacing historical architectures like FAT16/FAT32 and HPFS. 

### Core Architectural Advantages over FAT Frameworks:
*   **Journaling Integrity:** NTFS functions as a journaling file system, maintaining a dedicated metadata transaction log file (`$LogFile`) to automatically recover and repair disk errors or directory corruptions following a system crash or power disruption.
*   **Scale Capabilities:** Natively handles large single-file capacities exceeding the rigid **4GB** storage limitation inherent to FAT32 systems.
*   **Granular Security:** Enforces robust Access Control Lists (ACLs) directly on individual folders and discrete file entries.
*   **Optimization Triage:** Supports native storage optimization boundaries via built-in compression and transparent **Encrypting File System (EFS)** cryptopipeline arrays.
*   *Legacy Environments:* FAT file allocations remain standard outside of local OS installations, driving public formatting parameters on external USB peripherals and MicroSD devices.

## 2. NTFS Discretionary Access Control Lists (DACLs)
Security controls over file system assets are regulated by explicit permissions granted or denied across user accounts and structural group definitions.

### The Six Standard Ingress Permission Pillars:
1.  **Full Control:** Grants complete operational administrative authority, allowing users to alter data, run binaries, modify subfolder inheritance settings, and alter ownership permissions.
2.  **Modify:** Allows users to read, execute, overwrite, and delete files, but restricts the capacity to manipulate asset ownership or underlying security parameters.
3.  **Read & Execute:** Permits viewing application structural properties and executing executable software binaries or background script parameters.
4.  **List Folder Contents:** Allows traversing folders to view internal contents and filenames, regardless of individual file permissions.
5.  **Read:** Grants basic visibility parameters to view file text or open media structures.
6.  **Write:** Authorizes appending metadata arrays, instantiating new objects, and overwriting data assets.

### Administrative Permission Validation Triage:
An incident investigator evaluates an item's current DACL state using the native graphical user interface:
```text
[Right-Click Target Object] ➔ Properties ➔ Security Tab ➔ Select Target User/Group
```

## 3. Threat Intelligence: Alternate Data Streams (ADS)
NTFS introduces a hidden storage architecture component known as **Alternate Data Streams (ADS)**. Natively, every file object possesses a primary default stream containing visible application text or code, designated as **`$DATA`**. 

*   **The Mechanic:** ADS allows a single file entity to house multiple secondary data streams hidden away from standard user interfaces.
*   **Evasion Impact:** Natively, Windows File Explorer hides ADS footprints completely, omitting the hidden payload data from the standard file size metrics visible to standard users.
*   **SOC Analysis Triage:** Malware authors frequently abuse ADS vectors to stash malicious binaries, malicious scripts, or exfiltrated data tables inside harmless, benign files. 
*   **Legitimate Use Cases:** The operating system uses ADS identifiers to establish web asset sourcing. When downloading a document from the external web, Windows appends a custom Zone Identifier stream (`:Zone.Identifier`) to flag the file as an external asset, prompting local sandbox execution policies.
*   **Command Line Query Syntax (PowerShell Visibility):**
    ```powershell
    Get-Item -Path .\target_file.exe -Stream *
    ```

## 4. The System Boot Environment & Core Operating System Paths
*   **`%windir%` (The Windows Directory System Variable):** Dynamically references the absolute drive location hosting the active operating system environment (natively mapped to `C:\Windows`). Environmental system variables ensure applications locate infrastructure folders regardless of structural partition reassignments.
*   **`C:\Windows\System32` (The Operational Core):** The high-privilege engine room of the operating system. Holds critical DLL extensions, low-level kernel drivers, and essential operating configuration files. 
    *   *SOC Incident Note:* System32 houses native command binaries and administrative monitoring tools. Accidentally deleting or tampering with these system assets can cause severe runtime crashes, making this directory a frequent target for file masquerading and binary replacement schemes during privilege escalation attempts.

## 5. Local Identity Perimeters & User Space Account Triage
Windows endpoint operating environments categorize user accounts into two distinct authorization frameworks:

| Account Type Classification | Operating Scope & Boundary | SOC Investigation/Privilege Focus |
| :--- | :--- | :--- |
| **Administrator** | Holds unrestricted access rights across the local computer plane. | Can modify core security parameters, instantiate user accounts, drop or change software packages, and manipulate system registries. |
| **Standard User** | Restricted strictly to the localized account workspace path parameters. | Cannot execute system-level software setups, modify host infrastructure frameworks, or view high-privilege configuration trees. |

### User Profile Ingress Mapping
When a new user successfully authenticates for the first time, the local User Profile Service instantiates an isolated workspace root folder path under **`C:\Users\<Username>`**. This automated process maps out uniform data storage environments, including:
*   `C:\Users\<Username>\Desktop`
*   `C:\Users\<Username>\Documents`
*   `C:\Users\<Username>\Downloads`

### High-Efficiency Identity Triage via Run Diagnostics
To immediately evaluate the active user account configurations, hidden groups, and group membership mappings on an endpoint, security analysts run the local user management console utility using the native **Run Dialog box**:
```text
Windows Key + R ➔ Type: lusrmgr.msc
```
*   **`Users/` Node:** Details local accounts, operational status flags (Disabled/Active), and password constraints.
*   **`Groups/` Node:** Displays localized permission buckets. When an analyst assigns an identity to a group (e.g., the local *Backup Operators* group), that identity dynamically inherits the group's pre-configured access rights.
## 6. Access Token Separation & User Account Control (UAC) Mechanics
To mitigate the severe systemic risks associated with running daily tasks (such as web browsing or standard utility execution) under high-privilege administrative tokens, modern Windows systems implement **User Account Control (UAC)**. 
*   **The Elevation Prompt:** When an application attempts a restricted system configuration change or software setup, the OS halts execution. It prompts a UAC context switch overlay, requiring explicit interactive approval to map the full administrative token onto the process thread.
*   *Built-In Bypass Exception:* By default architectural design, UAC enforcement constraints do not apply to the native, default built-in local `Administrator` profile account.

### Standard User Enforcement Triage:
When a standard user hits a UAC boundary (visually flagged on the application icon by a dual-color shield overlay), the system prompts for explicit high-privilege credentials. If valid administrative login parameters are not provided, the operating system drops the execution request, neutralizing stealthy malware scripts trying to exploit the active user's session context.

## 7. Administrative Configuration Triage Hubs
Windows segments workspace alterations and system policies into two centralized interfaces, balancing user accessibility with deep corporate infrastructure hardening tools.

*   **The Settings Interface:** Act as the primary hub for consumer-level configuration adjustments, managing basic network interfaces, environment layouts, and default peripheral updates.
*   **The Control Panel:** The foundational system utility space designed for deep, complex administrative adjustments. It houses granular configuration applets required to manage hardware, deploy infrastructure policies, and audit application integrity.
    *   *The Interface Pipeline Effect:* Certain configurations initiated inside the newer Settings application (such as clicking `Change adapter options` under Network parameters) dynamically hand off the execution process to the underlying Control Panel binary layer.
*   **Software Verification Auditing:** In a SOC investigation or system hardening audit, an analyst navigates to the Control Panel:
    ```text
    Control Panel ➔ Programs ➔ Programs and Features
    ```
    This utility enumerates all installed system software binaries, verified publishers, precise software deployment dates, and active structural version keys to unmask unauthorized software packages.

## 8. Host Process Observation & Resource Triage (Task Manager)
The native **Task Manager** utility serves as the primary system oversight tool to obtain immediate visibility into active programs, background operations, and overall infrastructure resource health.

### Investigative Capabilities via Detailed Views:
*   **Processes Node:** Maps out active applications and headless background service daemons running in memory. It provides real-time tracking of critical compute metrics including CPU usage, RAM footprints, Disk I/O bandwidth, and Network throughput.
*   **Performance Monitoring Engine:** Generates interactive graphical telemetry displays illustrating baseline resource allocation states, network card traffic flow, and processor performance metrics.
*   *SOC Triage Application:* Analysts right-click the Windows Taskbar or execute structural shortcuts (`Ctrl + Shift + Esc`) to trigger Task Manager during a suspected incident. This view allows them to immediately detect abnormal CPU spikes, flag suspicious hidden binary filenames, or abruptly isolate rogue active processes by killing non-responsive threads.
