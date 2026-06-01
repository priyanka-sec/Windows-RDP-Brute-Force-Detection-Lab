# 🔍 Investigation Notes — SOC Analysis Documentation
## Windows RDP Brute Force Detection & SIEM Investigation Lab

<br><br>

## 📁 Folder Contents

| File | Description |
|---|---|
| [authentication-investigation-summary.md](./authentication-investigation-summary.md) | Complete investigation overview — findings, workflow, classification |
| [eventid-4625-analysis.md](./eventid-4625-analysis.md) | Deep analysis of failed login events — volume, pattern, SOC conclusion |
| [eventid-4624-analysis.md](./eventid-4624-analysis.md) | Successful login analysis — breach confirmation — Logon Type 10 |
| [rdp-logon-type-analysis.md](./rdp-logon-type-analysis.md) | Complete Logon Type reference — why Type 10 escalates severity |

<br><br>

## 📌 Purpose

This folder contains all investigation notes produced during
the RDP brute force analysis. Each file documents a specific
aspect of the investigation following the SOC analyst workflow:

```
Alert Received
     ↓
Read alert — understand what fired
     ↓
Triage — is this known FP? What is initial severity?
     ↓
Investigate — pull logs, enrich IOCs, build timeline
     ↓
Classify — True Positive or False Positive?
     ↓
Contain — block, disable, reset, preserve
     ↓
Report — document everything with evidence
     ↓
Escalate — notify L2 with full findings
```

<br><br>

## 🔑 Key Investigation Findings

| Finding | Detail | Severity |
|---|---|---|
| Total failed logins | 880 EventCode 4625 events | CRITICAL |
| Splunk alert count | 76 events in 15 minutes | CRITICAL |
| Successful breach | EventCode 4624 — 5/26/2026 9:09:57 PM | CRITICAL |
| Logon Type | 10 — RemoteInteractive RDP | HIGH |
| Auth Protocol | NTLM — relay attack risk | HIGH |
| Post-compromise | No malicious process detected | Contained |
| Attacker IP | 192.168.56.10 — Kali Linux | Confirmed |
| Target Account | socuser — compromised | Confirmed |

<br><br>

## 📋 Investigation Workflow Used

| Step | Action | Tool |
|---|---|---|
| 1 | Alert received — EventCode 4625 threshold exceeded | Splunk Enterprise |
| 2 | Count-based SPL query — 76 events — CRITICAL confirmed | Splunk SPL |
| 3 | Source IP identified — 192.168.56.10 | Splunk correlation |
| 4 | Breach confirmed — EventCode 4624 Logon Type 10 | Windows Security Log |
| 5 | Post-compromise hunt — Sysmon EventCode 1 | Sysmon + Splunk |
| 6 | No malicious process found — contained | Sysmon analysis |
| 7 | True Positive classified — P2 High | SOC classification |
| 8 | Containment initiated — IP blocked, account disabled | SOC action |
| 9 | Escalated to L2 — INC-RDP-2026-001 raised | Ticket system |

<br><br>

## 🏷️ Final Classification

| Field | Value |
|---|---|
| Alert Classification | True Positive |
| Attack Type | RDP Brute Force — Password Guessing |
| Severity | P2 — High |
| Breach Confirmed | Yes — EventCode 4624 Logon Type 10 |
| Post-Compromise Activity | None detected |
| Data Exfiltration | None confirmed |
| Containment Status | Successful |

<br><br>

## 🔗 Related Files

| File | Location |
|---|---|
| Full Incident Report | [Reports/soc-investigation-report.md](../Reports/soc-investigation-report.md) |
| Detection Rules | [Detection-Logic/Splunk-Detection-Rules.md](../Detection-Logic/Splunk-Detection-Rules.md) |
| Incident Timeline | [Timeline/incident-timeline.md](../Timeline/incident-timeline.md) |
| All Screenshots | [Screenshots/](../Screenshots/) |

<br><br>

**Analyst:** Priyanka Rane | SOC Analyst L1
**Date:** 26 May 2026
