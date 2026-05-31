# SOC Investigation Report
## INC-RDP-2026-001 — Windows RDP Brute Force Attack

| Field | Details |
|---|---|
| **Report ID** | INC-RDP-2026-001 |
| **Date** | 26 May 2026 |
| **Analyst** | Priyanka Rane — SOC Analyst L1 |
| **Severity** | P2 — High |
| **Status** | Contained |
| **Classification** | True Positive |
| **Attack Type** | RDP Brute Force — Password Guessing |
| **MITRE Techniques** | T1110.001 \| T1021.001 \| T1078.003 |

<br><br>

## 1. Executive Summary

On 26 May 2026, a brute force attack was detected against
Windows Server 2022 (192.168.56.110) via RDP port 3389.
The attacker machine (192.168.56.10 — Kali Linux) generated
multiple failed authentication attempts against the `socuser`
account between 21:09:06 PM and 21:09:57 PM IST.

A successful RDP login was confirmed via EventCode 4624
at 21:09:57 PM with Logon Type 10 — confirming the attacker
established a live Remote Desktop session on the server.

The attack was detected in Splunk Enterprise using a
count-based SPL threshold alert showing 76 CRITICAL events.
Containment was completed after confirmed breach detection.
No post-compromise activity was detected via Sysmon analysis.

<br><br>

## 2. Detection Method

### Primary Detection — Windows Event Viewer

Failed authentication events were first identified in
Windows Security Event Log:

| EventCode | Time | Details |
|---|---|---|
| 4625 | 5/26/2026 9:09:06 PM | Failed login — socuser — Source: 192.168.56.10 — NTLM |
| 4625 | 5/26/2026 9:09:07 PM | Failed login — socuser — Source: 192.168.56.10 — NTLM |
| 4624 | 5/26/2026 9:09:57 PM | Successful login — socuser — Logon Type 10 — RDP |

### Secondary Detection — Splunk SIEM

```splunk
index=main EventCode=4625
| stats count by host
| eval severity=if(count>50,"CRITICAL",if(count>20,"HIGH","MEDIUM"))
| sort -count
```

**Result:**
```
host              count    severity
WIN-TLKR5B0U5QP   76       CRITICAL
```

Time range: 26/05/2026 20:24:10 to 20:39:10

### Confirmation — Attack Success Validation

```splunk
index=main (EventCode=4625 OR EventCode=4624)
| eval event_type=if(EventCode=4624,"SUCCESS","FAILURE")
| table _time, event_type, Account_Name
| sort _time
```

**Result:** First event at 2026-05-26 12:08:33 —
chain of FAILURE events followed by SUCCESS —
confirmed True Positive breach.

### Post-Compromise Hunt — Sysmon

```splunk
index=main sourcetype="WinEventLog:Sysmon" EventCode=1
| table _time, User, Image, CommandLine, ParentImage
| sort -_time
```

**Result:** No malicious process execution detected
after confirmed breach timestamp.

<br><br>

## 3. Attack Timeline

| Time (IST) | Phase | Event | EventCode | Source |
|---|---|---|---|---|
| 5/26/2026 9:09:06 PM | Attack Detected | First confirmed failed RDP login — socuser — 192.168.56.10 | 4625 | Windows Security Log |
| 5/26/2026 9:09:07 PM | Attack Continues | Second failed login — same source — NTLM confirmed | 4625 | Windows Security Log |
| 5/26/2026 9:09:57 PM | Breach Confirmed | Successful RDP login — Logon Type 10 — socuser | 4624 | Windows Security Log |
| 26/05/2026 20:24:10 | SIEM Detection | Splunk threshold alert fires — 76 events — CRITICAL | SPL | Splunk Enterprise |
| 26/05/2026 20:24:10 | Investigation | Analyst begins Splunk triage | — | SOC |
| 26/05/2026 20:39:10 | TP Confirmed | Success-after-failure chain confirmed — True Positive | — | Splunk SPL |
| Post-detection | Sysmon Hunt | EventCode 1 searched — no malicious process found | Sysmon 1 | Sysmon Log |
| Post-detection | Containment | Source IP 192.168.56.10 blocked at Windows Firewall | — | SOC Action |
| Post-detection | Containment | socuser account disabled and password reset | — | SOC Action |
| Post-detection | Forensics | Security logs exported as .evtx evidence | — | Event Viewer |
| Post-detection | Escalation | L2 notified — ticket INC-RDP-2026-001 raised | — | Ticket System |

<br><br>

## 4. Indicators of Compromise

| IOC Type | Observed Value | Verdict | Validated Via | Action Taken |
|---|---|---|---|---|
| Source IP | 192.168.56.10 | **MALICIOUS** — confirmed attacker | Windows Security Log + Splunk | Blocked at Windows Firewall |
| Target Account | socuser | **COMPROMISED** — successful login confirmed | EventCode 4624 at 9:09:57 PM — Logon Type 10 | Account disabled + password reset |
| Auth Protocol | NTLM | **WEAK** — relay attack risk | EventCode 4625 — NtLmSsp logon process | Kerberos enforcement recommended |
| Failed Logins | 76 events in 15 minutes | **BRUTE FORCE CONFIRMED** | Splunk count query — CRITICAL threshold | Account lockout policy enforced |
| Successful Login | EventCode 4624 — 9:09:57 PM | **TRUE POSITIVE — BREACH CONFIRMED** | Logon Type 10 — RDP session | Immediate containment initiated |

<br><br>

## 5. MITRE ATT&CK Mapping

| Tactic | Technique | Sub-Technique | ID | Evidence |
|---|---|---|---|---|
| Credential Access | Brute Force | Password Guessing | T1110.001 | Multiple EventCode 4625 — socuser targeted |
| Lateral Movement | Remote Services | Remote Desktop Protocol | T1021.001 | EventCode 4624 — Logon Type 10 confirmed |
| Defense Evasion | Valid Accounts | Local Accounts | T1078.003 | Successful login using valid socuser credentials |

<br><br>

## 6. Containment Actions Taken

| Action | Outcome |
|---|---|
| Blocked source IP 192.168.56.10 at Windows Firewall | No further RDP connections possible |
| Disabled socuser account | Active RDP session terminated |
| Reset socuser password | Compromised credential invalidated |
| Exported Security logs as forensic .evtx | Evidence preserved for L2 review |
| Escalated to L2 analyst | Ticket INC-RDP-2026-001 raised |

### Escalation Decision
Escalated to L2 because EventCode 4624 confirmed a successful
attacker RDP session with Logon Type 10. Post-compromise
investigation was required to rule out lateral movement,
data exfiltration, or persistence mechanisms.

<br><br>

## 7. Post-Compromise Hunt Results

Sysmon EventCode 1 (Process Creation) was searched for all
processes after the confirmed breach at 9:09:57 PM.

**Result: No malicious processes identified.**

No evidence of lateral movement tools, credential dumping,
persistence mechanisms, or data exfiltration activity.

**Incident classified as contained — no post-compromise impact.**

<br><br>

## 8. Root Cause Analysis

| Root Cause | Detail | Recommendation |
|---|---|---|
| No account lockout policy | Unlimited login attempts allowed | Lockout after 5 failures |
| RDP directly exposed | Port 3389 accessible on network | Restrict behind VPN |
| Weak socuser password | Cracked via wordlist attack | Enforce 14-char minimum |
| NTLM authentication active | Vulnerable to relay attacks | Replace with Kerberos |
| No real-time SIEM alerting | Detection was reactive | Deploy Splunk threshold alert |
| No MFA on RDP accounts | Credential alone sufficient | Implement MFA on all RDP |

<br><br>

## 9. Recommendations

| Priority | Recommendation | Expected Outcome |
|---|---|---|
| CRITICAL | Account lockout after 5 failed attempts | Stops brute force attacks entirely |
| CRITICAL | Restrict RDP behind VPN only | Eliminates direct attack surface |
| CRITICAL | Enforce MFA on all RDP accounts | Credential theft becomes useless |
| HIGH | Replace NTLM with Kerberos | Eliminates relay attack risk |
| HIGH | Enforce 14-character minimum password | Increases crack time exponentially |
| MEDIUM | Deploy Splunk real-time alert for 4625 | Detection within 5 minutes of attack |
| MEDIUM | Enable Network Level Authentication | Adds pre-session authentication layer |
| LOW | Disable RDP on non-essential servers | Reduces overall attack surface |

<br><br>

## 10. Lessons Learned

1. **Account lockout policy is non-negotiable.** Multiple
   failed attempts succeeded without any automatic blocking.
   A single Group Policy change stops this attack class.

2. **RDP must never be directly exposed.** Any machine with
   port 3389 accessible on a network is an active target.
   VPN-only access is the minimum acceptable standard.

3. **Splunk detected what manual review would have missed.**
   The count-based threshold query identified 76 CRITICAL
   events instantly across a 15-minute window.

4. **Sysmon provided critical post-compromise clarity.**
   Without Sysmon EventCode 1 analysis, confirming no
   malicious processes ran post-breach would have been
   impossible.

5. **NTLM is a legacy risk.** Every environment should
   migrate to Kerberos-only authentication. NTLM relay
   attacks are well-documented and trivially exploitable.

<br><br>

**Analyst:** Priyanka Rane | SOC Analyst L1
**Date:** 26 May 2026
**Report Status:** Final — Submitted to L2
