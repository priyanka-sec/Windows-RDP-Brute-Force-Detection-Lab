# 🔍 Splunk Detection Rules — Windows RDP Brute Force Investigation

This document contains the Splunk detection logic used during the investigation of a Windows RDP brute force attack against Windows Server 2022.

The purpose of these detections is to identify:

✅ Failed authentication activity  
✅ Brute force patterns  
✅ Successful compromise confirmation  
✅ Authentication spikes  
✅ Post-compromise activity hunting  

<br><br>

# 🚨 Detection Rule 1 — Failed RDP Authentication Detection

## Objective

Identify Windows failed authentication attempts generated during RDP login failures.

## SPL Query

```splunk
index=main EventCode=4625
| table _time host Account_Name Source_Network_Address Logon_Type Failure_Reason
| sort -_time
```

## Detection Logic

- EventCode 4625 represents failed logon attempts
- Multiple occurrences may indicate password guessing
- Logon Type helps determine authentication source

## Expected Outcome

- Failed authentication events visible
- Source IP identification
- Target account identification
- Authentication failure reason visibility

<br><br>

# 🔥 Detection Rule 2 — Brute Force Threshold Detection

## Objective

Detect excessive authentication failures indicating brute force behavior.

## SPL Query

```splunk
index=main EventCode=4625
| stats count as failed_attempts by host
| eval severity=case(
failed_attempts>50,"CRITICAL",
failed_attempts>20,"HIGH",
true(),"MEDIUM")
| where failed_attempts>20
| sort -failed_attempts
```

## Detection Logic

- Count failed logins
- Apply severity thresholds
- Filter suspicious authentication volume

## Severity Thresholds

| Failed Attempts | Severity |
|---|---|
| >50 | 🔴 CRITICAL |
| >20 | 🟠 HIGH |
| ≤20 | 🟡 MEDIUM |

## Expected Outcome

- Rapid identification of brute force activity
- Automatic severity classification
- Reduced analyst triage time

<br><br>

# ✅ Detection Rule 3 — Successful Authentication Confirmation

## Objective

Identify successful login occurring after multiple authentication failures.

## SPL Query

```splunk
index=main (EventCode=4624 OR EventCode=4625)
| eval event_type=if(EventCode=4624,"SUCCESS","FAILURE")
| table _time event_type Account_Name Source_Network_Address
| sort _time
```

## Detection Logic

- Correlate failures and successes
- Detect success-after-failure patterns
- Confirm compromise

## Expected Outcome

- True Positive confirmation
- Breach validation
- Faster escalation decision making

<br><br>

# 📈 Detection Rule 4 — Authentication Spike Visualization

## Objective

Visualize authentication spikes during attack windows.

## SPL Query

```splunk
index=main EventCode=4625
| timechart span=1m count
```

## Detection Logic

- Groups failed logins by time window
- Creates visual attack timeline
- Identifies attack bursts

## Expected Outcome

- Clear visualization of attack intensity
- Easier incident timeline creation
- Faster SOC triage

<br><br>

# 🕵️ Detection Rule 5 — Post Compromise Process Hunting

## Objective

Identify suspicious activity occurring after successful compromise.

## SPL Query

```splunk
index=main sourcetype="WinEventLog:Sysmon" EventCode=1
| table _time User Image CommandLine ParentImage
| sort -_time
```

## Detection Logic

- Search process creation events
- Review commands executed after login
- Hunt persistence or malware execution

## Expected Outcome

- Process visibility after breach
- Persistence hunting
- Attacker activity validation

<br><br>

# ⚔️ Detection Coverage Summary

| Detection | Event IDs | Purpose |
|---|---|---|
| Failed Authentication | 4625 | Detect login failures |
| Brute Force Detection | 4625 | Detect password guessing |
| Successful Login Correlation | 4624 + 4625 | Confirm compromise |
| Authentication Spike Detection | 4625 | Visualize attack activity |
| Post Compromise Hunting | Sysmon 1 | Investigate attacker actions |

<br><br>

# 🧠 Analyst Notes

These detection rules were tested in a controlled SOC lab environment using:

- Kali Linux attacker machine
- Windows Server 2022 target
- Splunk Enterprise SIEM
- Sysmon telemetry
- Splunk Universal Forwarder

These rules were used to investigate and confirm a successful Windows RDP brute force attack and produce incident report:

**INC-RDP-2026-001**

<br><br>

# 🛡️ Detection Engineering Lessons Learned

✅ Authentication logs alone are insufficient without correlation

✅ Successful logins after failures require immediate investigation

✅ Sysmon significantly improves post-compromise visibility

✅ Time-based aggregation dramatically improves brute force detection

✅ Detection engineering is not only query writing — context matters

<br><br>

**Analyst:** Priyanka Rane | SOC Analyst L1
**Date:** 26 May 2026
