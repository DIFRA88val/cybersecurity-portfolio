# Windows Event Log Analysis & Key Event ID Tracking

## Objective
Identify malicious behavior, privilege escalation attempts, and account compromises by monitoring core Windows Security Event logs and specific Event IDs.

---

## Part 1: Windows Log Categories
When conducting incident triage on a Windows endpoint, defensive analysts focus on three primary event logs:
* **Application Logs:** Records events logged by programs or system applications.
* **System Logs:** Records events logged by Windows system components (e.g., driver failures, boot events).
* **Security Logs:** Records authentication events, privilege usage, and audit policy changes.

---

## Part 2: Critical Event IDs for Incident Response

| Event ID | Log Trigger | Security Focus Area |
| :--- | :--- | :--- |
| **4624** | Successful Account Logon | Baseline activity monitoring and lateral movement detection. |
| **4625** | Failed Account Logon | Key indicator of brute-force or credential stuffing attacks. |
| **4720** | A User Account Was Created | Monitoring for rogue persistence mechanisms by attackers. |
| **4672** | Special Privileges Assigned to New Logon | Investigating potential privilege escalation to administrative status. |
| **1102** | The Audit Log Was Cleared | High-alert indicator of an attacker attempting to cover their tracks. |
