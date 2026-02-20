# 📸 Screenshots – Windows RDP Attack Lab

This section contains visual evidence of the simulated RDP attack performed in a controlled lab environment.

The screenshots are organized into three phases of the incident lifecycle:

- 🧨 Attack Execution  
- 🔍 Detection & Log Analysis  
- 🛡️ Mitigation & Hardening  

Each phase has its own documentation file describing the captured evidence.

<br><br>



## 📂 Folder Structure

| Phase | Description | Documentation |
|------|------------|--------------|
| 🧨 Attack Phase | Screenshots of attacker activity from Kali Linux | [Attack Phase Documentation](attack-phase.md) |
| 🔍 Detection Phase | Event Viewer logs and authentication analysis | [Attack Phase Documentation](detection-phase.md) |
| 🛡️ Mitigation Phase | Defensive actions and system hardening | [Attack Phase Documentation](mitigation-phase.md) |

<br><br>




## 🧨 Attack Phase

Demonstrates how the attacker attempted unauthorized access via RDP.

Includes:

- RDP connection attempts
- Brute-force login trials
- Attacker source visibility

📄 See: [Attack Phase Documentation](attack-phase.md)

<br><br>




## 🔍 Detection Phase

Shows how suspicious activity was detected using Windows Security Logs.

Includes:

- Event ID 4625 (Failed Logons)
- Event ID 4624 (Successful Logons)
- Logon type and timestamp correlation

📄 See: [Attack Phase Documentation](detection-phase.md)

<br><br>





## 🛡️ Mitigation Phase

Documents the defensive response after detecting malicious activity.

Includes:

- PowerShell mitigation commands
- Registry configuration changes
- Post-hardening system state

📄 See: [Attack Phase Documentation](mitigation-phase.md)

<br><br>





## 🔒 Privacy Notice

All screenshots were captured in an isolated lab environment.

Sensitive information has been redacted for safe public sharing:

- IP addresses masked
- Hostnames sanitized
- Usernames anonymized

No real-world infrastructure was involved.

<br><br>





## 🎯 Purpose of This Section

This section provides visual validation of:

- Hands-on attack simulation  
- Log-based detection techniques  
- SOC-style investigation workflow  
- Practical mitigation steps  

These screenshots support the technical analysis documented across the project.

<br><br>




## 🏁 Summary

The Screenshots section transforms this project from theory into a practical SOC-style case study by providing visual proof of:

1. Attack execution  
2. Detection methodology  
3. Incident response actions  

This reinforces the hands-on nature of the Windows RDP Attack Lab.
