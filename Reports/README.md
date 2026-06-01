# 📋 Reports — SOC Incident Report
## Windows RDP Brute Force Detection & SIEM Investigation Lab

<br><br>

## 📁 Folder Contents

| File | Description |
|---|---|
| [soc-investigation-report.md](./soc-investigation-report.md) | Complete SOC incident report — 10 sections covering detection, IOCs, MITRE mapping, containment, root cause, and recommendations |

<br><br>

## 📌 Purpose

This folder contains the formal SOC incident report produced
following the confirmed RDP brute force attack against
Windows Server 2022.

The report follows the professional SOC documentation
standard used in enterprise security operations centers:

```
Detection → Triage → Analysis → Containment → Escalation
```

<br><br>

## 📄 Report Summary

| Field | Details |
|---|---|
| Report ID | INC-RDP-2026-001 |
| Date | 26 May 2026 |
| Analyst | Priyanka Rane — SOC Analyst L1 |
| Severity | P2 — High |
| Status | Contained |
| Classification | True Positive |
| Attack Type | RDP Brute Force — Password Guessing |
| MITRE Techniques | T1110.001 \| T1021.001 \| T1078.003 |

<br><br>

## 📑 Report Sections

| Section | Content |
|---|---|
| 1. Executive Summary | High-level attack overview for management |
| 2. Detection Method | Splunk SPL queries and Event Log evidence |
| 3. Attack Timeline | Real timestamps from logs and Splunk |
| 4. IOC Table | All indicators with verdict and action taken |
| 5. MITRE ATT&CK Mapping | Sub-technique level mapping with evidence |
| 6. Containment Actions | Exact steps taken to stop the attack |
| 7. Post-Compromise Hunt | Sysmon EventCode 1 analysis results |
| 8. Root Cause Analysis | Why the attack succeeded |
| 9. Recommendations | Prioritized hardening controls |
| 10. Lessons Learned | Key takeaways for SOC improvement |

<br><br>

## 🚨 Key Findings

| Finding | Evidence |
|---|---|
| 880 failed login attempts | Splunk timechart — Screenshot 26 |
| 76 CRITICAL events in 15 min | Splunk SPL — Screenshot 25 |
| Successful breach at 9:09:57 PM | EventCode 4624 — Screenshot 15 |
| Logon Type 10 — RDP confirmed | Event Properties — Screenshot 15 |
| No post-compromise activity | Sysmon EventCode 1 — Screenshot 23 |
| True Positive — contained | Full investigation confirmed |

<br><br>

## 🔗 Related Files

| File | Location |
|---|---|
| Detection Rules | [Detection-Logic/Splunk-Detection-Rules.md](../Detection-Logic/Splunk-Detection-Rules.md) |
| Sigma Rule | [Detection-Logic/sigma-rdp-bruteforce.yml](../Detection-Logic/sigma-rdp-bruteforce.yml) |
| Incident Timeline | [Timeline/incident-timeline.md](../Timeline/incident-timeline.md) |
| Investigation Notes | [Investigation-Notes/](../Investigation-Notes/) |
| Hardening Guide | [Mitigation-Recommendations/rdp-hardening-recommendations.md](../Mitigation-Recommendations/rdp-hardening-recommendations.md) |
| All Screenshots | [Screenshots/](../Screenshots/) |

<br><br>

**Analyst:** Priyanka Rane | SOC Analyst L1
**Date:** 26 May 2026
