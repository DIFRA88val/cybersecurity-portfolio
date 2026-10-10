# Comprehensive Lab Report: Linux Core Fundamentals (Parts 1-3)

## 1. System Identity & Terminal Conversational Architecture
Interacting with a Linux enterprise asset begins at the Command Line Interface (CLI). In defensive operations, establishing current session privileges is a prerequisite for security auditing and log analysis.

*   `whoami` — Outputs the current active username configuration, establishing explicit file system access boundaries.
*   `echo <text>` — Generates explicit string output to the standard emission stream. String values containing multi-byte characters or spaces are encapsulated using structural quotes (e.g., `echo "hello world"`).

## 2. File System Navigation Matrix
Navigating hierarchical storage perimeters without a graphical interface is performed using four foundational operational utilities:

*   `pwd` (Print Working Directory) — Evaluates and outputs the absolute directory path string of the current operational terminal shell context.
*   `ls` (List) — Enumerates the directory items, files, and child endpoints contained in the active directory path.
*   `cd <directory>` (Change Directory) — Alters the operational shell perimeter, shifting the working scope into the designated directory path.
*   `cat <file>` (Concatenate) — Reads raw file contents and outputs the entire structural stream directly into the terminal window for real-time inspection.

## 3. High-Efficiency Fuzzing & Log Searching
To parse large infrastructure text fields (such as enterprise web access records) without manual scrolling, automated filtering utilities are deployed:

*   **File Level Tracking:** `find -name <filename>` — Systematically scans file system paths to locate specific files by their exact naming syntax (e.g., `find -name passwords.txt`).
*   **Data Content Extraction:** `grep "<string>" <file>` — Parses text files sequentially to isolate lines matching targeted keyword criteria (e.g., extracting anomaly footprints from an active `access.log`).

## 4. Multi-Command Operators & Data Redirection Control
Linux environments utilize conditional characters and flow redirectors to chain utility execution and manage system output logs:

| Operator | Syntax Logic | Incident Response/SOC Utility Case |
| :--- | :--- | :--- |
| **`&`** | Background Execution | Runs long-duration discovery tasks or scanning tools without locking the active interactive shell window. |
| **`&&`** | Sequential Dependency | Executes the secondary command only if the primary command exits successfully with a zero-error return code. |
| **`>`** | Stream Redirection (Overwrite) | Intercepts standard command output and saves it into a file, completely erasing prior content. |
| **`>>`** | Stream Redirection (Append) | Redirects tool outputs directly to the bottom of an asset file to create a continuous chronological ledger. |

## 5. Remote Access Security & The SSH Protocol
Secure Shell (SSH) is a cryptographic network protocol utilized for operating network services securely over an unsecure network. It provides encrypted command-line access to remote Linux assets, ensuring confidentiality and data integrity during transit.

*   **Mechanism:** Encrypts human-readable payload inputs at the host terminal before transmission across network mediums, decrypting the data only upon arrival at the authenticated destination gateway.
*   **Authentication & Ingress Connection Syntax:**
    ```bash
    ssh username@REMOTE_IP
    ```
    *   *Operational Note:* Standard administrative security protocols mask password character input on the interactive terminal interface; no visual feedback (characters or placeholders) is rendered during password entry strings.

## 6. Command Behavior Extension (Flags & Arguments)
Linux binaries operate under default configurations unless explicitly modified at runtime via arguments, switches, or flags prefixed by hyphens (`-` or `--`).

*   `ls` — Enumerates standard files within the working environment while omitting system configuration elements.
*   `ls -a` (or `--all`) — Extends the binary scope to list hidden files and configuration profiles. *In Linux file structures, any object prefixed with a dot (`.`) is classified as a hidden entity.*

## 7. Native System Documentation Triage
To self-document asset binaries and troubleshoot flag options directly inside the shell, Linux platforms embed native instructional matrices:

*   **Inline Contextual Help (`--help`):** Appending the `--help` switch to an execution string displays a compressed syntax schema, usage arguments, and tool descriptions.
*   **System Manual Pages (`man <command>`):** Accesses comprehensive system manual records stored on the operating system to document structural syntax parameters, protocol flags, and utility definitions (e.g., executing `man ls`).

## 8. Directory & Object Triage (File System Interactivity)
Managing the state, location, and structural existence of filesystem components is performed via six core manipulation tools:

*   `touch <file>` — Instantiates an empty, unformatted file object requiring sequential tool intervention (such as `echo` or `nano`) to append textual metadata.
*   `mkdir <directory>` — Constructs a new, independent directory node within the designated path perimeter.
*   `cp <source> <destination>` — Clones data contents from an existing source entity into a completely new standalone file destination.
*   `mv <source> <destination>` — Teleports an object file path or renames file system items by altering or merging target parameters.
*   `rm <file>` — Deletes file system entries. *To clear directory structures and subfolders recursively, the recursive override flag must be appended (`rm -R <directory>`).*
*   `file <target>` — Evaluates the underlying machine header structure of a target object to determine its true asset file format (e.g., `ASCII text`), exposing instances where file extensions (`.txt`) have been spoofed or omitted.

## 9. Granite Authorization Access Control Lists (ACLs)
Linux utilizes highly granular file permission layers to restrict access across three distinct system entities: **Owner (User), Group, and Others**. Evaluating security states via long-form listings (`ls -lh`) reveals ten specific parameter columns.

### Permissions Layout Structural Model
```text
  User      Group     Others
[ r w x ] [ r w x ] [ r w x ]
  4+2+1     4+2+1     4+2+1
```

### Absolute Octal Numeric Conversion Array
Permissions are calculated dynamically by converting symbolic access tags into concrete numeric sums:
*   **Read (r)** = `4`
*   **Write (w)** = `2`
*   **Execute (x)** = `1`

| Symbolic | Numeric | Administrative Context & Meaning |
| :--- | :--- | :--- |
| `rwxr-xr-x` | **755** | Full owner execution rights; groups and external accounts are restricted to read/execute operations. |
| `rw-r--r--` | **644** | Standard data object asset baseline. Owner can read/write; all other groups are limited to read-only visibility. |
| `rwx------` | **700** | Strict tenant isolation. Full interactive domain control reserved strictly for the object owner. |

### Operational State Enforcement (`chmod`)
Modifying security exposure levels or restricting directory access is managed via the `chmod` binary:
```bash
chmod 750 system_overview.txt
```
*   *Security Triage Impact:* Granting `750` assigns full administrative control to the owner, structural read/execute validation to the group, and explicitly cuts off all external guest (`others`) access (`0`) to isolate sensitive telemetry data.

## 10. Privilege Level Alteration (Session Flipping)
Pivoting between environment user configurations to inspect file visibility constraints or handle service processes is managed using the `su` (Switch User) utility.

*   `su <username>` — Interactively spawns a secondary shell instance targeting the specified account while keeping the previous environment variables and home directory shell path intact.
*   `su -l <username>` (or `su - <username>`) — Instantiates a clean, isolated login shell session, completely inheritance-wiping the environment to load the fresh user's dedicated path properties and home directory configurations automatically.

## 11. Core Linux Directory Hierarchy & Security Targets
Understanding root directory layouts allows a security analyst to pinpoint critical file spaces during an incident or vulnerability evaluation:

*   **`/etc` (System Configuration Space):** Stores vital operating system parameters and policy frameworks. Notable targets include:
    *   `/etc/sudoers` — Regulates which user accounts can execute root-level tasks.
    *   `/etc/passwd` & `/etc/shadow` — Stores local user accounts and cryptographically salted password signatures (e.g., using `SHA-512` structures).
*   **`/var` (Variable Dynamic Logging Directory):** Hosts files that fluctuate in data size over time. The primary target for SOC analysis is `/var/log`, which tracks dynamic events generated by running network services and application processes.
*   **`/root` (Superuser Home Space):** The isolated home directory reserved exclusively for the `root` administrative account, remaining distinct from the shared standard `/home/username` boundaries.
*   **`/tmp` (Volatile Storage Directory):** A temporary cache that flushes and resets automatically during system reboots. 
    *   *Pentesting Triage Significance:* By default, `/tmp` leaves standard write privileges wide open to all local users. Attackers and auditors frequently leverage this globally writable folder to plant enumeration tools or script arrays without hitting permission blocks.

## 12. Terminal Text Manipulation Controls
Modifying configuration templates or updating local exploit notes requires lightweight interactive terminal text editors:

*   **`nano <filename>`** — An accessible, modeless CLI text editor. Operations are executed utilizing simple key combinations anchored by the `Ctrl` key symbol (`^`), such as saving data via `Ctrl + O` (`^O`) and exiting via `Ctrl + X` (`^X`).
*   **`vim <filename>`** — An advanced, highly scalable modal text editor built natively into virtually all Linux terminal shells. It features distinct operational modes (Insert vs. Command mode), highly customizable hotkeys, and custom code syntax highlighting for script auditing.

## 13. Network File Ingress & Delivery Infrastructure
Data exfiltration, tool delivery, and automated software extraction across remote hosts rely on direct file transfer tools:

*   **`wget <URL>` (Web Get):** Downloads operational scripts, binaries, or assets directly from remote HTTP platforms using standard web navigation endpoints.
*   **`scp <source> <destination>` (Secure Copy):** Transfers localized folders or objects across networks by routing data packets securely inside an active, encrypted SSH tunnel workspace.
    *   *Push Local File to Remote Host:*
        ```bash
        scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt
        ```
    *   *Pull Remote File to Local Host:*
        ```bash
        scp ubuntu@192.168.1.30:/home/ubuntu/documents.txt notes.txt
        ```
*   **Python Instant Web Server Hosting:** Spawns a volatile, temporary port-listening instance directly inside the terminal to serve localized folders to external auditing targets instantly:
    ```bash
    python3 -m http.server
    ```
    *   *Operational State:* Runs transparently inside the active working terminal window on default port `8000` until explicitly halted via a cancel execution command (`Ctrl + C`). Ingress machines pull target items utilizing web download operators mapping the server IP (e.g., `wget http://10.48.178`).

## 14. Kernel Process Administration & Monitoring
Programs running on the system are managed by the kernel as isolated structures called **Processes**. Each process is assigned an incremental, unique decimal identification key called a **PID (Process ID)** starting from system boot initialization.

*   `ps` — Renders a static, snapshot list tracking the running processes associated with the active user's current console terminal shell.
*   `ps aux` — Expands process discovery scope wide open to capture all operations executing across all system accounts (e.g., `root`, `tryhackme`), including headless daemons running independent of an active terminal window.
*   `top` — Launches an interactive dashboard that provides real-time system monitoring. This view dynamically flushes and updates computing infrastructure utilization metrics every 10 seconds.

### High-Privilege Signal Interception (`kill`)
Process structures can be modified or safely terminated by forwarding explicit numeric or symbolic flags directly to the kernel plane using the `kill` command syntax:
*   `kill -SIGTERM <PID>` (or standard `kill <PID>`) — Instructs the application to stop execution while giving it adequate buffer scope to perform cleanup routines and drop file locks cleanly.
*   `kill -SIGKILL <PID>` (or `kill -9 <PID>`) — Enforces an instantaneous kernel-level shutdown sequence, terminating the binary immediately without processing any background cleanup operations.
*   `kill -SIGSTOP <PID>` — Completely stops and freezes an active process execution state mid-flight.

## 15. The Initialization Process & Isolated Namespaces
*   **Namespaces:** The core operating system splits computing parameters (CPU cores, RAM limits, thread paths) into isolated logical clusters called namespaces. This architecture ensures that child binaries running inside one environment slice remain hidden from other environments.
*   **PID 0 & PID 1 (`systemd`):** The bootloader loads **PID 0** to bring the system online. This immediately spawns **PID 1**, which on modern Ubuntu deployments is handled by the `systemd` daemon. Every application or tool subsequently launched on the host executes as a child process of `systemd`.
*   `systemctl [option] [service]` — Directly interacts with the `systemd` supervisor core to alter system service run states. Supported switches include `start`, `stop`, `enable`, `disable`, and `status` (e.g., `systemctl start apache2`).

## 16. Context Foregrounding & Backgrounding Control
To prevent a long-running process or repetitive script array from locking terminal access, analysts balance tasks between background and foreground memory structures:

*   **Direct Backgrounding (`&`):** Appending an ampersand to an execution command routes the process output straight to background memory buffers (e.g., `echo "Hi THM" &`). This returns an active console prompt immediately along with the job tracking token number.
*   **Shell Suspend (`Ctrl + Z`):** Pauses and freezes an active foreground process mid-execution, passing the suspended runtime token safely into background storage.
*   `fg` — Pulls the highest priority background or suspended job index back into focus, restoring interactive control to the terminal.

## 17. Automated Task Scheduling (Cron Jobs)
The system leverages the persistent `cron` daemon to run routine maintenance, system updates, or logging tasks automatically at pre-scheduled intervals. Analysts interact with this service by modifying **Crontab files** using `crontab -e`.

### Crontab Timing Configuration Matrix
Every execution string defined inside a crontab profile requires 6 specific positional parameters:
```text
 *     *     *     *     *    [COMMAND]

 |     |     |     |     |
Min  Hour   DOM  Mon   DOW
```
*Note: The wildcard asterisk (`*`) is processed as an unconditional placeholder value, indicating that the target command should run at every interval increment for that specific positional column.*

*   *Practical Automation Example:* To establish a background task that backs up a user's local documents folder and appends it to a backup directory every 12 hours, the configuration line is defined as follows:
    ```text
    0 */12 * * * cp -R /home/tryhackme/Documents /var/backups/
    ```

## 18. Package Management & Software Repositories
Installing software or extending core kernel components on Ubuntu environments relies on the Advanced Package Tool (`apt`) framework to manage verified software archives.

*   **Cryptographic Key Integrity Verification (GPG Keys):** To prevent malicious supply-chain insertions or tampered software installation attempts, systems validate downloads against public **GNU Privacy Guard (GPG)** signatures provided directly by developers. If keys mismatch or are missing from the local trusted registry, the installation aborts.
*   **Core Repository Pipeline Sequences:**
    1.  *Trust GPG Identifier:*
        ```bash
        wget -qO - https://sublimetext.com | sudo apt-key add -
        ```
    2.  *Synchronize Package Databases:* `sudo apt update` — Polls all defined registry streams to register new application indexes.
    3.  *Execute Binary Installation:* `sudo apt install sublime-text`
    4.  *Teardown / Software Removal:* `sudo apt remove sublime-text`

## 19. Defensive Log Triage & Rotation Architecture
The operating system pipes execution metrics and network transaction flags directly into the `/var/log` folder directory. System administrators configure automatic log rotation systems to compress and lifecycle data automatically to preserve drive space.

### Essential Security Analysis Targets:
*   **`access.log` / `error.log` (Web Traffic Verification):** Maintained by web services (such as `Apache2`). Essential for auditing system calls, locating web scanning parameters (`gobuster`), or mapping path navigation strings executed by external threat agents.
*   **`/var/log/fail2ban.log`:** Monitors and alerts on active network authentication bypass indicators and localized brute-force attack vectors.
*   **`/var/log/ufw.log`:** Records network entry anomalies blocked at the firewall policy layer.
