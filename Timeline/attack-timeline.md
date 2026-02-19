# 🕒 RDP Attack Timeline (SOC Investigation Style)

This section documents the chronological timeline of the simulated RDP attack performed in a controlled lab environment.

The purpose of this timeline is to demonstrate how a Security Operations Center (SOC) analyst reconstructs an incident using log data and system observations.

<br><br>

## 🎯 Objective

- Track attacker activity step-by-step
- Correlate failed and successful authentication attempts
- Identify patterns in RDP access attempts
- Simulate real-world SOC investigation methodology

<br><br>

## 🖥️ Lab Environment Context

- Attacker Machine: Kali Linux  
- Victim Machine: Windows Server 2022  
- Attack Vector: Remote Desktop Protocol (RDP)  
- Log Source: Windows Security Event Logs  

<br><br>

## ⏱️ Attack Timeline

### 🟡 Phase 1 — Environment Preparation

**Time:** T0

- Windows Server 2022 deployed in a virtual environment
- Remote Desktop Protocol (RDP) enabled via PowerShell
- Firewall rule configured to allow RDP connections
- Event Viewer opened to monitor Security Logs

📌 Analyst Note:  
This phase ensures proper logging visibility before simulating attacker activity.

<br>

### 🔍 Phase 2 — Initial Reconnaissance

**Time:** T1

- Attacker identified the target system’s IP address
- Network connectivity verified between attacker and victim machines
- RDP port (3389) confirmed accessible

📌 SOC Insight:  
In real environments, this stage often includes scanning activity.

<br>

### 🚨 Phase 3 — Failed Login Attempts (Brute-Force Simulation)

**Time:** T2

- Multiple RDP login attempts performed using incorrect credentials
- Windows Security Logs generated **Event ID 4625 (Failed Logon)** entries
- Observed Logon Type: **Type 3 (Network Logon)**

📊 Observations:
- Repeated authentication failures
- Same source IP appearing in logs
- High frequency of failed attempts

<br>

📌 Technical Insight:  
Although the attack targeted RDP, failed attempts were logged as **Logon Type 3**, which represents network-based authentication attempts before a full RDP session is established.

📌 SOC Detection Tip:  
Repeated 4625 events from a single source IP are strong brute-force indicators.

<br>

### 🔐 Phase 4 — Successful Authentication

**Time:** T3

- Valid credentials used by the attacker
- Successful RDP login achieved
- Security Log recorded **Event ID 4624 (Successful Logon)**

Key Indicators:
- Logon Type 10 confirmed Remote Interactive session
- Source network address visible in logs
- New logon session ID generated

📌 SOC Insight:  
A 4624 event following multiple 4625 failures is a strong compromise indicator.

<br>

### 🧠 Phase 5 — Log Analysis & Correlation

**Time:** T4

- Event Viewer used to analyze authentication patterns
- Timeline reconstructed using timestamps
- Correlated:
  - Failed login bursts (4625)
  - Successful login entry (4624)
  - Source IP consistency

📌 Analyst Thinking:
This phase represents core SOC work — connecting isolated log events into a coherent attack narrative.

<br>

### 🛡️ Phase 6 — Defensive Actions

**Time:** T5

Mitigation steps applied:

- RDP access disabled via registry modification
- Firewall rules reviewed
- Suspicious login patterns documented

📌 SOC Best Practice:
Immediate containment after detecting unauthorized access is critical.

<br><br>

## 📊 Key Findings

- Multiple failed authentication attempts detected (Event ID 4625, Logon Type 3)
- Successful RDP compromise identified (Event ID 4624, Logon Type 10)
- Same source IP observed across events
- Clear brute-force pattern identified

<br><br>

## 🧠 Lessons from Timeline Analysis

- Authentication logs are critical forensic evidence
- Logon types provide valuable attack context
- Event correlation is essential for detection
- Even simple attacks leave strong log trails
- Timeline reconstruction is a core SOC analyst skill

<br><br>

## 🏁 Conclusion

This timeline demonstrates how attackers move from reconnaissance to access and how defenders can reconstruct the entire attack using Windows Security Logs.

By analyzing event sequences and login patterns, defenders can detect intrusions early and take rapid mitigation actions.

This lab highlights the importance of proactive monitoring, log analysis, and structured incident investigation in modern cybersecurity operations.
