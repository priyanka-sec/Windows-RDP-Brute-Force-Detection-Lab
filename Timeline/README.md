# 🕒 Timeline — Incident Timeline Folder
## INC-RDP-2026-001 — Windows RDP Brute Force Attack

<br><br>

## 📁 Folder Contents

| File | Description |
|---|---|
| [incident-timeline.md](./incident-timeline.md) | Complete incident timeline with real timestamps extracted from Windows Security Event Logs and Splunk SIEM |

<br><br>

## 📌 Purpose

This folder contains the complete incident timeline reconstructed
during the Windows RDP Brute Force Detection & SIEM Investigation Lab.

Timeline reconstruction is a critical SOC analyst skill used to:

- Establish exact sequence of attacker actions
- Correlate events across multiple log sources
- Identify time between attack start and breach confirmation
- Measure detection and containment response times
- Provide evidence for incident escalation and reporting

<br><br>

## 🔍 Timeline Sources

The timeline was reconstructed using evidence from four sources:

| Source | Evidence Provided | Screenshot |
|---|---|---|
| Windows Security Event Log | Real timestamps for EventCode 4625 and 4624 | Screenshots 13, 14, 15 |
| Windows Event Viewer Overview | Full security log date confirmation | Screenshot 16 |
| Splunk Enterprise SIEM | CRITICAL alert timestamp and event count | Screenshot 25 |
| Splunk Timechart | Attack volume per minute — 880 total events | Screenshot 26 |
| Splunk TP Confirmation | Success-after-failure chain timestamp | Screenshot 27 |

<br><br>

## ⏱️ Timeline Summary

| Phase | Time | Key Event |
|---|---|---|
| 🔴 Attack Detected | 5/26/2026 9:09:06 PM | First failed RDP login — EventCode 4625 |
| 🚨 Breach Confirmed | 5/26/2026 9:09:57 PM | Successful login — EventCode 4624 — Logon Type 10 |
| 🟡 SIEM Detection | 26/05/2026 20:24:10 | Splunk CRITICAL alert — 76 events in 15 minutes |
| 🟡 TP Confirmed | 26/05/2026 20:39:10 | True Positive — success after failure chain |
| 🟢 Containment | Post-detection | IP blocked — account disabled — evidence preserved |
| 🟢 Escalation | Post-detection | L2 notified — INC-RDP-2026-001 raised |

<br><br>

## 📊 Attack Volume

| Time | Event Count |
|---|---|
| 26/05/2026 12:08 | 538 events |
| 26/05/2026 12:09 | 172 events |
| 26/05/2026 12:10 | 60 events |
| 26/05/2026 12:11 – 12:15 | 110 events |
| **Total** | **880 events** |

> Source: Splunk timechart query
> Evidence: [Screenshot 26](https://github.com/priyanka-sec/Windows-RDP-Brute-Force-Detection-Lab/blob/main/Screenshots/26-splunk-timechart-attack-spike.png)

<br><br>

## 🔗 Related Files

| File | Location |
|---|---|
| Full Incident Report | [Reports/soc-investigation-report.md](../Reports/soc-investigation-report.md) |
| Detection Rules | [Detection-Logic/splunk-detection-rules.md](../Detection-Logic/splunk-detection-rules.md) |
| Sigma Rule | [Detection-Logic/sigma-rdp-bruteforce.yml](../Detection-Logic/sigma-rdp-bruteforce.yml) |
| Hardening Guide | [Mitigation-Recommendations/rdp-hardening-recommendations.md](../Mitigation-Recommendations/rdp-hardening-recommendations.md) |
| All Screenshots | [Screenshots/](../Screenshots/) |

<br><br>

**Analyst:** Priyanka Rane | SOC Analyst L1
**Date:** 26 May 2026
