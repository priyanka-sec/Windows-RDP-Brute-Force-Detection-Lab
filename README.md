<h1 align="center">🛡️ Windows RDP Brute Force Detection & SIEM Investigation Lab</h1>

<h3 align="center">
RDP Brute Force Simulation · Windows Event Log Analysis · Splunk SIEM Detection · Sysmon Telemetry · SOC Investigation Workflow
</h3>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-VirtualBox-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/Attacker-Kali%20Linux-red?style=flat-square"/>
  <img src="https://img.shields.io/badge/Victim-Windows%20Server%202022-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/SIEM-Splunk%20Enterprise-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/Telemetry-Sysmon-orange?style=flat-square"/>
  <img src="https://img.shields.io/badge/Log%20Forwarding-Splunk%20UF-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/MITRE%20ATT%26CK-T1110.001%20%7C%20T1021.001%20%7C%20T1078.003-red?style=flat-square"/>
  <img src="https://img.shields.io/badge/Status-Completed-brightgreen?style=flat-square"/>
</p>

<br><br>

# 📑 Table of Contents

- [Project Overview](#-project-overview)
- [Objectives](#-objectives)
- [Lab Architecture](#️-lab-architecture)
- [Tools & Technologies](#️-tools--technologies)
- [Attack Simulation Workflow](#️-attack-simulation-workflow)
- [Splunk SIEM Detection](#-splunk-siem-detection)
- [Sysmon Deep Visibility](#-sysmon-deep-visibility)
- [Windows Event Log Analysis](#-windows-event-log-analysis)
- [Detection Logic](#-detection-logic)
- [Indicators of Compromise](#-indicators-of-compromise-iocs)
- [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
- [Incident Timeline](#-incident-timeline)
- [Containment & Response](#️-containment--response)
- [Mitigation Recommendations](#️-mitigation-recommendations)
- [Lessons Learned](#-lessons-learned)
- [Screenshots](#️-screenshots)
- [About the Analyst](#-about-the-analyst)

<br><br>

# 📌 Project Overview

This project simulates a real-world RDP brute force attack against a Windows Server 2022 environment and demonstrates a complete SOC analyst investigation workflow — from attack simulation through SIEM detection, log analysis, MITRE ATT&CK mapping, and incident response.

**What makes this lab different from typical student projects:**

- Real RDP authentication traffic generated from Kali Linux using xfreerdp
- Logs forwarded live into Splunk Enterprise via Universal Forwarder
- Sysmon deployed for deep process and network visibility
- SPL detection queries and Sigma rules written from scratch
- Complete incident report following SOC runbook: Detection → Triage → Containment → Escalation
- All timestamps and findings verified against real log evidence

<br><br>

# 🎯 Objectives

- Simulate RDP brute force attack in a controlled lab environment
- Forward Windows Security logs to Splunk Enterprise in real time
- Detect brute force pattern using SPL count-based threshold queries
- Confirm breach using EventCode 4624 Logon Type 10 correlation
- Hunt post-compromise activity using Sysmon EventCode 1
- Map full attack chain to MITRE ATT&CK sub-techniques
- Produce professional SOC incident report with real evidence
- Write production-ready Sigma detection rule

<br><br>

# 🏗️ Lab Architecture

## Environment Configuration

| Component | Details |
|---|---|
| Attacker Machine | Kali Linux — 192.168.56.10 |
| Victim Machine | Windows Server 2022 — 192.168.56.110 |
| SOC / SIEM | Splunk Enterprise — Host Machine 192.168.56.1 |
| Virtualization | VirtualBox — NAT + Host-Only Adapter |
| Network | Host-Only: 192.168.56.0/24 |
| Log Forwarding | Splunk Universal Forwarder → Splunk Enterprise (Port 9997) |
| Deep Logging | Sysmon (SwiftOnSecurity config) |
| Targeted Protocol | RDP — Port 3389 |

<br><br>

## 🖼️ Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                  VirtualBox Lab Environment                  │
│                                                             │
│  ┌──────────────────┐        ┌──────────────────────────┐  │
│  │   Kali Linux     │──RDP──▶│   Windows Server 2022    │  │
│  │  192.168.56.10   │ :3389  │    192.168.56.110        │  │
│  │   ATTACKER       │        │  Sysmon + Splunk UF      │  │
│  └──────────────────┘        └────────────┬─────────────┘  │
│                                           │ Logs (9997)     │
│                                           ▼                 │
│                              ┌──────────────────────────┐  │
│                              │    Splunk Enterprise      │  │
│                              │      192.168.56.1         │  │
│                              │    SOC ANALYST VIEW       │  │
│                              └──────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

<br><br>

# 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Kali Linux | Attack simulation — xfreerdp brute force |
| Windows Server 2022 | Target — RDP enabled, AD environment |
| Splunk Enterprise | SIEM — log ingestion, SPL queries, alerting |
| Splunk Universal Forwarder | Ships Windows logs → Splunk in real time |
| Sysmon | Deep endpoint visibility — process, network, file |
| xfreerdp | RDP authentication testing tool |
| Windows Event Viewer | Manual log verification |
| PowerShell | Local log hunting and validation |
| MITRE ATT&CK Navigator | Attack technique mapping |

<br><br>

# ⚔️ Attack Simulation Workflow

## Phase 1 — Environment Setup

- Configured VirtualBox Host-Only network (192.168.56.0/24)
- Installed Windows Server 2022 with RDP enabled on port 3389
- Created test user account `socuser` — added to Remote Desktop Users group
- Deployed Sysmon with SwiftOnSecurity configuration for deep logging
- Installed Splunk Universal Forwarder — forwarding to 192.168.56.1:9997
- Configured inputs.conf to forward Security, System, Application and Sysmon logs

## Phase 2 — Attack Execution

From Kali Linux, repeated failed RDP authentication attempts were generated
using xfreerdp with incorrect credentials:

```bash
xfreerdp /u:socuser /p:'WrongPassword' /v:192.168.56.110 /cert:ignore
```

After multiple failed attempts, a successful RDP login was performed
using valid credentials confirming breach:

```bash
xfreerdp /u:socuser /p:'Password@123' /v:192.168.56.110 /cert:ignore
```

This generated:
- Multiple failed authentication events — EventCode 4625
- One successful authentication event — EventCode 4624
- Logon Type 10 — RDP session confirmed
- NTLM authentication logs — EventCode 4776
- Sysmon process creation telemetry — EventCode 1

## Phase 3 — Detection & Investigation

All attack events were forwarded to Splunk in real time via Universal
Forwarder. SPL queries were used to detect, correlate, and classify
the attack. Sysmon logs provided post-compromise process visibility.

<br><br>

# 🔍 Splunk SIEM Detection

All logs were ingested into Splunk Enterprise via Universal Forwarder
installed on Windows Server 2022. The following SPL queries detected
and confirmed the attack.

## Query 1 — Brute Force Detection

```splunk
index=main EventCode=4625
| stats count by host
| eval severity=if(count>50,"CRITICAL",if(count>20,"HIGH","MEDIUM"))
| sort -count
```

**Result:** WIN-TLKR5B0U5QP — count=76 — **CRITICAL**

## Query 2 — Attack Success Confirmation

```splunk
index=main (EventCode=4625 OR EventCode=4624)
| eval event_type=if(EventCode=4624,"SUCCESS","FAILURE")
| table _time, event_type, Account_Name
| sort _time
```

**Result:** Chain of FAILURE events ending in SUCCESS —
True Positive breach confirmed.

## Query 3 — Attack Volume Timechart

```splunk
index=main EventCode=4625
| timechart span=1m count
```

**Result:** Spike of 538 events at 12:08 — clear brute
force pattern visible.

## Query 4 — Post-Compromise Process Hunt

```splunk
index=main sourcetype="WinEventLog:Sysmon" EventCode=1
| table _time, User, Image, CommandLine, ParentImage
| sort -_time
```

**Result:** No malicious process execution detected.

<br><br>

# 🔬 Sysmon Deep Visibility

Sysmon was deployed on Windows Server 2022 using the SwiftOnSecurity
configuration providing deep endpoint visibility beyond standard
Windows Event Logs.

## Sysmon Event IDs Monitored

| Sysmon EventCode | Description | SOC Value |
|---|---|---|
| 1 | Process Creation | Full command line of every process |
| 3 | Network Connection | Every outbound connection with PID |
| 7 | Image Loaded | DLL loading — detects injection |
| 10 | Process Access | Detects Mimikatz targeting LSASS |
| 11 | File Created | Malware dropping files |
| 13 | Registry Value Set | Persistence mechanisms |

## Key Finding

Sysmon EventCode 1 confirmed no suspicious post-authentication
process execution following the successful RDP session. The attacker
did not execute any commands during the active session window.

<br><br>

# 📊 Windows Event Log Analysis

## Critical Event IDs

| Event ID | Source | Description | Count |
|---|---|---|---|
| 4625 | Security | Failed login attempt | 880 total |
| 4624 | Security | Successful login — Logon Type 10 | 1 confirmed |
| 4776 | Security | NTLM authentication attempt | Observed |
| 4672 | Security | Special privileges assigned | Post-login |
| Sysmon 1 | Sysmon | Process creation post-compromise | No malicious |
| Sysmon 3 | Sysmon | Network connections | Monitored |

## Key Event Details

**EventCode 4625 — Failed Login**
- Account Name: socuser
- Source IP: 192.168.56.10
- Workstation: kali
- Auth Package: NTLM
- Logged: 5/26/2026 9:09:06 PM
- Evidence: [Screenshot 14](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/14-failed-rdp-authentication-event-4625.jpg)

**EventCode 4624 — Successful Login**
- Account Name: socuser
- Source IP: 192.168.56.10
- Logon Type: 10 (RemoteInteractive — RDP)
- Logged: 5/26/2026 9:09:57 PM
- Evidence: [Screenshot 15](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/15-successful-rdp-authentication-event-4624-logon-type.jpg)

<br><br>

# 🧠 Detection Logic

See full detection files in the [Detection-Logic](./Detection-Logic/) folder.

## Splunk Alert Rule Summary

- **Trigger:** Single host with >5 failed logins (EventCode 4625)
- **Severity:** >50 = CRITICAL — >20 = HIGH — >5 = MEDIUM
- **Result:** 76 events — CRITICAL severity confirmed
- **Escalation:** CRITICAL alert → immediate L2 escalation

## Sigma Rule

Production-ready Sigma rule available at:
[Detection-Logic/sigma-rdp-bruteforce.yml](./Detection-Logic/sigma-rdp-bruteforce.yml)

<br><br>

# 🚨 Indicators of Compromise (IOCs)

| IOC Type | Observed Value | Verdict | Validated Via | Action Taken |
|---|---|---|---|---|
| Source IP | 192.168.56.10 | **MALICIOUS** — confirmed attacker | Windows Security Log + Splunk | Blocked at Windows Firewall |
| Target Account | socuser | **COMPROMISED** — successful login confirmed | EventCode 4624 — 9:09:57 PM — Logon Type 10 | Account disabled + password reset |
| Auth Protocol | NTLM | **WEAK** — relay attack risk | EventCode 4776 — NtLmSsp confirmed | Kerberos enforcement recommended |
| Failed Logins | 880 total — 76 in 15 min | **BRUTE FORCE CONFIRMED** | Splunk count query — CRITICAL | Account lockout enforced |
| Successful Login | EventCode 4624 — 9:09:57 PM | **TRUE POSITIVE — BREACH** | Logon Type 10 — RDP session | Immediate containment initiated |

<br><br>

# 🧠 MITRE ATT&CK Mapping

| Tactic | Technique | Sub-Technique | ID | Evidence |
|---|---|---|---|---|
| Credential Access | Brute Force | Password Guessing | T1110.001 | 880 × EventCode 4625 targeting socuser |
| Lateral Movement | Remote Services | Remote Desktop Protocol | T1021.001 | EventCode 4624 — Logon Type 10 confirmed |
| Defense Evasion | Valid Accounts | Local Accounts | T1078.003 | Successful login using valid socuser credentials |

<br><br>

# 🕒 Incident Timeline

| Time | Phase | Event | EventCode | Evidence |
|---|---|---|---|---|
| 5/26/2026 9:09:06 PM | 🔴 Attack | First failed RDP login — socuser — 192.168.56.10 | 4625 | [Screenshot 14](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/14-failed-rdp-authentication-event-4625.jpg) |
| 5/26/2026 9:09:07 PM | 🔴 Attack | Second failed login — NTLM confirmed | 4625 | [Screenshot 13](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/13-eventid-4625-failed-rdp-logon-analysis.jpg) |
| 5/26/2026 9:09:57 PM | 🚨 Breach | Successful RDP login — Logon Type 10 | 4624 | [Screenshot 15](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/15-successful-rdp-authentication-event-4624-logon-type.jpg) |
| 26/05/2026 20:24:10 | 🟡 Detection | Splunk CRITICAL alert — 76 events in 15 min | SPL | [Screenshot 25](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/25-splunk-bruteforce-detected.png) |
| 26/05/2026 20:39:10 | 🟡 TP Confirmed | Success-after-failure chain — True Positive | SPL | [Screenshot 27](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/27-splunk-success-after-failures.png) |
| Post-detection | 🟡 Sysmon Hunt | No malicious process found post-breach | Sysmon 1 | [Screenshot 23](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/23-sysmon-powershell-process-hunting.jpg) |
| Post-detection | 🟢 Containment | IP blocked — account disabled — password reset | — | SOC Action |
| Post-detection | 🟢 Escalation | L2 notified — INC-RDP-2026-001 raised | — | SOC Runbook |

<br><br>

# 🛡️ Containment & Response

## Incident ID: INC-RDP-2026-001
## Severity: P2 — High
## Analyst: Priyanka Rane — SOC Analyst L1

## Containment Actions Taken

| Action | Outcome |
|---|---|
| Blocked source IP 192.168.56.10 at Windows Firewall | No further RDP connections possible |
| Disabled socuser account | Active RDP session terminated |
| Reset socuser password | Compromised credential invalidated |
| Exported Security logs as forensic .evtx | Evidence preserved for L2 |
| Escalated to L2 — ticket INC-RDP-2026-001 | Post-compromise review initiated |

## Post-Compromise Hunt Result

Sysmon EventCode 1 searched for all process creation after
confirmed breach at 9:09:57 PM. No malicious processes found.
Incident classified as **contained — no post-compromise impact.**

## Escalation Decision

Escalated to L2 because EventCode 4624 confirmed a successful
attacker RDP session with Logon Type 10. Post-compromise
investigation was required before closing the incident.

<br><br>

# 🛡️ Mitigation Recommendations

| Priority | Recommendation | Impact |
|---|---|---|
| CRITICAL | Account lockout after 5 failed attempts | Stops brute force immediately |
| CRITICAL | Restrict RDP behind VPN only | Eliminates direct attack surface |
| CRITICAL | Enforce MFA on all RDP accounts | Credential theft becomes useless |
| HIGH | Replace NTLM with Kerberos | Eliminates relay attack risk |
| HIGH | Enforce 14-character minimum password | Increases crack time exponentially |
| MEDIUM | Deploy Splunk real-time alert for 4625 | Detection within 5 minutes |
| MEDIUM | Enable Network Level Authentication | Pre-session auth layer |
| LOW | Disable RDP on non-essential servers | Reduces attack surface |

Full hardening guide:
[Mitigation-Recommendations/rdp-hardening-recommendations.md](./Mitigation-Recommendations/rdp-hardening-recommendations.md)

<br><br>

# 📚 Lessons Learned

1. **Account lockout policy is non-negotiable.** 880 attempts
   succeeded without any automatic blocking. One Group Policy
   change stops this attack class entirely.

2. **RDP must never be directly exposed.** Any machine with
   port 3389 accessible on a network is an active target.
   VPN-only access is the minimum acceptable standard.

3. **Splunk detected what manual review would have missed.**
   The count-based threshold query identified 76 CRITICAL
   events instantly across a 15-minute window.

4. **Sysmon provided critical post-compromise clarity.**
   Without Sysmon EventCode 1, confirming no malicious
   process execution post-breach would have been impossible.

5. **NTLM is a legacy risk.** Every environment should
   migrate to Kerberos. NTLM relay attacks are trivially
   exploitable with tools like Responder.

<br><br>

# 🖼️ Screenshots

| # | Screenshot | Description |
|---|---|---|
| 01 | [Lab Setup](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/01-lab-setup.png) | VirtualBox lab environment |
| 02 | [Kali → Windows Connectivity](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/02-kali-to-windows-connectivity.jpg) | Ping verification |
| 03 | [Windows → Kali Connectivity](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/03-windows-to-kali-connectivity.jpg) | Ping verification |
| 04 | [RDP Enabled](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/04-rdp-enabled-configuration.jpg) | RDP configuration on Windows Server |
| 05 | [RDP Port Verified](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/05-rdp-port-verification.jpg) | Port 3389 open confirmed |
| 06 | [User Created](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/06-test-user-account-creation.jpg) | socuser account creation |
| 07 | [User Confirmed](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/07-test-user-account-created.jpg) | Account verification |
| 08 | [RDP Group Assignment](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/08-add-user-to-remote-desktop-users-group.jpg) | User added to RDP group |
| 09 | [Group Verified](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/09-rdp-user-group-assignment.jpg) | Group membership confirmed |
| 10 | [RDP Auth Command](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/10-rdp-successful-authentication-command.png) | Successful RDP from Kali |
| 11 | [Successful RDP Session](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/11-successful-rdp-login-from-kali-to-windows-server.jpg) | Full RDP session established |
| 12 | [Failed Login Attempts](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/12-rdp-failed-login-attempts-from-kali.png) | xfreerdp failed attempts |
| 13 | [EventID 4625 Analysis](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/13-eventid-4625-failed-rdp-logon-analysis.jpg) | Failed login — 5/26/2026 9:09:07 PM |
| 14 | [4625 Log Detail](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/14-failed-rdp-authentication-event-4625.jpg) | Failed login — 5/26/2026 9:09:06 PM |
| 15 | [4624 Logon Type 10](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/15-successful-rdp-authentication-event-4624-logon-type.jpg) | Successful login — 5/26/2026 9:09:57 PM |
| 16 | [Security Log Overview](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/16-security-log-authentication-analysis-overview.jpg) | Full security log — May 26 2026 |
| 17 | [PowerShell 4625](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/17-eventid-4625-failed-logon-powershell-query.jpg) | PS query — failed logins |
| 18 | [PowerShell 4624](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/18-eventid-4624-successful-logon-powershell-query.jpg) | PS query — successful logins |
| 19 | [Sysmon Install](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/19-sysmon-installation-powershell-success.jpg) | Sysmon deployment confirmed |
| 20 | [Sysmon Logs](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/20-sysmon-operational-logs.jpg) | Sysmon operational log view |
| 21 | [Sysmon EventID 1](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/21-sysmon-eventid1-process-creation.jpg) | Process creation event |
| 22 | [Sysmon Process Details](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/22-sysmon-process-details-analysis.jpg) | Deep process analysis |
| 23 | [Sysmon PS Hunt](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/23-sysmon-powershell-process-hunting.jpg) | Post-compromise process hunt |
| 24 | [Splunk Sources Flowing](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/24-splunk-all-sourcetypes-flowing.png) | All 4 log sources in Splunk |
| 25 | [Splunk CRITICAL Detection](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/25-splunk-bruteforce-detected.png) | 76 events — CRITICAL severity |
| 26 | [Splunk Attack Timechart](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/26-splunk-timechart-attack-spike.png) | Attack spike — 880 events |
| 27 | [Splunk TP Confirmed](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/27-splunk-success-after-failures.png) | Success after failure chain |

<br><br>

# 👩‍💻 About the Analyst

<div align="center">

**Priyanka Rane** — SOC Analyst L1

BSc Information Technology — CGPA 9.70 | University of Mumbai

🏅 Certified Ethical Hacker v13 AI (CEHv13) — EC-Council

🏅 eLearnSecurity Network Penetration Tester (eNPT) — INE

📧 ranepriyanka567@gmail.com

🔗 [LinkedIn](https://www.linkedin.com/in/priyanka-rane-606a71257/)

🐙 [GitHub](https://github.com/priyanka-sec)

</div>

<br>

> ⭐ If this project helped you understand SOC detection workflows, feel free to star the repository.
