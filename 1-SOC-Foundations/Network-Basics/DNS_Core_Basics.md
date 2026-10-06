# 🗺️ Domain Name System (DNS) Core Architecture & Hierarchy

**Module:** SOC Foundations / Network Basics  
**Room Source:** TryHackMe — Introduction to DNS  
**Objective:** Deconstruct the logical domain hierarchy tree, classify standard DNS resource record types, trace the network resolution path from client request to authoritative lookup, and identify infrastructure components.

---

## 🏗️ The Domain Hierarchy Structural Tree

The **Domain Name System (DNS)** functions as the automated logical lookup infrastructure of the internet. It maps human-readable alphanumeric strings into machine-routable numerical **IP Addresses** (e.g., mapping `tryhackme.com` to its backend host layer at `104.26.10.229`).

The namespace is organized into a strict, right-to-left inverted hierarchical tree structure:

```text
               [ . ] (Root Zone Domain)
              /     \
         [.com]     [.org]       [Found? Stops]
      │
(2. Cache Miss)
      ▼
[Recursive Resolver] ──(3. Queries)──> [Root Name Servers (.)]
      │                                       │
(6. Relays IP)                         (4. Returns TLD Server IP)
      │                                       ▼
      │◀─────────────────────────────── [TLD Name Servers (.com)]
      │                                       │
      │                                (5. Returns Authoritative IP)
      ▼                                       ▼
[Authoritative DNS Server] ───────────────────┘
(Maintains True Source Records & TTL)
```

1.  **Local Cache Interrogation:** The endpoint OS checks its internal local memory cache or physical static hosts file. If a match is found within its operational limits, the lookup terminates instantly.
2.  **Recursive Server Despatch:** If a cache miss occurs, the system issues a lookup request outward to a designated **Recursive DNS Server** (typically hosted by the ISP or configured via a public provider like Google's `8.8.8.8` or Cloudflare's `1.1.1.1`).
3.  **The Root Zone Check:** If the Recursive server does not contain the answer cached in its local table, it hits the internet's **Root DNS Servers (`.`)**. The Root server analyzes the query suffix and redirects the recursive node to the matching **TLD Server**.
4.  **TLD Referral Handling:** The **TLD Server** isolates the domain registry parameters and returns the IP address mapping for the domain's specific **Authoritative DNS Nameservers** (e.g., Cloudflare's `://cloudflare.com`).
5.  **Authoritative Answer Retrieval:** The Recursive resolver connects directly to the domain's **Authoritative DNS Server**, which holds the true record schema. The Authoritative host reads the database record and returns the target mapping alongside a hardcoded **TTL (Time to Live)** parameter.
6.  **Client-Side Delivery & Caching:** The Recursive server commits a duplicate copy of the record to its local volatile storage for the amount of time specified by the TTL integer, then forwards the final resolved payload back to the client computer to allow communication to initialize.
