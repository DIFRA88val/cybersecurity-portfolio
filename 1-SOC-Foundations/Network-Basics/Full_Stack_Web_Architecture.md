# 🌐 Full-Stack Enterprise Web Architecture & Availability

**Module:** SOC Foundations / Network Basics  
**Room Source:** TryHackMe — Putting It All Together: Web Architectural Design  
**Objective:** Synthesize edge distribution networks, classify enterprise backend and database tiers, audit web server daemon configurations, and trace the processing boundaries between client-side frontend renders and server-side dynamic execution.

---

## 🏗️ The End-to-End Enterprise Web Pipeline

When an asset request flows past standard DNS routing lookups, a multi-tiered infrastructure layers defensive filtering, traffic optimization, file caching, and application processing before the client browser renders a single pixel.

```text
[Client Browser] ──(HTTP/S Request)──> [ WAF ] ──> [ CDN ] ──> [ Load Balancer ]
                                                                      │
                                                   ┌──────────────────┴──────────────────┐
                                                   ▼                                     ▼
                                          [ Web Server #1 ]                     [ Web Server #2 ]
                                           (Nginx / Apache)                      (Nginx / Apache)
                                                   │                                     │
                                                   ▼                                     ▼
                                         [ Backend Processing ]                [ Backend Processing ]
                                          (PHP / Python / JS)                   (PHP / Python / JS)
                                                   │                                     │
                                                   └──────────────────┬──────────────────┘
                                                                      ▼
                                                              [ Database Tier ]
                                                          (MySQL / Postgres / Mongo)
```

---

## 🛡️ Edge Infrastructure & Availability Services

### 1. Web Application Firewalls (WAF)
A WAF serves as an inline protocol filter positioned directly at the network's perimeter edge.
*   **Operational Mechanism:** Executes real-time inspection of inbound HTTP/HTTPS traffic matrices. It decodes request bodies, header fields, and parameter lines to detect common exploit signatures (e.g., OWASP Top 10 indicators like SQL Injection, Cross-Site Scripting, or Automated Bot scrapers).
*   **Rate Limiting:** Enforces strict execution quotas restricting the maximum number of requests allowed from a single source IP per second, proactively mitigating brute-force and Denial-of-Service (DoS) vectors. If an entry triggers an attack profile, the WAF drops the connection instantly.

### 2. Load Balancers
High-availability production ecosystems deploy clusters of redundant web hosts. Load Balancers ingest inbound user traffic first and route connections downward based on programmatic metrics:
*   **Round-Robin Routing:** Sequentially hands each incoming request to the next backend host in the cluster ring array.
*   **Weighted/Least-Connection Routing:** Tracks active connection quotas across the cluster matrix and pushes incoming connections straight to the host handling the lowest current CPU processing footprint.
*   **Health Status Monitoring:** Continuously transmits automated keep-alive probes (Health Checks) down to the host nodes. If a server stops responding or throws service errors, the load balancer removes it from the routing group until it passes the check again.

### 3. Content Delivery Networks (CDN)
CDNs cut down on latency and strain on internal infrastructure by caching static application resources (images, video streams, text sheets, large script frameworks) across globally distributed edge arrays.
*   **Operational Mechanism:** Calculates the physical geographic proximity of the requesting client node and fulfills the asset download straight from the closest regional edge cache server, completely saving central infrastructure bandwidth.

---

## 💾 Core Hosting Engines & Data Warehouses

### 1. Web Server Daemons & Root Directories
A web server is a specialized background software suite engineered to monitor specific protocol sockets (e.g., TCP Ports 80 and 443) and serve hosted materials via the HTTP protocol structure.

| Web Engine Daemon | Standard Operating System | Default Structural Hard Drive Web Root Directory |
| :--- | :--- | :--- |
| **Nginx** | Linux Distributions (Ubuntu, Debian, RHEL) | `/var/www/html` |
| **Apache (httpd)** | Linux Distributions (Ubuntu, Debian, RHEL) | `/var/www/html` |
| **Internet Information Services (IIS)** | Windows Server Infrastructure | `C:\inetpub\wwwroot` |
| **NodeJS** | Cross-Platform / Application-Driven | Programmatically designated inside file structures |

### 2. Virtual Hosts (Multi-Tenant Routing Configuration)
Web servers leverage **Virtual Hosts** (text-based mapping configuration files) to serve dozens of entirely separate domain names from a single physical computer system and IP address.
*   **The Check Mechanism:** The web server daemon parses the inbound `Host:` header line hidden within the incoming client HTTP request block. It compares the string against its active Virtual Host configuration tables.
*   **Path Mapping Separation:** If a match is found, it directs the connection to the designated directory (e.g., mapping `one.com` to `/var/www/website_one` and `two.com` to `/var/www/website_two`). If no match exists, it surfaces the default server index.

### 3. The Database Tier
Centralized backend engines that systematically structure, read, modify, and store persistent information profiles (user records, transactional history, application states).
*   **Relational Datastores (SQL):** Enforce rigid, schema-mapped tabular layouts utilizing Structured Query Language (e.g., *MySQL*, *PostgreSQL*, *Microsoft SQL Server*).
*   **Non-Relational Datastores (NoSQL):** Store unstructured or highly dynamic data using document models, key-value stores, or graph maps (e.g., *MongoDB*).

---

## 🔀 The Operational Processing Boundary: Static vs. Dynamic

Understanding the exact line where backend processing ends and frontend rendering begins is a requirement for analyzing data-handling vulnerabilities.

### 1. Static Web Content
*   **Profile:** Asset structures that do not change based on user characteristics or input variables (e.g., raw images, local `.css` stylesheets, hardcoded `.html` layouts).
*   **Execution:** Serviced instantly from the local storage drive by the web server daemon without triggering external scripts or script interpreters.

### 2. Dynamic Web Content
*   **Profile:** Content that changes adaptively based on real-time factors, parameterized inputs, account access parameters, or database responses (e.g., user home feeds, personalized dashboards, search result indexes).
*   **Execution:** Handled entirely behind the scenes at the **Backend Tier** utilizing specialized scripting or programming engines (*PHP*, *Python*, *Ruby*, *NodeJS*).

### 🔍 Scripting Context Isolation Case Study (PHP)
Consider a client submitting a dynamic query string to a server: `http://example.com`. The server file contains this code:

```php
<html><body>Hello <?php echo $_GET["name"]; ?></body></html>
```

*   **Behind-the-Scenes Processing:** The web server receives the request, identifies the `.php` extension, and forwards the file to the local PHP interpreter engine. The engine reads the `$_GET["name"]` variable, extracts the string `"adam"`, compiles the final plain HTML string, and passes it back to the web server daemon.
*   **The Generated Output Sent to Client:**
```html
<html><body>Hello adam</body></html>
```
*   **Security Insight:** The client **never sees a single line of backend PHP code** in their browser. They only receive the plain HTML output generated *after* backend execution. If input parameters are not securely handled before compilation, an application opens itself up to severe backend vulnerabilities like Database Injection or Server-Side Command Execution.
