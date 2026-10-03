# NIST SP 800-30 Risk Assessment & Framework Implementation

## Objective
Establish a structured approach to identifying, analyzing, and mitigating organizational security risks using the NIST SP 800-30 framework, quantitative risk calculations, and threat modeling protocols.

---

## Part 1: Quantitative Risk Calculation Matrix

To justify security spending and prioritize defensive assets, quantitative calculations are executed to determine the financial impact of potential vulnerabilities.

### Single Loss Expectancy (SLE)
Calculates the monetary loss of a single security incident.
* **Formula:** `Asset Value (AV) x Exposure Factor (EF) = SLE`

### Annualized Rate of Occurrence (ARO)
The estimated number of times an incident is expected to occur within a single year.

### Annualized Loss Expectancy (ALE)
The total projected annual financial loss for a specific risk.
* **Formula:** `SLE x ARO = ALE`

---

## Part 2: NIST SP 800-30 Risk Handling Strategies

Once risks are calculated and analyzed, one of four primary risk handling strategies must be applied based on organizational risk tolerance:

* **Risk Mitigation:** Implementing technical or administrative safeguards (e.g., deploying an EDR agent, enforcing Multi-Factor Authentication) to reduce the vulnerability.
* **Risk Transference:** Shifting the financial burden of the risk to a third party (e.g., purchasing cyber insurance or outsourcing operations to an MSSP).
* **Risk Avoidance:** Eliminating the risk entirely by stopping the associated activity or decommissioning the vulnerable asset (e.g., shutting down a legacy server).
* **Risk Acceptance:** Acknowledging the risk exists without taking active measures, typically because the cost of mitigation exceeds the value of the asset.

---

## Part 3: Security Audit & Testing Workflow

To ensure risk controls remain effective, the following auditing pipeline is maintained:
1. **Identify Threats & Vulnerabilities:** Map assets against known threat vectors.
2. **Execute Controlled Vulnerability Scans:** Use automated tools to locate misconfigurations.
3. **Analyze Controls:** Evaluate if current firewalls and access lists are successfully lowering the ARO.
