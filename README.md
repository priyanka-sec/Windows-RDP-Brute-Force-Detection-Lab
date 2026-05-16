<h1 align="center">🛡️ Windows RDP Brute Force Detection & Log Analysis Lab </h1>

<h3 align="center">
RDP Brute Force Attack Simulation, Windows Event Log Analysis & MITRE ATT&CK Mapping
</h3>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-VirtualBox-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/Attacker-Kali%20Linux-red?style=flat-square"/>
  <img src="https://img.shields.io/badge/Victim-Windows%20Server%202022-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/Status-Completed-green?style=flat-square"/>
</p>

<br><br>

## 📑 Table of Contents

- [Project Overview](#-project-overview)
- [Objectives](#-objectives)
- [Lab Architecture](#️-lab-architecture)
- [Architecture Diagram](#️-architecture-diagram)
- [Tools & Technologies Used](#️-tools--technologies-used)
- [Attack Simulation Workflow](#️-attack-simulation-workflow)
- [Windows Event Log Analysis](#-windows-event-log-analysis)
- [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
- [Incident Timeline](#-incident-timeline)
- [Mitigation Recommendations](#️-mitigation-recommendations)
- [Lessons Learned](#-lessons-learned)
- [About the Analyst](#-about-the-analyst)

<br><br>

## 📌 Project Overview

This project simulates a controlled Remote Desktop Protocol (RDP) brute-force attack against a Windows Server 2022 environment to demonstrate how Security Operations Center (SOC) analysts identify, investigate, and respond to suspicious authentication activity.

The lab focuses on analyzing Windows Security Event Logs generated during repeated RDP login attempts and understanding how attackers target exposed remote access services. The project also demonstrates how Event Viewer can be used to monitor authentication behavior, investigate failed login patterns, and support incident response activities.

The environment was built using VirtualBox with Kali Linux acting as the attacker machine and Windows Server 2022 serving as the target system. Controlled attack simulations were performed using Hydra to generate authentication events for analysis and defensive monitoring.

This project highlights key SOC concepts including:

- Windows authentication log analysis
- RDP brute-force attack detection
- Event ID investigation
- Incident timeline analysis
- MITRE ATT&CK technique mapping
- Security hardening and mitigation strategies
- SOC monitoring and incident response workflows

<br><br>

## 🎯 Objectives

- Simulate controlled RDP brute-force attacks in a virtual lab environment
- Analyze Windows Security Event Logs related to authentication activity
- Investigate Event IDs associated with successful and failed login attempts
- Understand how SOC analysts identify suspicious authentication behavior
- Perform attack timeline analysis using Windows Event Viewer
- Apply MITRE ATT&CK framework mapping to RDP attack techniques
- Learn defensive security practices for protecting exposed RDP services
- Implement mitigation and hardening recommendations to reduce attack surface
- Develop hands-on experience with SOC investigation and incident response concepts

<br><br>

## 🏗️ Lab Architecture

The lab environment was designed in a controlled virtualized setup to simulate real-world RDP brute-force attack scenarios and authentication log analysis workflows commonly investigated by Security Operations Center (SOC) analysts.

### Environment Configuration

| Component | Description |
|-----------|-------------|
| Attacker Machine | Kali Linux |
| Victim Machine | Windows Server 2022 |
| Virtualization Platform | VirtualBox |
| Attack Tool | Hydra |
| Monitoring Tool | Windows Event Viewer |
| Protocol Targeted | Remote Desktop Protocol (RDP) |

### Lab Workflow

1. Kali Linux was configured as the attacker machine.
2. Windows Server 2022 was configured with Remote Desktop Protocol (RDP) enabled.
3. Controlled brute-force login attempts were performed using Hydra.
4. Windows Security Event Logs recorded authentication activity.
5. Event Viewer was used to investigate failed and successful login attempts.
6. Attack behavior, Event IDs, and mitigation strategies were analyzed as part of the SOC investigation workflow.

<br><br>

## 🖼️ Architecture Diagram

<p align="center">
  <img src="Diagrams/architecture-diagram.png" width="850"/>
</p>

<br><br>

<br><br>

## 🛠️ Tools & Technologies Used

| Tool / Technology | Purpose |
|-------------------|---------|
| Kali Linux | Attacker machine used to simulate brute-force attacks |
| Windows Server 2022 | Victim machine targeted during the attack simulation |
| Hydra | Password brute-forcing tool used for RDP attack attempts |
| Windows Event Viewer | Monitoring and analysis of authentication logs |
| VirtualBox | Virtualized lab environment setup |
| RDP Protocol | Remote access protocol targeted during the attack |

### Technical Skills Demonstrated

- Windows Event Log Analysis
- RDP Attack Detection
- Authentication Monitoring
- Event ID Investigation
- MITRE ATT&CK Mapping
- SOC Investigation Workflow
- Security Hardening Techniques

<br><br>

## 🔐 Key Security Concepts Covered

- RDP Brute Force Attacks
- Authentication Monitoring
- Windows Security Event Analysis
- Event ID Investigation
- Credential Access Techniques
- Incident Timeline Analysis
- Remote Access Security
- MITRE ATT&CK Framework
- SOC Investigation Workflow
- Security Hardening

<br><br>

## ⚔️ Attack Simulation Workflow

### Step 1 — Environment Preparation

A virtual lab environment was created using VirtualBox. Kali Linux was configured as the attacker machine, while Windows Server 2022 was configured as the target system with Remote Desktop Protocol (RDP) enabled.

### Step 2 — RDP Enumeration

The target Windows machine IP address was identified, and RDP connectivity was verified before initiating the attack simulation.

### Step 3 — Brute-Force Attack Simulation

Hydra was used to perform controlled RDP brute-force login attempts against the Windows Server 2022 machine in order to generate authentication-related security events.

### Step 4 — Authentication Log Generation

Repeated login attempts generated Windows Security Event Logs associated with successful and failed authentication activities.

### Step 5 — Security Log Investigation

Windows Event Viewer was used to analyze Event IDs related to authentication behavior and identify suspicious login patterns commonly associated with brute-force attacks.

### Step 6 — Defensive Analysis

The generated logs, attack timeline, and authentication events were reviewed to understand attack behavior and evaluate defensive monitoring techniques used in SOC environments.

<br><br>

## 📊 Windows Event Log Analysis

### Important Windows Event IDs

| Event ID | Description |
|----------|-------------|
| 4625 | Failed Login Attempt |
| 4624 | Successful Login |
| 4776 | NTLM Authentication |
| 4672 | Special Privileges Assigned |

### Event Analysis

During the attack simulation, Windows Security Event Logs captured multiple failed authentication attempts associated with the RDP brute-force activity. Event ID 4625 was repeatedly generated as invalid login attempts were performed against the target system.

The authentication logs provided visibility into:

- Failed login attempts
- Authentication patterns
- Login timestamps
- Source system activity
- Account targeting behavior

These logs are commonly investigated by SOC analysts to detect suspicious authentication activity and identify potential brute-force attacks targeting exposed remote access services.

<br><br>

## 🧠 MITRE ATT&CK Mapping

| Technique | MITRE ATT&CK ID | Description |
|-----------|-----------------|-------------|
| Brute Force | T1110 | Repeated login attempts using multiple password combinations |
| Remote Services | T1021.001 | Abuse of Remote Desktop Protocol (RDP) for remote access |
| Valid Accounts | T1078 | Attempted use of legitimate credentials for unauthorized access |

### MITRE Analysis

The simulated attack behavior aligns with credential access and remote access techniques documented in the MITRE ATT&CK framework. These techniques are commonly associated with brute-force attacks targeting exposed RDP services in enterprise environments.

<br><br>

## 🕒 Incident Timeline

| Time | Activity |
|------|-----------|
| 10:01 | Kali Linux attacker machine initialized |
| 10:02 | RDP service enumeration performed |
| 10:03 | Hydra brute-force attack initiated |
| 10:04 | Multiple failed login attempts generated |
| 10:05 | Windows Event ID 4625 logs recorded |
| 10:06 | Authentication log analysis performed |
| 10:08 | Suspicious authentication activity confirmed |
| 10:10 | Mitigation and hardening review conducted |

### Timeline Analysis

The incident timeline demonstrates how repeated failed login attempts can rapidly generate authentication events within Windows Security Logs. Timeline analysis is an important SOC investigation technique used to reconstruct attacker activity and identify suspicious behavior patterns.

<br><br>

## 🛡️ Mitigation Recommendations

### Recommended Security Controls

- Enable Multi-Factor Authentication (MFA) for remote access services
- Restrict RDP access to VPN users only
- Implement account lockout policies
- Enforce strong password requirements
- Disable unnecessary remote access exposure
- Limit public-facing RDP services
- Monitor Windows authentication logs regularly
- Apply firewall restrictions for remote access ports

### Security Impact

Implementing these controls significantly reduces the risk of brute-force attacks targeting exposed Windows RDP services and improves defensive monitoring capabilities within enterprise environments.

<br><br>

## 📚 Lessons Learned

- Importance of monitoring Windows authentication logs
- Risks associated with publicly exposed RDP services
- Value of Event ID analysis during incident investigations
- Importance of proactive security monitoring and hardening
- Benefits of implementing MFA and account lockout protections
- Understanding how brute-force attacks generate authentication patterns within Windows Security Logs

### Project Outcome

This project provided hands-on experience with Windows authentication monitoring, RDP attack detection concepts, and SOC investigation workflows commonly used in enterprise cybersecurity environments.

<br><br>

## 👩‍💻 About the Analyst

**Priyanka Rane**
SOC Analyst L1 | Threat Detection & Incident Response

📧 ranepriyanka567@gmail.com

🔗 [LinkedIn](https://www.linkedin.com/in/priyanka-rane-606a71257/)

## ⭐ If you found this project useful, feel free to star the repository.
