# Linux CLI & SQL Log Filtering Assessment

## Objective
Demonstrate practical proficiency using the Linux Command Line Interface (CLI) and SQL queries to filter, parse, and isolate security event logs for threat analysis.

---

## Part 1: Linux CLI Log Analysis

### Core Concept: Log Focus Areas
When triaging a Linux system, defense analysts focus heavily on the `/var/log/` directory to track authentication attempts and system status.

### Practical Commands & Triggers
To parse multi-gigabyte logs without consuming excessive memory, the following lightweight string-filtering commands are utilized:

* **Isolate Failed Authentication Attempts:**
  ```bash
  grep "Failed password" /var/log/auth.log
  ```
* **Extract and Count Unique Attacking IPs:**
  ```bash
  grep "Failed password" /var/log/auth.log | awk '{print \$11}' | sort | uniq -c | sort -nr
  ```
* **Monitor Live Authentication Log Additions:**
  ```bash
  tail -f /var/log/auth.log | grep --line-buffered "ACCEPTED"
  ```

---

## Part 2: SQL Security Data Filtering

### Objective
Query database tables containing user access logs, system permissions, and asset inventories to locate security anomalies.

### Critical Queries
* **Identify Suspicious Off-Hours Logins:**
  ```sql
  SELECT username, login_time, ip_address 
  FROM user_logs 
  WHERE login_time BETWEEN '00:00:00' AND '05:00:00';
  ```
* **Audit High-Privilege Account Access:**
  ```sql
  SELECT * 
  FROM employee_access 
  WHERE role = 'Administrator' OR privileges = 'Root';
  ```
