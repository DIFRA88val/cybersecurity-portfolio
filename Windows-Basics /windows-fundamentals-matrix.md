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
When a new user successfully authenticates for the first time, the local User Profile Service instantiates an isolated workspace root folder path under `C:\Users\<Username>`. This automated process maps out uniform data storage environments, including:
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

### The Security Token Mechanism:
*   **Split-Token Architecture:** When an account assigned to the local *Administrators* group logs into the endpoint environment, the operating system assigns a dual-token matrix to the active user session: a filtered **Standard User Access Token** and a high-privilege **Full Administrator Access Token**.
*   **The Default Context:** By default, all applications, processes, and terminal instances execute within the security context of the filtered standard token, stripping the session of administrative power during routine tasks.
*   **The Elevation Prompt:** When an application attempts a restricted system configuration change or software setup, the OS halts execution. It prompts a UAC context switch overlay, requiring explicit interactive approval to map the full administrative token onto the process thread.
*   *Built-In Bypass Exception:* By default architectural design, UAC enforcement constraints do not apply to the native, default built-in local `Administrator` profile account.

### Standard User Enforcement Triage:
When a standard user hits a UAC boundary (visually flagged on the application icon by a dual-color shield overlay), the system prompts for explicit high-privilege credentials. If valid administrative login parameters are not provided, the operating system drops the execution request, neutralizing stealthy malware scripts trying to exploit the active user's session context.
## 7. Administrative Configuration Triage Hubs
Windows segments workspace alterations and system policies into two centralized interfaces, balancing user accessibility with deep corporate infrastructure hardening tools.

*   **The Settings Interface:** Acts as the primary hub for consumer-level configuration adjustments, managing basic network interfaces, environment layouts, and default peripheral updates.
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
*   *SOC Triage Application:* Analysts right-click the Windows Taskbar or execute structural shortcuts (`Ctrl + Shift + Esc`) to trigger Task Manager during a suspected incident to quickly flag suspicious hidden binary filenames or isolate rogue active threads.

## 9. Diagnostic Triage & System Configuration Utility (MSConfig)
The native **System Configuration utility (`msconfig.exe`)** serves as a core diagnostic framework designed to identify and isolate system startup issues, anomalies, and active service footprints. Accessing this utility requires high-privilege local administrative credentials.

### Structural Oversight Tabs:
*   **General:** Manages initialization scopes during system boot transitions via three isolated states: *Normal Startup* (loads all default drivers/services), *Diagnostic Startup* (limits loading strictly to basic operational devices), and *Selective Startup* (empowers administrators to selectively toggle individual startup parameters).
*   **Boot:** Customizes the operating system initiation sequence, safe mode parameters, and advanced hardware processing boundaries.
*   **Services:** Enumerates all low-level system application daemons running in the background, fields tracked regardless of whether their status is actively `Running` or `Stopped`.
*   **Startup:** Directs the user context to the appropriate monitoring engine based on host classification. *Server Hierarchy Note:* In consumer clients, startup management transitions into Task Manager. In enterprise Windows Server setups, user-level startup configurations are managed natively inside the filesystem. Analysts audit auto-launching shortcuts or hidden binary components by running the folder shortcut path layout:
    ```text
    Run Box (Win + R) ➔ Type: shell:startup
    ```
*   **Tools:** An administrative command table aggregating quick-access links to deeper OS orchestration applications (e.g., Command Prompt, Registry Editor, Performance Monitor).

## 10. Memory Virtualization & Advanced System Settings
System engineering configurations can be customized to adjust operational stability, background telemetry, and memory metrics by searching *View advanced system settings* to open the System Properties dashboard.

### Virtual Memory Page File Management:
Windows establishes virtual memory extensions using a dedicated structural storage element called a **Page File (`pagefile.sys`)**. When physical RAM fills up, the OS maps data blocks onto this virtual space to prevent performance bottlenecks or application crashes.

## 11. Post-Crash Triage & The Startup and Recovery Matrix
When Windows encounters unrecoverable structural failures or a critical kernel error (e.g., the Blue Screen of Death / BSOD), the operating system triggers a protective save loop to gather live data.

### Crash Dump Infrastructure:
The system captures the exact current memory registers and executing kernel threads to construct a dedicated debug log file called a **Crash Dump File**.
*   *Write Debugging Information Profiles:* Configures how much data is retained following a crash sequence: *Automatic memory dump*, *Kernel memory dump* (excludes user-mode space), *Small memory dump (256 KB)* (extracts stop error codes and loaded driver lists), and *Complete memory dump* (captures the entire dynamic physical RAM footprint for deep forensics).

## 12. User Account Control (UAC) Notification Calibration
The sensitivity baseline of the UAC prompt architecture is managed via a four-tier slider matrix controlling how Windows alerts you to system-level alterations:
*   **Always Notify:** The highest security level. Prompts UAC validation whenever software changes system properties or when an administrator alters options. The display shifts to the isolated **Secure Desktop mode** (dimming the display) to block background script automation or tap-jacking injection vectors.
*   **Notify for Apps Only:** The default baseline context. Prompts exclusively for third-party binary executions while automatically bypassing prompts for native administrative adjustments. Secure Desktop remains active.
*   **Notify Without Dimming:** Mirrors default app notification profiles, but refrains from shifting the workspace window context into Secure Desktop mode, leaving it vulnerable to programmatic click-jacking attempts.
*   **Never Notify:** Suppresses all UAC security alerts entirely. Binaries execute and alter high-privilege parameters silently.

## 13. System Inspection & Consolidation (Computer Management - `compmgmt.msc`)
The **Computer Management utility (`compmgmt.msc`)** aggregates low-level configuration snap-ins into a single management console, structured across three core infrastructure nodes: **System Tools**, **Storage**, and **Services and Applications**.

### A. System Tools Engine
*   **Task Scheduler (`taskschd.msc`):** Orchestrates automated background script or application execution based on defined conditions called **Triggers**.
    *   *SOC Triage Application:* Attackers frequently abuse Task Scheduler for persistence. Analysts inspect the *Task Scheduler Library* to unmask suspicious recurring tasks or non-recurring triggers tied to user session transitions (e.g., launching code immediately at user logoff).
*   **Event Viewer (`eventvwr.msc`):** Captures automated logging data across the host to maintain a forensic audit trail. Core logging targets are categorized under **Windows Logs**: *Application* (software errors), *Security* (authentication audits, success/failure logons), and *System* (kernel/driver logs).
*   **Shared Folders:** Provides complete network resource visibility. Tracks active network folders under *Shares* (including administrative shares like `C$` and `ADMIN$`), ongoing connections under *Sessions*, and actively used remote files under *Open Files*.
*   **Performance Monitor (`perfmon.msc`):** Records real-time performance diagnostics and spikes.
*   **Device Manager (`devmgmt.msc`):** The hardware configuration management layer used to audit or explicitly disable attached hardware items and drivers.

### B. Storage Infrastructure & Services
*   **Disk Management (`diskmgmt.msc`):** Provides low-level partition control to format new disks, shrink active volumes, or change volume letter maps (e.g., assigning a drive to `E:`).
*   **Services Manager (`services.msc`):** Audits headless background daemons. Opening a service's **Properties** reveals its true internal service name, execution path binary string, and configured **Startup Type** (*Automatic* at boot, *Manual* via invocation, or *Disabled* to restrict runtime authorization completely).
*   **WMI Control:** Manages Windows Management Instrumentation (WMI) infrastructure profiles, which allow local or remote administration scripting via PowerShell or VBScript.

## 14. Total Asset Enumeration (System Information - `msinfo32.exe`)
The **System Information utility (`msinfo32.exe`)** queries system properties to output an exhaustive inventory across three key sectors: **Hardware Resources**, **Components** (verifying hardware device drivers like *Display* and *Input* controllers), and **Software Environment** (tracking active environment variables, active network connections, and third-party software packages).
## 15. Operational Resource Auditing & Telemetry (Resource Monitor - `resmon.exe`)
The native **Resource Monitor (`resmon.exe`)** serves as an advanced infrastructure diagnosis engine. It provides high-granularity, per-process telemetry tracking that extends beyond the default summary metrics visible in Task Manager.
*   *SOC Analytics Application:* Investigators leverage the four core tabs (**CPU**, **Memory**, **Disk**, and **Network**) to filter active file handles, find network connections, analyze active TCP port listeners, and identify process deadlocks.

## 16. Local CLI Triage & Network Diagnostic Commands (`cmd.exe`)
Security analysts utilize the native **Command Prompt (`cmd.exe`)** to collect system metrics and audit host network parameters quickly during an incident response audit:
*   `hostname` — Outputs the distinct computer name configured on the system.
*   `whoami` — Displays the active user account context and domain tracking strings.
*   `ipconfig` — Enumerates network adapter configurations, local IP addresses, and active default gateways.
*   `netstat` — Displays current TCP/IP network connection parameters. Appending specific flags (e.g., `netstat -a` or `netstat -b`) allows investigators to trace all active inbound/outbound sockets and reveal the exact executable application name initiating the connection.
*   **The `net` Utility Suite:** Managed explicitly via the syntax layout `net help <sub-command>`. Key investigative actions include: `net user` (identifies local accounts), `net localgroup` (audits administrative group memberships), `net share` (maps open file shares), and `net session` (tracks remote network connections targeting the host).

## 17. Host Configuration Persistence & The Windows Registry Architecture
The **Windows Registry** is a centralized, hierarchical database utilized by the operating system to store low-level configuration settings, hardware parameters, application configurations, and user profile properties.

### SOC Triage & Threat Hunting Perspective:
Because the operating system continually references the registry during active operations, it is a high-value target for threat actors. Analysts audit specific registry paths using the **Registry Editor (`regedit.exe`)** console utility to discover malicious modifications:
1.  **Persistence Mechanisms (Run Keys):** Attackers frequently abuse specific keys (e.g., `HKLM\Software\Microsoft\Windows\CurrentVersion\Run`) to force malicious scripts or remote access tools to execute automatically whenever the host boots up or a user authenticates.
2.  **Malware Evasion Tracking:** Adversaries modify registry hives to explicitly disable host defensive tools, turn off security monitoring daemons, or alter local firewalls.
3.  **Artifact Discovery (Forensics):** The registry retains deep forensic footprints, tracking recently executed binaries, user activity history, and external USB drive connections.

## 18. Enterprise Patch Management & The Windows Update Pipeline
The **Windows Update service** serves as the primary system lifecycling framework deployed by Microsoft to deliver critical security updates, operating stability hotfixes, feature enhancements, and updated malware signatures for native defensive applications like Microsoft Defender.

### Structural Patch Cycles & Operational Realities:
*   **Patch Tuesday:** Microsoft establishes a predictable, recurring monthly deployment cadence on the **second Tuesday of each calendar month** to release consolidated security patch arrays.
*   **Out-of-Band (OOB) Patches:** Urgent, high-severity zero-day vulnerability mitigations bypass the standard Patch Tuesday cycle; Microsoft pushes these critical updates immediately via the automated update stream to blunt live exploitation vectors.
*   **System Enforcement Constraints:** To resolve historical vulnerabilities caused by end-users indefinitely delaying system patches, modern enterprise setups enforce strict update boundaries. Patches can be temporarily postponed, but the operating system eventually enforces an automated restart to lock the code updates into the system kernel.
*   *Analytical CLI Triage Path:* Analysts trigger or inspect the update control plane via terminal execution tools: `control /name Microsoft.WindowsUpdate` via the Run Dialog box.

## 19. Host Defense Orchestration (Windows Security Suite)
The **Windows Security dashboard** functions as the centralized endpoint security management console, consolidating five core infrastructure protection nodes into a unified management plane.

### Visual Telemetry Status Array:
*   **Green Icon:** The local environment meets baseline protection requirements; no administrative tasks or policy adjustments are recommended.
*   **Yellow Icon:** Identifies non-critical safety recommendations, policy discrepancies, or pending configuration audits that require administrative review.
*   **Red Icon:** Flags an active warning or severe security deficiency (such as disabled defensive layers) requiring immediate endpoint triage.

## 20. Endpoint Scanning & Threat History Infrastructure
The **Virus & threat protection** engine handles real-time threat hunting and manual on-demand file system scanning across the system storage volumes.

### A. Scanning Execution Profiles
*   **Quick Scan:** Targets high-risk system directories, volatile registry structures, and memory zones where malicious drop-files are commonly staged by threat actors.
*   **Full Scan:** Conducts an exhaustive, byte-level evaluation of every file on the system volume alongside all actively executing memory blocks and background processes.
*   **Custom Scan:** Restricts the scanning perimeter strictly to explicit file paths or folder intersections designated manually by the security analyst.

### B. Forensics & Threat Ingestion Layers
*   **Quarantined Threats:** Isolates verified malicious binaries into a high-security cryptographic vault, changing file properties to safely block the code from executing while preserving the file for security analysis.
*   **Allowed Threats:** Tracks items flagged by the signature scanner but explicitly authorized to run by the local administrator.

## 21. Antivirus Hardening Settings & Behavioral Controls
Advanced host defense configurations can be adjusted within the Microsoft Defender Antivirus engine to customize endpoint defenses:

### A. Real-Time & Cloud Protection Matrix
*   **Real-Time Protection:** Monitors thread behavior in real time to inspect binaries during invocation or initial delivery before they can execute in system memory.
*   **Cloud-Delivered Protection:** Links the endpoint to global cloud threat indices, enabling rapid signature updates and zero-day defense verification against emerging global threat campaigns.
*   **Automatic Sample Submission:** Forwards suspicious telemetry signatures or unclassified executables back to Microsoft's analysis labs to update global defensive definitions.

### B. Advanced Containment Controls
*   **Controlled Folder Access:** A vital anti-ransomware defensive layer. When enabled, it locks down designated storage arrays and memory structures, blocking unauthorized third-party applications from modifying or encrypting local files. Only explicitly whitelisted, verified system binaries are permitted to modify metadata within these protected folders.
*   **Exclusions Optimization:** Allows administrators to define explicit folder perimeters, file paths, or specific application extensions to be bypassed by the antivirus scanner to limit false-positive logs across enterprise software suites.

## 22. Network Boundary Controls (Windows Defender Firewall)
The native **Windows Defender Firewall** functions as a localized network access control gatekeeper, regulating the entry and exit of data packets across explicit communication channels called **Ports**. It mitigates unauthorized discovery probes and halts external threat propagation by matching traffic requests against three isolated network connection environments:
*   **Domain Profile:** Applies exclusively to networks where the endpoint successfully identifies and authenticates to a centralized Active Directory Domain Controller.
*   **Private Profile:** A user-assigned network boundary optimized for trusted perimeters, such as corporate home offices or internal private intranets, allowing broader local device discovery.
*   **Public Profile (Default State):** The restrictive default configuration applied to untrusted networks (e.g., airport Wi-Fi, public hotspots). It explicitly hardens the host by blocking inbound discovery requests and restricting open application permissions.

## 23. Application Containment & Memory Exploit Protection
Windows builds runtime isolation and integrity checks directly into the host system to minimize vulnerability exploitation risks:
*   **Microsoft Defender SmartScreen:** A proactive application and browser filter that monitors web downloads, untrusted files, and remote URLs. It cross-references assets against a global reputation matrix to block or issue an interactive Warn prompt before unrecognized binaries can execute.
*   **Core Isolation & Memory Integrity:** A hardware-virtualized defensive layer that leverages hypervisor controls to prevent unauthorized code injection attacks. It stops advanced malware from injecting malicious execution threads into high-security kernel processes.

## 24. Hardware-Rooted Cryptography (TPM & BitLocker Data Protection)
To address the threat of physical data theft, unauthorized exposure from lost hardware, or offline data manipulation, Windows combines software-level encryption with hardware-rooted keys:
*   **Trusted Platform Module (TPM):** A dedicated, tamper-resistant cryptographic co-processor chip embedded directly on the motherboard. It carries out secure cryptographic key generation, maintains system startup metrics, and locks down authentication keys so they cannot be extracted or modified by malicious software.
*   **BitLocker Drive Encryption:** A full-disk data protection layer that integrates seamlessly with the host operating system. When backed by a TPM chip (v1.2 or later), BitLocker verifies boot integrity before decrypting the storage volume, ensuring the underlying operating system files were not tampered with while the system was offline.

## 25. Host Backup Resiliency & Volume Shadow Copy Service (VSS)
The **Volume Shadow Copy Service (VSS)** coordinates the necessary snapshot actions to generate point-in-time backup copies of active system data volumes without interrupting running applications.

*   **Storage Workspace:** These automated data snapshots are retained within the hidden, protected **System Volume Information** folder root on each enabled volume partition.
*   **Recovery Capabilities:** If system protection is enabled, administrators can generate instant Restore Points, revert the OS environment via System Restore, or completely delete historic restore metrics to recover space.
*   *Critical SOC Ransomware Note:* Sophisticated ransomware strains are explicitly programmed to hunt down the VSS engine and clear all local shadow copies (`vssadmin.exe delete shadows /all /quiet`) before initializing encryption loops. This malicious action forces total collapse of the local host recovery plane, making offline or off-site immutable backup registries a mandatory requirement for corporate infrastructure resilience.
