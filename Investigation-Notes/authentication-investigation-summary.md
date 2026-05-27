# Authentication Investigation Summary
## INC-RDP-2026-001

**Analyst:** Priyanka Rane — SOC L1
**Date:** 26 May 2026
**Status:** Contained

<br><br>

## Investigation Overview

A brute force attack was detected against Windows Server 2022
targeting the `socuser` account via RDP (port 3389).
The attacker IP 192.168.56.10 (Kali Linux) generated 142
failed authentication attempts followed by one successful login.

<br><br>

## Key Findings

| Finding | Detail | Severity |
|---|---|---|
| Total failed logins | 142 EventCode 4625 events | CRITICAL |
| Successful login | 1 EventCode 4624 confirmed | CRITICAL |
| Logon Type | 10 — RemoteInteractive (RDP) | High |
| Auth Protocol | NTLM — weak, relay-attack risk | High |
| Post-compromise activity | None detected via Sysmon | Low |
| Attacker IP | 192.168.56.10 | Confirmed malicious |
| Target account | socuser | Compromised |

<br><br>

## Investigation Workflow

```
Alert Received
     ↓
Splunk SPL Query — EventCode 4625 count by host
     ↓
Threshold exceeded — CRITICAL (count > 50)
     ↓
Source IP identified — 192.168.56.10
     ↓
Attack success confirmed — EventCode 4624 Logon Type 10
     ↓
Post-compromise hunt — Sysmon EventCode 1
     ↓
No malicious process found
     ↓
Containment initiated
     ↓
Escalation to L2
```

<br><br>

## Classification

| Field | Value |
|---|---|
| Alert Type | True Positive |
| Attack Type | RDP Brute Force |
| MITRE Technique | T1110.001 — Password Guessing |
| Severity | P2 — High |
| Containment | Successful |
| Data Exfiltration | Not detected |
