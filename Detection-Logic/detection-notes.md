# Detection Notes
## Windows RDP Brute Force — SOC Detection Analysis

**Analyst:** Priyanka Rane — SOC L1
**Date:** 26 May 2026
**Incident ID:** INC-RDP-2026-001

<br><br>

## Detection Approach

This investigation used a layered detection approach combining
Windows Security Event Logs, Sysmon telemetry, and Splunk Enterprise
SIEM to detect, correlate, and classify the RDP brute force attack.

<br><br>

## Detection Layer 1 — Windows Security Event Logs

### Primary EventCodes Monitored

| EventCode | Description | Detection Value |
|<br><br>|<br><br>|<br><br>|
| 4625 | Failed logon attempt | Primary brute force indicator |
| 4624 | Successful logon | Confirms attack success — breach indicator |
| 4776 | NTLM credential validation | Authentication protocol weakness |
| 4672 | Special privileges assigned | Privilege escalation indicator |

### Key Detection Logic
A single source IP generating more than 5 EventCode 4625
events within a 5-minute window was classified as suspicious.
More than 20 events = HIGH. More than 50 events = CRITICAL.

<br><br>

## Detection Layer 2 — Sysmon Telemetry

### Sysmon EventCodes Used

| EventCode | Description | What It Detected |
|<br><br>|<br><br>|<br><br>|
| 1 | Process Creation | Post-compromise process execution hunting |
| 3 | Network Connection | Attacker IP network activity |
| 10 | Process Access | LSASS access — credential dumping detection |

### Sysmon Finding
No malicious post-compromise process execution was identified
after the successful RDP login. EventCode 1 search returned
no suspicious CommandLine activity from the attacker session.

<br><br>

## Detection Layer 3 — Splunk SIEM Correlation

### Detection Rules Applied

**Rule 1 — Brute Force Threshold Alert:**
Trigger when single IP exceeds 5 failed logins in 5 minutes.
Severity automatically classified as MEDIUM / HIGH / CRITICAL.

**Rule 2 — Attack Success Confirmation:**
Correlate 4625 failures with subsequent 4624 success from
same source IP — confirms true positive breach.

**Rule 3 — Post-Compromise Hunt:**
Search Sysmon EventCode 1 after confirmed 4624 timestamp
to identify any malicious process execution by attacker.

<br><br>

## True Positive Classification

This alert was classified as **TRUE POSITIVE** based on:

1. Source IP 192.168.56.10 generated 142 EventCode 4625 events
2. All attempts targeted the same account — `socuser`
3. Successful EventCode 4624 login confirmed from same source IP
4. Logon Type 10 confirmed RDP session — not local login
5. Attack pattern consistent with MITRE T1110.001

<br><br>

## False Positive Considerations

| Scenario | Ruled Out Because |
|<br><br>|<br><br>|
| Admin testing RDP | No change request or maintenance window active |
| Vulnerability scanner | Scanner IPs are whitelisted — 192.168.56.10 is not |
| User forgot password | 142 attempts in 2 minutes — impossible for human user |
