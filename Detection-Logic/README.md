# 🧠 Detection Logic — SOC Detection Engineering
## Windows RDP Brute Force Detection & SIEM Investigation Lab

<br><br>

## 📁 Folder Contents

| File | Description |
|---|---|
| [detection-notes.md](./detection-notes.md) | Complete detection methodology — layered approach across Event Logs, Sysmon, and Splunk SIEM |
| [Splunk-Detection-Rules.md](./Splunk-Detection-Rules.md) | All SPL queries used to detect and confirm the brute force attack |
| [sigma-rdp-bruteforce.yml](./sigma-rdp-bruteforce.yml) | Production-ready Sigma rule for RDP brute force detection |
| [powershell-eventid-4625-query.txt](./powershell-eventid-4625-query.txt) | PowerShell queries for failed login investigation |
| [powershell-eventid-4624-query.txt](./powershell-eventid-4624-query.txt) | PowerShell queries for successful login investigation |

<br><br>

## 📌 Purpose

This folder contains all detection logic developed during the
RDP brute force investigation. It demonstrates how a SOC analyst
moves from raw log data to confirmed threat detection using:

- Count-based threshold alerting in Splunk
- True Positive / False Positive classification logic
- Sigma rule writing for cross-SIEM compatibility
- PowerShell local log hunting
- Post-compromise process hunting via Sysmon

<br><br>

## 🔍 Detection Methodology

The attack was detected using a three-layer approach:

```
Layer 1 — Windows Security Event Logs
    EventCode 4625 — Failed login detection
    EventCode 4624 — Breach confirmation
    EventCode 4776 — NTLM weakness identification
         ↓
Layer 2 — Sysmon Telemetry
    EventCode 1 — Post-compromise process hunting
    EventCode 3 — Network connection monitoring
         ↓
Layer 3 — Splunk SIEM Correlation
    Count-based threshold query — CRITICAL alert
    Success-after-failure correlation — TP confirmed
    Timechart visualization — attack pattern proven
```

<br><br>

## 🚨 Detection Results

| Query | Result | Severity |
|---|---|---|
| EventCode 4625 count by host | 76 events in 15 minutes | **CRITICAL** |
| Success after failure chain | EventCode 4624 confirmed | **TRUE POSITIVE** |
| Sysmon EventCode 1 hunt | No malicious process found | Contained |
| Total attack volume | 880 events | CRITICAL |

<br><br>

## 📝 Splunk Alert Rule Summary

```splunk
index=main EventCode=4625
| stats count by host
| eval severity=if(count>50,"CRITICAL",if(count>20,"HIGH","MEDIUM"))
| sort -count
```

**Threshold Logic:**

| Count | Severity | Action |
|---|---|---|
| > 5 | MEDIUM | Monitor — raise ticket |
| > 20 | HIGH | Investigate immediately |
| > 50 | CRITICAL | Escalate to L2 now |
| SUCCESS after failures | TRUE POSITIVE | Containment — P1/P2 |

<br><br>

## 🛡️ Sigma Rule Summary

The Sigma rule in this folder can be deployed in any SIEM:

```yaml
title: RDP Brute Force Authentication Attack
detection:
    selection:
        EventID: 4625
        LogonType: 10
    timeframe: 5m
    condition: selection | count() by IpAddress > 10
level: high
tags:
    - attack.t1110.001
    - attack.t1021.001
```

<br><br>

## 🔗 Related Files

| File | Location |
|---|---|
| Full Incident Report | [Reports/soc-investigation-report.md](../Reports/soc-investigation-report.md) |
| Incident Timeline | [Timeline/incident-timeline.md](../Timeline/incident-timeline.md) |
| Investigation Notes | [Investigation-Notes/](../Investigation-Notes/) |
| All Screenshots | [Screenshots/](../Screenshots/) |

<br><br>

**Analyst:** Priyanka Rane | SOC Analyst L1
**Date:** 26 May 2026
