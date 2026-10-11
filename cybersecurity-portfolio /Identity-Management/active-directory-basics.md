# Comprehensive Lab Report: Active Directory Domain Services (AD DS) Infrastructure

## 1. Architectural Scaling & Centralized Identity Management (IAM)
In localized networking models, managing assets on a machine-by-machine basis becomes structurally unsustainable as an organization scales. Managing local accounts, access configurations, and endpoint adjustments individually creates immense administrative overhead and severe visibility gaps.

To solve this scaling limitation, enterprise networks transition to a **Windows Domain** model. This architecture groups users, endpoints, and corporate resources under a single administrative boundary.

```text
               [ GLOBAL DOMAIN PERIMETER ]
                           │
                           ▼
          [ DOMAIN CONTROLLER (DC) ENGINE ]
                           │
             ┌─────────────┴─────────────┐
             ▼                           ▼
    [ Directory Services ]      [ Network Security ]
     (AD DS Database Hive)       (Centralized DACLs)
```

### Core Components of the Domain Perimeter:
*   **Active Directory Domain Services (AD DS):** The centralized identity and access management database directory. It stores network entity objects, permissions, and configuration schema in a unified repository.
*   **Domain Controller (DC):** The dedicated server hosting active AD DS database instances. It acts as the central authentication clearinghouse, processing identity verification strings (e.g., checking user credentials across campus networks) and applying group security policies natively over the wire.

## 2. Object Classification & Security Principals
Active Directory categorizes network entities into functional units called **Objects**. Objects that can be authenticated by the domain subsystem and explicitly assigned access rights over shared resources (such as printers, storage partitions, or network shares) are classified as **Security Principals**.

### A. User Objects (Identities)
Users represent entities capable of acting upon corporate assets. They are split into two primary operational profiles:
*   **Human Identities (Employees):** Standard accounts representing corporate staff members who require specific network access to execute daily tasks.
*   **Service Accounts:** Non-human automated identities mapped to background database engines or enterprise web services (e.g., IIS, MSSQL). These accounts are hardened to hold strictly the minimum system permissions required to run their underlying services.

### B. Machine Accounts (Computer Perimeters)
Every physical workstation, virtual machine, or server that joins the domain perimeter receives a dedicated Machine Object. 
*   **Authentication & Rotation:** Machine objects function as security principals, holding their own domain authentication keys. To prevent offline hijacking, Windows automatically rotates machine account passwords using **120-character random string arrays**.
*   **Naming Convention Triage:** Machine accounts are immediately identifiable in logs and security audits by a trailing dollar sign suffix appended to the hostname (e.g., a domain asset named `DC01` authenticates via the security principal account **`DC01$`**).

## 3. Security Group Hierarchies & Privileged Access Profiles
Security Groups aggregate multiple users or machine accounts into a single security principal object. This allows administrators to grant access rights across entire organizational departments rather than modifying individual user profiles sequentially.

### Standard Administrative Group Layouts:

| Highly Privileged Security Group | Core Infrastructure Operational Scope |
| :--- | :--- |
| **Domain Admins** | Holds absolute administrative control across the entire domain perimeter, including the ability to manipulate Domain Controllers. |
| **Server Operators** | Authorized to manage Domain Controllers directly; restricted from altering administrative group membership structures. |
| **Backup Operators** | Granted structural authorization to bypass file system access permissions explicitly to execute backup operations on sensitive volumes. |
| **Account Operators** | Authorized to instantiate, modify, or terminate standard user objects across the directory database environment. |
| **Domain Users / Computers** | Global default groups automatically collecting all configured user accounts or endpoint assets within the domain. |

## 4. Logical Segmentation: Organizational Units (OUs)
The directory database relies on **Organizational Units (OUs)** to arrange security principals into a clean, hierarchical container structure.

*   **The Purpose:** OUs segment users and host computers into logical groups based on distinct security and compliance requirements (e.g., keeping the *Sales* department separated from the *IT Infrastructure* team).
*   **Constraint Boundary:** Unlike security groups, a security principal object can reside within **only one Organizational Unit container at any given time**.
*   **Management Entry Point:** Enterprise engineers handle directory structural updates by authenticating into a Domain Controller and initializing the native Microsoft Management Console snap-in:
    ```text
    Start Menu ➔ Administrative Tools ➔ Active Directory Users and Computers (dsa.msc)
    ```
## 5. Directory Container Subsystems & Default Registries
Active Directory initializes a standardized set of default system containers and structural Organizational Units (OUs) to govern network objects across a domain-wide footprint:

*   **`Builtin/` Container:** Houses default localized administrative and operator security groups inherited by default across all domain-joined Windows hosts.
*   **`Computers/` Container:** The global landing pad for endpoints. Any physical workstation, virtual machine, or server that executes a domain-join sequence is dropped into this unsegmented folder by default.
*   **`Domain Controllers/` OU:** A critical, dedicated default container explicitly reserved for hosting Active Directory Domain Controllers (DCs). 
    *   *Hardening Advisory:* This container applies specialized, high-security baseline policies across all DCs; standard endpoints must never be moved into this OU.
*   **`Users/` Container:** Holds domain-wide default user profiles and security groups (such as *Domain Users* and *Domain Admins*).
*   **`Managed Service Accounts/` Container:** Dedicated storage vault managing specialized accounts utilized natively by automated background system services to enforce managed password cycling.

## 6. Security Architecture: OUs vs. Security Groups
While both mechanisms handle object classification, their fundamental purposes inside an enterprise access control model are entirely distinct:

| Structural Component | Primary Administrative Objective | Cardinality Constraints |
| :--- | :--- | :--- |
| **Organizational Units (OUs)** | Used to arrange objects hierarchically to deploy targeted **Group Policy Objects (GPOs)** and configuration restrictions based on corporate roles. | Strict single-tenant mapping. A security principal can belong to **only one OU container at a time** to prevent conflicting policy execution trees. |
| **Security Groups** | Used explicitly as security principals to assign access control permissions over network resources (e.g., shared file servers, printers, network zones). | Multi-tenant mapping. An object can simultaneously hold membership across **unlimited security groups** to inherit cumulative resource access rights. |

## 7. Directory Hardening: Accidental Deletion Protection Bypass
Active Directory object properties include an explicit security constraint to prevent administrative accidents from dropping critical structural assets out of the live database hive.

### Incident Recovery and Deletion Protocol:
When an obsolete organizational unit must be permanently purged from the directory database, attempts to delete the node will fail with an Access Denied error message. To successfully execute the decommissioning routine, an analyst must alter the interface viewing state:
```text
DSA MMC Console ➔ View Menu ➔ Enable: Advanced Features ➔ Right-Click Target OU ➔ Properties ➔ Object Tab ➔ Uncheck: "Protect object from accidental deletion"
```
*   *Security Impact:* Disabling this parameter allows the administrator to successfully execute the deletion routine. This action drops the target OU alongside all child security principals, user objects, and nested subfolders nested underneath it in the directory tree.

## 8. Privilege Isolation: The Principle of Least Privilege via Control Delegation
To prevent administrative credential bloating, organizations leverage **Delegation of Control**. This mechanism allows a Domain Administrator to securely distribute specific, low-level operational permissions over an Organizational Unit container to specific support technicians without adding those technicians to high-privilege groups like *Domain Admins*.

### Practical Helpdesk Delegation Pipeline:
A standard tier-1 helpdesk technician account (e.g., `phillip`) can be delegated password-reset authority over a target department container (e.g., the `Sales` OU):
```text
Right-Click Target OU ➔ Delegate Control ➔ Add Target User Principal ➔ Select: "Reset user passwords and force password change at next logon"
```

### Scripted Triage Execution via PowerShell:
A delegated technician who lacks permissions to run the graphical `dsa.msc` management console can execute password lifecycle resets directly using the native **ActiveDirectory PowerShell Module**:

1.  *Execute Secure Remote Password Reset:*
    ```powershell
    Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose
    ```
2.  *Enforce Immediate Password Expiration Lifecycle:* Forces a mandatory credential update sequence at the next network authentication prompt to maintain identity isolation:
    ```powershell
    Set-ADUser -ChangePasswordAtLogon $true -Identity sophie -Verbose
    ```

## 9. Defending the Endpoint Fleet: Device Segregation Strategy
Leaving all corporate machines pooled in the default `Computers/` container creates severe security blind spots, making it difficult to enforce tailored defense-in-depth configurations. An essential structural hardening task involves moving devices into distinct OU pillars based on their operational purpose:

### A. Workstations (User Endpoints)
The day-to-day productivity machines utilized by standard employees for localized operations, emails, and web browsing. 
*   *SOC Hardening Rule:* Highly privileged domain accounts (such as *Domain Admins*) must never log into standard workstations. Doing so leaves high-privilege credential hashes vulnerable to extraction from volatile memory if the endpoint is compromised via an entry-level phishing or malware vector.

### B. Servers (Infrastructure Nodes)
Dedicated network nodes providing services to the enterprise (e.g., file shares, internal web servers, databases). These assets require specialized, restricted group policies, aggressive logging levels, and limited interactive user login rights.

### C. Domain Controllers (Identity Vaults)
The ultimate authority tier holding the entire identity database hive, including the hashed password secrets for every account across the enterprise. 
*   *Forensic Impact:* Domain Controllers represent the highest-value tier on the network. They require strict network segmentation, complete isolation from standard asset zones, and continuous log auditing to immediately detect unauthorized administrative access attempts.
## 10. Fleet Tiering & Hierarchical Device Relocation
Leaving endpoints pooled within the default `Computers/` folder breaks core **Least Privilege** parameters. To establish specialized policy boundaries, an administrator must implement an intentional device tiering model directly under the domain root partition:

*   **Workstations OU:** Groups user-facing client endpoints (e.g., corporate laptops and desktop PCs). Isolating these nodes allows defenders to apply aggressive endpoint logging configurations and prevent high-privilege administrative token exposures on daily-use machines.
*   **Servers OU:** Enforces strict server-tier boundaries for assets running enterprise network services (e.g., file shares and application backends), restricting interactively initialized user sessions.
*   **Asset Migration:** Executing object handoffs out of the default storage path allows administrators to target child organizational perimeters precisely with customized compliance templates.

## 11. Group Policy Object (GPO) Architecture & Inheritance Rules
**Group Policy Objects (GPOs)** allow domain administrators to define and enforce uniform security baselines and registry configurations across users and computers throughout the domain tree.

### Fundamental Policy Mechanics:
*   **Scope & Linking:** GPOs are instantiated within the centralized *Group Policy Objects* directory. To take effect, they must be explicitly linked to a target domain root or specific Organizational Unit container.
*   **Inheritance Properties:** GPOs apply automatically down the directory tree. Any child container recursively inherits the policy layers applied to its parent OU node unless explicit inheritance blocks or overrides are configured.
*   **GPO Structure Segregation:** Every policy object maintains a strict internal separation between the **Computer Configuration** engine (modifying low-level system properties, local security policies, and registry keys upon boot) and the **User Configuration** engine (applying user space restrictions and shell preferences at logon).
*   **Security Filtering:** Allows engineers to narrow down the execution target boundary by overriding the global default group layout (*Authenticated Users*) to restrict policy ingestion to specific identities or machine sets.

### Administrative GPO Management Workflow:
```text
Start Menu ➔ Administrative Tools ➔ Group Policy Management (gpmc.msc)
```

## 12. Local Host Hardening via Targeted Security Policies
Two high-priority GPO policy paths were configured and deployed to reduce the local attack surface and mitigate lateral movement risks across the environment:

### A. Attack Surface Reduction: Control Panel Access Restrictions
To block standard users from modifying security settings or uncovering localized software details, a custom GPO was created:
*   *GPO Name:* `Restrict Control Panel Access`
*   *Policy Path:* `User Configuration ➔ Policies ➔ Administrative Templates ➔ Control Panel`
*   *Enforced Setting:* Enable `Prohibit Access to Control Panel and PC settings`
*   *Target Deployment:* Linked directly to the standard corporate departments (`Marketing`, `Sales`, `Management` OUs) while intentionally bypassing the `IT` container. This enforces a hard block against system preference manipulation for non-technical employees.

### B. Session Security: Automated Machine Inactivity Lockout
To mitigate the risk of exposed physical workstations being hijacked when left unattended, a global computer configuration GPO was built:
*   *GPO Name:* `Auto Lock Screen`
*   *Policy Path:* `Computer Configuration ➔ Policies ➔ Windows Settings ➔ Security Settings ➔ Local Policies ➔ Security Options`
*   *Enforced Setting:* Set `Interactive logon: Machine inactivity limit` to **300 seconds (5 minutes)**.
*   *Target Deployment:* Linked directly to the root domain (`thm.local`). Because all device OUs function as child objects under the root domain, they inherit this mandatory screensaver lock sequence automatically.

## 13. GPO Distribution Ingress & Forced Synchronization
*   **The SYSVOL Share Infrastructure:** GPO templates, startup scripts, and policy registry tables are distributed throughout the network via a high-availability share called **`SYSVOL`**. This network share points to the local directory path `C:\Windows\SYSVOL\sysvol\` on each Domain Controller. Endpoints communicate with this directory over the network to periodically sync their configurations.
*   **Replication Latency:** By default architectural configuration, background GPO synchronization checks execute on a standard loop that can take up to **2 hours** to process changes across the fleet.
*   **Forced Triage Synchronization:** During a suspected incident or active deployment triage, an analyst can bypass replication latency to force immediate, local security policy ingestion by running this command in a terminal:
    ```powershell
    gpupdate /force
    ```
## 14. Network Authentication Ecosystems: Kerberos vs. NetNTLM
Active Directory Domain Services centralize identity credentials within Domain Controllers. When endpoints or applications attempt resource validation across the wire, authentication routines are processed using two core network protocols:

### A. Kerberos Authentication (The Default Modern Matrix)
Kerberos serves as the primary, high-security default network authentication protocol in modern Active Directory perimeters. It operates utilizing a cryptographic ticket-granting model, allowing identities to prove previous successful authentication state validation to network services without passing raw credentials over transmission mediums continuously.

#### The Three-Step Ticket Lifecycle:
1.  **Authentication Service Request (AS-REQ / AS-REP):** The client transmits an interactive request containing their username and a timestamp encrypted utilizing a cryptographic key derived from their plaintext password. This is handled by the **Key Distribution Center (KDC)** daemon residing on the Domain Controller. The KDC decrypts the validation token and responds with a **Ticket Granting Ticket (TGT)** alongside a ephemeral *Session Key*. 
    *   *Cryptographic Hardening Note:* The TGT payload is encrypted using the password hash of the high-privilege system account **`krbtgt`**. Because the client lacks this key, they cannot read or alter the TGT data, which securely embeds a hidden copy of the Session Key for the KDC to verify later.
2.  **Ticket Granting Service Request (TGS-REQ / TGS-REP):** When the client requires access to a specific network resource (e.g., a file share or web database), they present their TGT to the KDC alongside a designated **Service Principal Name (SPN)**. The KDC decrypts the TGT, extracts the session key to validate the client's identity, and issues a targeted **Ticket Granting Service (TGS)** ticket encrypted using the password hash of the *Service Owner Account* (the user or machine context running the target application service).
3.  **Application Authentication (AP-REQ / AP-REP):** The client forwards the TGS ticket straight to the target resource server. The service owner decrypts the TGS utilizing its own local secret hash to extract the embedded *Service Session Key*, successfully completing mutual authentication and opening the communication channel.

```text
  [ CLIENT ] ══════ 1. AS-REQ (Password Key) ══════> [ KDC ENGINE ]
             <══════ 2. AS-REP (TGT + Session Key) ══ (DC Node)
             │
             ═══════ 3. TGS-REQ (TGT + SPN) ════════>
             <══════ 4. TGS-REP (TGS Ticket) ════════
             │
             ═══════ 5. AP-REQ (TGS Ticket) ════════> [ SERVICE SERVER ]
                                                      (Decrypts via Host Hash)
```

### B. NetNTLM Authentication (The Legacy Challenge-Response Pipeline)
NetNTLM functions as a legacy, challenge-response network authentication protocol maintained strictly to sustain backward compatibility loops with old applications.

#### The Challenge-Response Flow:
1.  **Negotiation:** The client issues an authentication ingress request string targeting the resource server.
2.  **Challenge:** The resource server generates a pseudo-random cryptographic integer called a **Challenge** and drops it back to the client.
3.  **Response:** The client mixes their local NTLM password hash with the challenge integer to execute a cryptographic proof token called the **Response**, pushing it back to the server.
4.  **DC Validation Clearinghouse:** The server transmits both the raw Challenge and the client's Response to a Domain Controller. The DC extracts the target user's NT hash from its primary database hive, recomputes the expected challenge-response calculation, and flags whether the signatures match, passing an access allowed or denied token back down the pipeline.
    *   *SAM Account Variance:* If an identity logs in utilizing a local machine account rather than a domain-wide account, the host skips DC routing entirely. It clears the response math locally by matching it against hashes inside its local **Security Account Manager (SAM)** registry hive.
    *   *Security Triage Advisory:* NetNTLM never exposes plaintext credentials or raw NT hashes across the wire. However, NetNTLM responses are vulnerable to offline brute-forcing and network **NTLM Relay Attacks**. These protocols should be isolated and systematically disabled via GPO configurations wherever modern Kerberos paths can be forced.

## 15. Scaling Infrastructure: Trees, Forests, and Trust Topologies
As enterprise footprints expand across geographic borders or corporate mergers, Active Directory structures scale outwards from simple single-domain boundaries to multi-tier corporate architectures.

### A. Active Directory Domain Trees
When an enterprise deploys multiple domains that share a contiguous, hierarchical namespace boundary (e.g., `thm.local`), those nodes are linked into a **Domain Tree**.
*   *Implementation Mechanics:* The parent root domain (`thm.local`) can branch into specialized subordinate domains like `uk.thm.local` and `us.thm.local`. Each subdomain maintains independent database partitions, dedicated Domain Controllers, and distinct local **Domain Admins** teams to isolate administration and target country laws cleanly.

### B. Enterprise Security Boundaries: Active Directory Forests
A **Forest** represents the ultimate, top-tier structural security boundary within an Active Directory infrastructure environment. It is formed by linking multiple separate Domain Trees that operate under distinct, non-contiguous namespaces (e.g., connecting a tree like `thm.local` with an acquired entity tree like `mht.local`).

#### Administrative Tiering Mechanics:
*   **Domain Admins:** Holds complete administrative power restricted strictly within their localized single-domain boundaries.
*   **Enterprise Admins Group:** A highly privileged security group instantiated exclusively within the forest root domain. Members hold absolute administrative access across every connected domain tree, child OU container, and Domain Controller throughout the entire corporate forest network.

### C. Resource Federation via Trust Relationships
To allow an identity authenticated within one domain subtree (e.g., `uk.thm.local`) to access shared files or resource services hosted inside an entirely separate namespace partition (e.g., `asia.mht.local`), Active Directory deploys **Trust Relationships**.

#### Trust Directionality Architecture:
*   **One-Way Trust:** Establishes a directional bridge where **Domain AAA trusts Domain BBB**. This means users residing in Domain BBB can pass through the security perimeter to be authorized over assets inside Domain AAA.
*   **The Trust Pointer Axiom:** In modern access control security, the direction of a trust relationship arrow points in the **exact opposite direction** of the real-world access flow path. The trust pointer points toward the trusted identity domain provider, while access travels toward the resource hosting system.

```text
    [ DOMAIN AAA ]  ┘
```
