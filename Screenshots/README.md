# 🖼️ Screenshots — Investigation Evidence
## Windows RDP Brute Force Detection & SIEM Investigation Lab

<br><br>

## 📌 Purpose

This folder contains 27 screenshots documenting complete
evidence for every step of the RDP brute force investigation —
from lab setup through attack simulation, log analysis,
Splunk SIEM detection, and Sysmon telemetry.

Every claim in the incident report is backed by a
screenshot in this folder.

<br><br>

## 📁 Complete Screenshot Index

### 🔧 Lab Setup Evidence (01–09)

| # | File | Description |
|---|---|---|
| 01 | [01-lab-setup.png](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/01-lab-setup.png) | VirtualBox lab environment — both VMs running |
| 02 | [02-kali-to-windows-connectivity.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/02-kali-to-windows-connectivity.jpg) | Ping from Kali → Windows Server confirmed |
| 03 | [03-windows-to-kali-connectivity.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/03-windows-to-kali-connectivity.jpg) | Ping from Windows → Kali confirmed |
| 04 | [04-rdp-enabled-configuration.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/04-rdp-enabled-configuration.jpg) | RDP enabled on Windows Server 2022 |
| 05 | [05-rdp-port-verification.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/05-rdp-port-verification.jpg) | Port 3389 open and listening confirmed |
| 06 | [06-test-user-account-creation.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/06-test-user-account-creation.jpg) | socuser account creation |
| 07 | [07-test-user-account-created.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/07-test-user-account-created.jpg) | socuser account verified |
| 08 | [08-add-user-to-remote-desktop-users-group.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/08-add-user-to-remote-desktop-users-group.jpg) | socuser added to Remote Desktop Users |
| 09 | [09-rdp-user-group-assignment.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/09-rdp-user-group-assignment.jpg) | Group membership confirmed |

<br><br>

### ⚔️ Attack Simulation Evidence (10–12)

| # | File | Description |
|---|---|---|
| 10 | [10-rdp-successful-authentication-command.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/10-rdp-successful-authentication-command.jpg) | xfreerdp successful auth command |
| 11 | [11-successful-rdp-login-from-kali-to-windows-server.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/11-successful-rdp-login-from-kali-to-windows-server.jpg) | Full RDP session from Kali to Windows |
| 12 | [12-rdp-failed-login-attempts-from-kali.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/12-rdp-failed-login-attempts-from-kali.jpg) | xfreerdp failed authentication attempts |

<br><br>

### 🔍 Windows Event Log Investigation (13–16)

| # | File | Timestamp | Description |
|---|---|---|---|
| 13 | [13-eventid-4625-failed-rdp-logon-analysis.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/13-eventid-4625-failed-rdp-logon-analysis.jpg) | 5/26/2026 9:09:07 PM | EventCode 4625 — failed login detail |
| 14 | [14-failed-rdp-authentication-event-4625.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/14-failed-rdp-authentication-event-4625.jpg) | 5/26/2026 9:09:06 PM | EventCode 4625 — NTLM confirmed |
| 15 | [15-successful-rdp-authentication-event-4624-logon-type.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/15-successful-rdp-authentication-event-4624-logon-type.jpg) | 5/26/2026 9:09:57 PM | EventCode 4624 — Logon Type 10 — BREACH |
| 16 | [16-security-log-authentication-analysis-overview.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/16-security-log-authentication-analysis-overview.jpg) | 5/26/2026 | Full Security Log — May 26 confirmed |

<br><br>

### 💻 PowerShell Log Hunting (17–18)

| # | File | Description |
|---|---|---|
| 17 | [17-eventid-4625-failed-logon-powershell-query.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/17-eventid-4625-failed-logon-powershell-query.jpg) | PowerShell query — failed logins |
| 18 | [18-eventid-4624-successful-logon-powershell-query.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/18-eventid-4624-successful-logon-powershell-query.jpg) | PowerShell query — successful logins |

<br><br>

### 🔬 Sysmon Deep Visibility (19–23)

| # | File | Description |
|---|---|---|
| 19 | [19-sysmon-installation-powershell-success.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/19-sysmon-installation-powershell-success.jpg) | Sysmon deployed — SwiftOnSecurity config |
| 20 | [20-sysmon-operational-logs.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/20-sysmon-operational-logs.jpg) | Sysmon operational log view |
| 21 | [21-sysmon-eventid1-process-creation.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/21-sysmon-eventid1-process-creation.jpg) | Sysmon EventCode 1 — process creation |
| 22 | [22-sysmon-process-details-analysis.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/22-sysmon-process-details-analysis.jpg) | Deep process analysis |
| 23 | [23-sysmon-powershell-process-hunting.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/23-sysmon-powershell-process-hunting.jpg) | Post-compromise hunt — no malicious process |

<br><br>

### 🔴 Splunk SIEM Detection (24–27)

| # | File | Timestamp | Description |
|---|---|---|---|
| 24 | [24-splunk-all-sourcetypes-flowing.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/24-splunk-all-sourcetypes-flowing.jpg) | 26/05/2026 | All 4 log sources flowing into Splunk |
| 25 | [25-splunk-bruteforce-detected.jpg](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/25-splunk-bruteforce-detected.jpg) | 26/05/2026 20:24 | 76 events — CRITICAL severity confirmed |
| 26 | [26-splunk-timechart-attack-spike.png](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/26-splunk-timechart-attack-spike.png) | 26/05/2026 12:08 | Attack spike — 538 events — 880 total |
| 27 | [27-splunk-success-after-failures.png](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/27-splunk-success-after-failures.png) | 26/05/2026 12:08 | True Positive — success after failure chain |

<br><br>

## 📊 Evidence Coverage Summary

| Investigation Area | Screenshots | Coverage |
|---|---|---|
| Lab Setup | 01–09 | ✅ Complete |
| Attack Simulation | 10–12 | ✅ Complete |
| Windows Event Logs | 13–16 | ✅ Complete — May 26 2026 |
| PowerShell Hunting | 17–18 | ✅ Complete |
| Sysmon Telemetry | 19–23 | ✅ Complete |
| Splunk SIEM | 24–27 | ✅ Complete |
| **Total** | **27 screenshots** | ✅ **Fully documented** |

<br><br>

## 🔗 Related Files

| File | Location |
|---|---|
| Full Incident Report | [Reports/soc-investigation-report.md](../Reports/soc-investigation-report.md) |
| Detection Rules | [Detection-Logic/Splunk-Detection-Rules.md](../Detection-Logic/Splunk-Detection-Rules.md) |
| Incident Timeline | [Timeline/incident-timeline.md](../Timeline/incident-timeline.md) |
| Investigation Notes | [Investigation-Notes/](../Investigation-Notes/) |

<br><br>

**Analyst:** Priyanka Rane | SOC Analyst L1
**Date:** 26 May 2026
