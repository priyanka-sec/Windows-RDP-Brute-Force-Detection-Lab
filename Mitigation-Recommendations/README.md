# 🛡️ Mitigation Recommendations — Security Hardening Guide
## Windows RDP Brute Force Detection & SIEM Investigation Lab

<br><br>

## 📁 Folder Contents

| File | Description |
|---|---|
| [rdp-hardening-recommendations.md](./rdp-hardening-recommendations.md) | Complete RDP hardening guide — 12 controls across 4 priority levels with exact PowerShell implementation commands |

<br><br>

## 📌 Purpose

This folder contains post-incident security hardening
recommendations produced after the confirmed RDP brute
force attack against Windows Server 2022.

These recommendations directly address every root cause
identified during the investigation and are prioritized
by impact and ease of implementation.

<br><br>

## 🔴 Root Causes Identified

| Root Cause | Risk | Priority |
|---|---|---|
| No account lockout policy | Unlimited login attempts allowed | CRITICAL |
| RDP directly exposed on network | Any host can attempt auth | CRITICAL |
| No MFA on RDP accounts | Password alone sufficient | CRITICAL |
| NTLM authentication active | Relay attack vulnerable | HIGH |
| Weak password on socuser | Cracked via wordlist | HIGH |
| No real-time SIEM alerting | Detection was reactive | MEDIUM |

<br><br>

## ✅ Hardening Controls Summary

| Priority | Control | Status |
|---|---|---|
| CRITICAL | Account lockout after 5 failures | ⬜ Pending |
| CRITICAL | RDP behind VPN only | ⬜ Pending |
| CRITICAL | MFA on all RDP accounts | ⬜ Pending |
| HIGH | Replace NTLM with Kerberos | ⬜ Pending |
| HIGH | 14-character minimum password | ⬜ Pending |
| HIGH | Change default RDP port | ⬜ Pending |
| MEDIUM | Splunk real-time 4625 alert | ⬜ Pending |
| MEDIUM | Enable Network Level Auth | ⬜ Pending |
| MEDIUM | Restrict RDP to admin accounts | ⬜ Pending |
| LOW | Disable RDP on non-essential servers | ⬜ Pending |
| LOW | Session timeout policies | ⬜ Pending |
| LOW | Deploy Credential Guard | ⬜ Pending |

<br><br>

## 🎯 Most Critical Fix

**Enable account lockout policy immediately:**

```powershell
net accounts /lockoutthreshold:5
net accounts /lockoutduration:30
net accounts /lockoutwindow:15
```

This single command stops the entire brute force attack
class. 5 failed attempts triggers a 30-minute lockout —
making automated password guessing computationally
impractical.

<br><br>

## 🔗 Related Files

| File | Location |
|---|---|
| Full Incident Report | [Reports/soc-investigation-report.md](../Reports/soc-investigation-report.md) |
| Detection Rules | [Detection-Logic/Splunk-Detection-Rules.md](../Detection-Logic/Splunk-Detection-Rules.md) |
| Investigation Notes | [Investigation-Notes/](../Investigation-Notes/) |
| Incident Timeline | [Timeline/incident-timeline.md](../Timeline/incident-timeline.md) |

<br><br>

**Analyst:** Priyanka Rane | SOC Analyst L1
**Date:** 26 May 2026
