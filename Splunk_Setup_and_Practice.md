# Lab Report: Monitoring Brute-Force Attacks with Splunk

**Target:** Windows Server 2022  
**Attacker:** Kali Linux  
**SIEM:** Splunk Enterprise  

---

## 1. Introduction
This report outlines the technical implementation of a homelab environment used to detect credential-based attacks. The objective was to capture **Event ID 4625 (Failed Login)** generated during a brute-force attack and analyze the data within Splunk.

---

## 2. Phase 1: Baseline & Attack Simulation

The Windows Server 2022 instance was configured to log all authentication attempts. Initial verification was performed using the local Event Viewer to establish a clean baseline before attack execution.

### 2.1 Local Log Monitoring
Initially, Event Viewer was used to monitor standard system and authentication events prior to any malicious activity.
![alt text](<Screenshots/Screenshot (1).png>)


---

### 2.2 Brute-Force Execution
A high-frequency brute-force attack was launched from Kali Linux, generating a large volume of authentication attempts against the Windows Server target.

![alt text](<Screenshots/Screenshot (2).png>)

---

## 3. Phase 2: Log Analysis & Detection

After executing the attack, analysis focused on identifying forensic evidence within the Windows logs.
![alt text](<Screenshots/Screenshot (3).png>)

### 3.1 Event Identification
The Windows Security logs recorded multiple **"Audit Failure"** events (Event ID 4625). These entries included critical details such as:
- Source IP address (attacker machine)
- Attempted usernames
- Logon types and failure reasons

![alt text](<Screenshots/Screenshot (5).png>)

---

### 3.2 Splunk Ingestion
Using the **Splunk Universal Forwarder**, Windows Security logs were forwarded and indexed in Splunk. The logs were successfully ingested and made searchable within the Splunk interface.

![alt text](<Screenshots/Screenshot (6).png>)

---

## 4. Phase 3: SPL & Data Visualization

To efficiently analyze thousands of failed login attempts, **Search Processing Language (SPL)** was used to aggregate and interpret the data.
![alt text](<Screenshots/Screenshot (7).png>)

### 4.1 Practicing SPL Queries
Custom SPL queries were developed to:
- Count failed login attempts per user
- Identify the attacker's IP address
- Detect patterns and anomalies in authentication behavior

![alt text](<Screenshots/Screenshot (8).png>)
![alt text](<Screenshots/Screenshot (9).png>)
---

### 4.2 Security Dashboards
Dashboards were created in Splunk to visualize attack patterns and trends, providing a centralized view ("single pane of glass") for monitoring security events.

![alt text](<Screenshots/Screenshot (10).png>)
![alt text](<Screenshots/Screenshot (11).png>)

---

## 5. Key Findings

### Detection
Windows Server 2022 successfully logged every failed login attempt, providing a complete audit trail of the brute-force activity.

### Analysis
Splunk enabled rapid correlation and aggregation of log data, making it significantly easier to analyze large volumes of events compared to manual inspection via Event Viewer.

### Mitigation
Based on the observed attack patterns, the following mitigation steps are recommended:
- Implement **Account Lockout Policies**
- Block or restrict the attacker's IP address
- Enable **multi-factor authentication (MFA)**
- Monitor for repeated authentication failures in real-time

---

## 6. Conclusion
This lab demonstrates the effectiveness of combining Windows logging with Splunk SIEM for detecting and analyzing brute-force attacks. Proper log ingestion, correlation, and visualization are critical components of modern security monitoring and incident response.

---