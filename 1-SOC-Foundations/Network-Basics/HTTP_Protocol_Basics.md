# 🌐 Web Infrastructure: HTTP Protocols & Request Mechanics

**Module:** SOC Foundations / Network Basics  
**Room Source:** TryHackMe — How Web Works: HTTP & Request Architecture  
**Objective:** Deconstruct URL structural syntax components, break down the anatomy of raw HTTP transactional requests and responses, map standard operational CRUD verbs, analyze server status response brackets, and audit session persistence via cookies.

---

## 🏗️ URL System Anatomy

A **URL (Uniform Resource Locator)** serves as a standardized network syntax instruction specifying how and where to access a particular asset hosted across the internet.

```text
 http://tryhackme.com
 └───┘  └───────────┘ └─────────────┘ └┘ └───────┘ └────┘ └───┘
Scheme      Auth          Host       Port  Path    Query  Fragment
```

*   **Scheme:** Instructs the application on which communication rule set to enforce (`http`, `https`, `ftp`).
*   **User Authentication:** Optional inline credentials (`user:password`) utilized to pass immediate plain-text authentication tokens directly to a target service portal.
*   **Host:** The target system identity, represented as a human-readable **Domain Name** or a raw **IP Address**.
*   **Port:** The targeted endpoint application socket channel (typically defaults to `80` for unencrypted HTTP traffic and `443` for encrypted HTTPS tunnels).
*   **Path:** The literal file location directory structure or resource path targeted on the remote file system.
*   **Query String:** Extra dynamic parameters passed to the file path (`?id=1`), telling the server backend specifically which record subset to parse and render.
*   **Fragment:** A client-side positional anchor bookmark (`#task3`) pointing to a specific element index on the rendered page layout.

---

## 🔀 The Transactional Architecture: Requests vs. Responses

Because the HTTP protocol functions under a strict **Stateless** architecture framework, every request/response transaction executes completely independent of historical interactions.

### 📥 1. Raw Request Anatomy
Sent from the client browser agent to inform the backend server what asset footprint to fetch:

```http
GET /index.html HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0 Firefox/87.0
Referer: https://tryhackme.com
Accept-Encoding: gzip, deflate
[Blank Line]
```
*   *Line 1 (Request Line):* Combines the **HTTP Verb** (`GET`), the targeted directory **Path** (`/index.html`), and the baseline **Protocol Version** (`HTTP/1.1`).
*   *The Blank Line:* A critical protocol formatting requirement. Tells the parsing server engine that all administrative header options are terminated and any payload tracking bytes follow immediately below.

### 📤 2. Raw Response Anatomy
The structural response string returned by the web server framework containing transaction metrics and application content:

```http
HTTP/1.1 200 OK
Server: nginx/1.15.8
Date: Fri, 09 Apr 2021 13:34:03 GMT
Content-Type: text/html
Content-Length: 98
[Blank Line]
<html><head><title>TryHackMe</title></head><body>...</body></html>
```
*   *Line 1 (Status Line):* Details the matching protocol version followed by the numerical **HTTP Status Code** and its associated text description (`200 OK`).

---

## 🛠️ The CRUD Verb & Header Matrix

### 1. Primary HTTP Methods
Verbs specify the exact intent of a client request sent against a web resource path:
*   **`GET`:** Requests data from a web server. Payload variables are passed visible inside the URL query line.
*   **`POST`:** Submits fresh payload data blocks down into the web server to generate entirely new records. Data parameters are safely nested inside the hidden request body segment.
*   **`PUT`:** Uploads replacement data payloads to update or overwrite an existing host record profile.
*   **`DELETE`:** Instructs the host interface to erase a target record index or file path permanently.

### 2. Essential Header Parameters

| Header Type | Name String | Functional Telemetry Profile |
| :--- | :--- | :--- |
| **Request** | `Host` | Informs multi-tenant web servers which specific domain asset the client needs to connect with. |
| **Request** | `User-Agent` | Identifies the client software application and version layout to allow the server to properly compile CSS/JS. |
| **Request** | `Accept-Encoding` | Instructs the server which compression models (e.g., `gzip`) the client can natively decode. |
| **Request** | `Cookie` | Forwards stored state tracking variables and authentication tokens back to the web server interface. |
| **Response** | `Set-Cookie` | Transmits unique session state strings from the server to be written directly into the client's local browser memory. |
| **Response** | `Content-Type` | Identifies the MIME structure of the payload data (`text/html`, `image/jpeg`, `application/pdf`) so the browser knows how to process it. |
| **Response** | `Content-Length` | Declares the precise size of the returning data block in bytes to verify no truncation or transmission drops occurred. |

---

## 📊 HTTP Status Code Response Brackets

Status codes are grouped into five explicit mathematical ranges detailing transaction outcomes:

*   **`100-199` — Informational:** Informs the client that the initial part of the request was accepted and transmission should continue.
*   **`200-299` — Success:** Confirms the intended request action executed cleanly.
    *   *`200 OK`:* Request successfully fulfilled.
    *   *`201 Created`:* A new database record or object was generated successfully.
*   **`300-399` — Redirection:** Forces the client browser agent to pivot to a separate URL location.
    *   *`301 Moved Permanently`:* Permanent route change; instructs search engines to map all SEO value to the new URL destination.
    *   *`302 Found`:* A temporary redirection loop; path configuration may change in the future.
*   **`400-499` — Client Errors:** Indicates a structural issue caused directly by the requesting client asset.
    *   *`400 Bad Request`:* Syntax error; missing required parameters.
    *   *`401 Unauthorized`:* Authentication required; user must log in.
    *   *`403 Forbidden`:* Access denied regardless of login rights.
    *   *`404 Page Not Found`:* The targeted resource path does not exist on the file system.
    *   *`405 Method Not Allowed`:* The path rejects the submitted verb (e.g., executing a `GET` on a registration portal expecting a `POST`).
*   **`500-599` — Server Errors:** Confirms the client request was valid, but a major internal crash or service failure occurred on the server side.
    *   *`500 Internal Server Error`:* Generic backend code crash.
    *   *`503 Service Unavailable`:* The host system is offline for maintenance or completely overwhelmed by connection traffic.

---

## 🍪 State Persistence & Session Management via Cookies

Because raw HTTP is entirely stateless, servers rely on **Cookies** to maintain persistent interactions. Cookies are structured key-value strings stored directly in a client's local browser storage.

1.  **Issuance:** When a user logs in, the server generates a cryptographically complex token and sends it back via a `Set-Cookie: session_id=XYZ123` header.
2.  **Ingestion:** The browser intercepts this token and securely pins it to the domain space.
3.  **Transmission:** On every subsequent click or asset fetch, the browser automatically injects the `Cookie: session_id=XYZ123` string into the outbound request header block. This tells the server backend who the user is without forcing them to re-authenticate on every page.

---

## 🚩 Practical Lab Evidence & Captures

### 🧪 API & Protocol Interaction Triage
Utilized the built-in HTTP request simulator console to construct custom protocol handshakes and capture administrative outputs:

```bash
# 1. Standard Room Page Fetch (GET Line Capture)
# Request Verb: GET | Path: /room
# Server Status Output Response: 200 OK

# 2. Target Blog Route Query
# Request Verb: GET | Path: /blog?id=1
# Telemetry Captured: Parameterized query stream executed clean.

# 3. Targeted User Destruction Request
# Request Verb: DELETE | Path: /user/1
# Server Status Output Response: 200 OK

# 4. Host Parameter Modification
# Request Verb: PUT | Path: /user/2 | Parameters: username=admin
# Server Status Output Response: 200 OK

# 5. Volatile Authentication Request Simulation
# Request Verb: POST | Path: /login | Parameters: username=thm & password=letmein
# Session Identity Capture Output: Valid Token Generated.
```
