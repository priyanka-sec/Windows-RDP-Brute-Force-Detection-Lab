# Incident Timeline
## INC-RDP-2026-001 — Windows RDP Brute Force Attack

| Field | Details |
|---|---|
| **Incident ID** | INC-RDP-2026-001 |
| **Date** | 26 May 2026 |
| **Analyst** | Priyanka Rane — SOC Analyst L1 |
| **Severity** | P2 — High |
| **Status** | Contained |

<br><br>

## Detailed Attack Timeline

| Time | Phase | Event | EventCode | Source | Screenshot Evidence |
|---|---|---|---|---|---|
| 5/26/2026 9:09:06 PM | 🔴 Attack Detected | Failed RDP login — socuser — 192.168.56.10 — NTLM | 4625 | Windows Security Log | Screenshot 14 |
| 5/26/2026 9:09:07 PM | 🔴 Attack Continues | Second failed login — same source confirmed | 4625 | Windows Security Log | Screenshot 13 |
| 5/26/2026 9:09:57 PM | 🚨 Breach Confirmed | Successful RDP login — socuser — Logon Type 10 | 4624 | Windows Security Log | Screenshot 15 |
| 26/05/2026 20:24:10 | 🟡 SIEM Alert | Splunk detects 76 failed logins — CRITICAL severity | SPL | Splunk Enterprise | Screenshot 25 |
| 26/05/2026 20:24:10 | 🟡 Investigation | SOC Analyst opens Splunk — begins triage | — | SOC | Screenshot 25 |
| 26/05/2026 12:08:33 | 🟡 TP Confirmed | First event in success-after-failure chain confirmed | 4624/4625 | Splunk SPL | Screenshot 27 |
| 26/05/2026 20:39:10 | 🟡 Analysis Complete | Full attack pattern correlated — True Positive classified | — | Splunk SPL | Screenshot 25 |
| Post-detection | 🟡 Sysmon Hunt | EventCode 1 searched — no malicious process found | Sysmon 1 | Sysmon Log | Screenshot 23 |
| Post-detection | 🟢 Containment | Source IP 192.168.56.10 blocked at Windows Firewall | — | SOC Action | — |
| Post-detection | 🟢 Containment | socuser account disabled and password reset | — | SOC Action | — |
| Post-detection | 🟢 Forensics | Security logs exported as .evtx forensic evidence | — | Event Viewer | Screenshot 16 |
| Post-detection | 🟢 Escalation | L2 notified — ticket INC-RDP-2026-001 raised | — | Ticket System | — |

<br><br>

## Attack Volume Analysis

| Time | Event Count | Source |
|---|---|---|
| 26/05/2026 12:08:00 | 538 events | Splunk Timechart |
| 26/05/2026 12:09:00 | 172 events | Splunk Timechart |
| 26/05/2026 12:10:00 | 60 events | Splunk Timechart |
| 26/05/2026 12:11:00 | 20 events | Splunk Timechart |
| 26/05/2026 12:12:00 | 20 events | Splunk Timechart |
| 26/05/2026 12:13:00 | 39 events | Splunk Timechart |
| 26/05/2026 12:14:00 | 19 events | Splunk Timechart |
| 26/05/2026 12:15:00 | 12 events | Splunk Timechart |
| **Total** | **880 events** | Screenshot 26 |

> Source: `index=main | timechart span=1m count`
> Evidence: Screenshots/26-splunk-timechart-attack-spike.png

<br><br>

## Phase Summary

| Phase | Event | Key Evidence |
|---|---|---|
| 🔴 Attack | Failed logins from 192.168.56.10 targeting socuser | Screenshots 13, 14 — EventID 4625 |
| 🚨 Breach | Successful RDP login — Logon Type 10 confirmed | Screenshot 15 — EventID 4624 |
| 🟡 Detection | Splunk CRITICAL alert — 76 events in 15 minutes | Screenshot 25 — Splunk SPL |
| 🟡 Investigation | Attack pattern correlated — True Positive confirmed | Screenshots 26, 27 — Splunk |
| 🟢 Containment | IP blocked — account disabled — evidence preserved | Screenshot 16 — Security Log |
| 🟢 Escalation | L2 notified — INC-RDP-2026-001 raised | SOC runbook followed |

<br><br>

## Key Metrics

| Metric | Value | Evidence |
|---|---|---|
| Total failed logins | 880 events | Screenshot 26 — Splunk timechart |
| Splunk CRITICAL count | 76 events in 15 minutes | Screenshot 25 |
| First failed login | 5/26/2026 9:09:06 PM | Screenshot 14 |
| Successful breach | 5/26/2026 9:09:57 PM | Screenshot 15 |
| Post-compromise activity | None detected | Screenshot 23 — Sysmon |
| Data exfiltration | None confirmed | Sysmon EventCode 1 hunt |
| Severity | P2 — High | SOC classification |
| Final classification | True Positive — Contained | Full investigation |

<br><br>

## Evidence Index

| Screenshot | File Name | Evidence Type |
|---|---|---|
| 13 | 13-eventid-4625-failed-rdp-logon-analysis.jpg | EventID 4625 — 5/26/2026 9:09:07 PM |
| 14 | 14-failed-rdp-authentication-event-4625.jpg | EventID 4625 — 5/26/2026 9:09:06 PM |
| 15 | 15-successful-rdp-authentication-event-4624-logon-type.jpg | EventID 4624 — Logon Type 10 — 5/26/2026 9:09:57 PM |
| 16 | 16-security-log-authentication-analysis-overview.jpg | Full Security Log — May 26 2026 |
| 25 | 25-splunk-bruteforce-detected.png | Splunk CRITICAL — 76 events |
| 26 | 26-splunk-timechart-attack-spike.png | Attack volume — 880 events |
| 27 | 27-splunk-success-after-failures.png | TP confirmation — success after failure |

<br><br>

**Analyst:** Priyanka Rane | SOC Analyst L1
**Date:** 26 May 2026
**Timeline Status:** Final
