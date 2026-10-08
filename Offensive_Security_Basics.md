# Lab Report: Introduction to Offensive Security

## 1. Reconnaissance & Directory Enumeration
To automate the discovery of hidden assets and sensitive paths on the target web server (`http://onlineshop.thm`), I ran a directory fuzzing scan using **Gobuster** with a standard directory wordlist.

```bash
gobuster dir --url http://onlineshop.thm -w /usr/share/wordlists/dirbuster/directory-list.txt
```

### Discovered Endpoints:
*   `/sitemap` (Status: 200) — Standard site structural map.
*   `/secret-panel` (Status: 200) — **Critical Finding:** Exposed administrative interface bypass.

## 2. Exploitation & Flag Retrieval
Navigating to the unauthorized `/secret-panel` page revealed internal configuration settings. By chaining the information disclosure vulnerability with weak permission models, I successfully intercepted the system flag:

*   **Flag Retrieved:** `THM{M0V1NG_D33P3R_1NT0_0FF3NS1V3}`
## 3. Credential Brute-Forcing & Weakness Chaining
Discovering the `/login` endpoint provided an initial vector. To simulate a real-world adversary looking for valid authentication tokens, I executed a dictionary attack targeting the default `admin` username.

### Manual Verification
Testing a foundational credential wordlist manually against the login mechanism showed a lack of rate-limiting controls.

### Brute-Force Automation via Hydra
To scale the discovery phase, I utilized **Hydra** to systematically submit automated HTTP POST form requests against the target parameters using an external credential list (`passlist.txt`):

```bash
hydra -l admin -P passlist.txt www.onlineshop.thm http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V
```

### Attack Summary:
*   **Target URL:** `http://onlineshop.thm`
*   **Identified Account:** `admin`
*   **Cracked Password:** `password`
*   **Failed Submissions Before Success:** 2

## 4. Administrative Session Access & Impact
Authenticated access to the restricted parameters of the enterprise portal confirmed the full collapse of the security posture due to chained vulnerabilities:
1. **Broken Object Level Access / Information Disclosure:** Finding unlinked administrative pages (`/secret-panel`, `/login`).
2. **Weak Credential Hygiene:** Leveraging default/common credentials to hijack high-privilege sessions.

*   **Retrieved Compromise Token:** `THM{CH41N1NG_W34KN35535_F0R_1MP4CT}`
