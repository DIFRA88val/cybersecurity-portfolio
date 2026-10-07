# 🌐 Introduction to Web Development & Client-Side Flaws

**Module:** SOC Foundations / Network Basics  
**Room Source:** TryHackMe — How Websites Work & Web Vulnerabilities  
**Objective:** Dissect client-side core components (HTML/JavaScript), analyze DOM structures, trace source code discovery workflows, and audit client-side flaw mechanics (Sensitive Data Exposure and HTML Injection).

---

## 🏗️ The Multi-Tier Web Architecture

Websites are delivered over a multi-tiered architecture split cleanly into two distinct operational execution layers:

*   **The Front End (Client-Side):** Code compiled and rendered directly inside the user's local web browser engine (e.g., Google Chrome, Safari, Firefox). This tier manages layout presentation and local user interactivity.
*   **The Back End (Server-Side):** A remote dedicated computer system that listens for client queries, executes database lookups, processes application logic, and returns formatted data payloads.

---

## 🎨 Client-Side Building Blocks: HTML & Interactive JavaScript

Frontend applications rely on structural markup combined with active execution scripts to deliver user experiences:

### 1. HTML5 (HyperText Markup Language) Document Hierarchy
HTML serves as the permanent structural blueprint of a webpage. Browser components parse sequential tags to establish structural nodes:
*   `<!DOCTYPE html>`: Defines the standard document type profile, forcing modern cross-browser standard uniformity via HTML5.
*   `<html>`: The global root boundary tag encapsulating all website asset nodes.
*   `<head>`: Contains metadata parameters, page titles, and pointers to external styling or script sheets.
*   `<body>`: Houses the true operational payload; only elements enclosed within this tag block are structurally rendered visible on the interface canvas.

#### 🏷️ Attribute Profiles: Class vs. ID Parameters
HTML tags use specific attribute parameters to assign styling or execution targets:
*   **The `class` Attribute:** A multi-tenant indicator used to apply standardized styling or alignment constraints to an infinite number of identical tags concurrently (e.g., `<p class="bold-text">`).
*   **The `id` Attribute:** A **strictly unique** local identifier. No two elements within the same active document can share an identical ID name (e.g., `<p id="demo">`). IDs are utilized by scripting engines to isolate and change distinct targeted nodes.

### 2. JavaScript (Dynamic Execution Engine)
JavaScript is a flexible, highly popular client-side scripting language that gives functionality to static document frameworks. It modifies the page structure dynamically in real time without forcing an entire page reload.
*   **Integration:** Injected inline inside `<script>` block tags or referenced remotely using source link wrappers: `<script src="/js/app.js"></script>`.
*   **DOM Manipulation Example:** The logic string `document.getElementById("demo").innerHTML = "Hack the Planet";` actively crawls the page layout, captures the element matching ID `demo`, and overrides its text string instantly.
*   **Event Handling:** HTML elements use event hooks to listen for real-world user interactions:
    *   `onclick`: Fires scripts when an item is clicked (e.g., `<button onclick="...">Click Me</button>`).
    *   `onhover`: Fires scripts when a pointer shifts across the boundary perimeter.

---

## 🔬 Client-Side Security Vectors & Flaw Analysis

When performing an application assessment, security analysts must comprehensively map frontend entry fields and hidden configuration parameters.

### 🔓 1. Sensitive Data Exposure & Source Disclosure
Sensitive Data Exposure occurs when a development team fails to properly prune or protect confidential clear-text indicators before shipping the production frontend build.

*   **Mechanism:** Because browsers must download the absolute text source of HTML and JavaScript files to map the interface locally, *any user* can instantly view the entire file configuration. An analyst can exploit this simply by executing a **"View Page Source"** triage action.
*   **Exploitation Potential:** Developers frequently leave temporary administrative credentials, development API master keys, hidden server pathways, or backend architectural details embedded inside standard HTML comments (`<!-- comment -->`) or debug arrays. Attackers parse these strings to harvest valid credentials and escalate privileges against hidden administrative assets.

### 💉 2. HTML Injection (Client-Side Input Reflected Flaw)
HTML Injection is a dangerous vulnerability that occurs when an application receives user-supplied data and prints it directly back onto the webpage interface without validating or stripping malicious code signatures.

```text
[Attacker Input: <h1>Hacked</h1>] ──> [Vulnerable Field] ──> [App appends input raw into sayHi()]
                                                                            │
                                                                            ▼
[Browser processes <h1> tag as real structure] <── [Reflects raw back onto page canvas]
```

*   **The Flaw Mechanism:** If a text form field takes input data and pushes it straight to a real-time output container (like a `sayHi` javascript loop) without checking its format, the browser cannot differentiate between legitimate server-delivered HTML code and rogue text input.
*   **Exploitation Profile:** An attacker inserts complete malicious HTML tag layouts (e.g., injecting an `<h1>` header, an external link reference, or an automated image load trigger) into the comment box. The browser parses the input as functional layout instructions, altering the physical structure, look, and operational behavior of the site. This vector is frequently weaponized to run phishing loops or deface target websites.

---

## 🛡️ Core Defensive Remediation Principle

*   **Never Trust User Input:** To properly eradicate injection flaws, developers must enforce strict **Input Sanitization** frameworks. 
*   **Remediation Action:** All input telemetry accepted from web forms must pass through robust sanitization functions that actively strip out structural symbols (such as `<` and `>`), encoding special markers into safe text symbols before they are allowed to touch active execution scripts or database loops.
