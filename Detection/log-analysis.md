# 🔍 Detection & Log Analysis (SOC Perspective)

This section explains how the simulated RDP attack was detected using Windows Security Logs and basic SOC investigation techniques.

The focus is on identifying malicious patterns through log monitoring and event correlation.

<br><br>

## 🎯 Detection Objective

- Identify unauthorized login attempts
- Detect brute-force indicators
- Analyze authentication logs
- Correlate failed and successful logins
- Reconstruct attacker behavior from logs

<br><br>





## 🖥️ Log Source

Primary log source used for detection:

- Windows Security Event Logs
- Tool Used: Event Viewer
- Log Path:
Event Viewer → Windows Logs → Security

These logs contain authentication activity, including both successful and failed login attempts.

<br><br>




## 🚨 Key Security Events Analyzed
## ❌ Event ID 4625 — Failed Logon Attempts

This event indicates unsuccessful authentication attempts.

Observed Characteristics in Lab:

- Multiple failed login entries recorded
- Repeated attempts from the same source IP
- Logon Type observed: Type 3 (Network Logon)
- High frequency of authentication failures

📌 Detection Insight:
Repeated 4625 events from a single source are strong indicators of brute-force or password spraying attacks.

<br><br>





## ✅ Event ID 4624 — Successful Logon

This event indicates a successful login.

Observed Indicators:

- Appeared after multiple failed attempts
- Logon Type associated with remote access activity
- New Logon Session ID generated
- Source network address visible

📌 SOC Insight:
A successful login immediately after failed attempts is a high-confidence compromise indicator.

<br><br>



## 🔎 Log Analysis Methodology

The detection process followed a basic SOC investigation workflow:

### 1️. Filtering Security Logs

- Opened Security logs in Event Viewer
- Filtered using Event IDs:
   - 4625 (Failed Logon)
   - 4624 (Successful Logon)

### 2️. Pattern Identification

- Identified clusters of failed logins
- Observed repeated timestamps within short intervals
- Noticed same source IP across events

This indicated automated attack behavior.

### 3️. Event Correlation

Correlated:

- Burst of failed login attempts
- Followed by successful authentication
- Matching source IP across logs

This confirmed a simulated brute-force compromise.

### 4️. Log Attribute Examination

- Key fields analyzed:
- Account Name
- Logon Type
- Source Network Address
- Timestamp
- Logon ID

These fields help reconstruct attacker activity.

<br><br>




## 🧠 Detection Challenges

During analysis, some real-world nuances were observed:

- Failed logins showed Logon Type 3 instead of 10
- Indicates authentication attempts at the network level
- Demonstrates how log interpretation varies by environment

📌 Learning:
SOC analysts must interpret logs based on context, not assumptions.

<br><br>





## 📊 Indicators of Compromise (IOCs)

The following indicators confirmed malicious activity:

- Repeated failed authentication events
- Same source IP across attempts
- Rapid login attempt frequency
- Successful login after failures
- Suspicious authentication pattern

<br><br>





## 🛡️ SOC Detection Takeaways

- Authentication logs are critical for intrusion detection
- Event correlation is key in identifying attacks
- Even basic attacks leave clear log evidence
- Log interpretation requires contextual awareness

<br><br>





## 🧪 Real-World Relevance

In enterprise environments, similar detection is performed using:

- SIEM platforms (Splunk, Sentinel, QRadar)
- Automated alerting rules
- Brute-force detection policies

This lab simulates that process manually using native Windows logs.

<br><br>





## 🏁 Detection Summary

The RDP attack was successfully detected by analyzing Windows Security Logs and correlating authentication events.

By identifying repeated failed logins followed by a successful authentication, the simulated compromise was confirmed.

This demonstrates how foundational log analysis skills form the backbone of real-world SOC investigations.
