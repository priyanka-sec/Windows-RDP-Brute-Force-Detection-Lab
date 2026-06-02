# 🔍 Splunk Detection Rules — Windows RDP Brute Force Investigation

This document contains the Splunk detection logic used during the investigation of a Windows RDP brute force attack against Windows Server 2022.

These detection rules were created and validated using real telemetry collected from a controlled SOC lab environment.

<br><br>

## 🎯 Detection Goals

The objective of these detections is to identify:

✅ Failed authentication activity

✅ Brute force behavior

✅ Successful compromise confirmation

✅ Authentication spikes and attack patterns

✅ Post-compromise attacker activity

✅ Escalation points within SOC workflow

<br><br>

# 🚨 Detection Rule 1 — Failed RDP Authentication Detection

## 🎯 Objective

Identify failed Windows authentication attempts generated during RDP login failures.

## SPL Query

```splunk
index=main EventCode=4625
| table _time host Account_Name Source_Network_Address Logon_Type Failure_Reason
| sort -_time
```

## 🧠 Detection Logic

- EventCode 4625 represents failed authentication attempts
- Multiple repeated failures may indicate password guessing activity
- Source IP helps identify attacker origin
- Logon Type helps identify authentication method

## 🔬 SOC Analyst Investigation Steps

1. Review source IP address

2. Identify targeted accounts

3. Verify authentication method

4. Determine authentication failure reason

5. Check if failures are isolated or repeated

## ✅ Expected Outcome

- Failed authentication visibility

- Source IP identification

- Target account visibility

- Authentication failure analysis

<br><br>

# 🔥 Detection Rule 2 — Brute Force Threshold Detection

## 🎯 Objective

Detect excessive failed authentication attempts indicating brute force activity.

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

## 🧠 Detection Logic

- Count authentication failures per host

- Apply severity classification

- Filter suspicious authentication volume

- Prioritize analyst investigation

## 🚦 Severity Thresholds

| Failed Attempts | Severity | SOC Action |
|---|---|---|
| >50 | 🔴 CRITICAL | Immediate escalation |
| >20 | 🟠 HIGH | Analyst investigation |
| ≤20 | 🟡 MEDIUM | Monitor |

## 🔬 SOC Investigation Workflow

1. Identify affected host

2. Review authentication volume

3. Check targeted accounts

4. Correlate with successful logins

5. Escalate if threshold exceeded

## ✅ Expected Outcome

- Rapid brute force identification

- Automatic severity classification

- Reduced triage time

- Faster escalation decisions

<br><br>

# ✅ Detection Rule 3 — Successful Authentication Confirmation

## 🎯 Objective

Identify successful authentication occurring after multiple failures.

## SPL Query

```splunk
index=main (EventCode=4624 OR EventCode=4625)
| eval event_type=if(EventCode=4624,"SUCCESS","FAILURE")
| table _time event_type Account_Name Source_Network_Address
| sort _time
```

## 🧠 Detection Logic

- Correlate failures with successful authentication

- Identify success-after-failure patterns

- Confirm compromise

- Validate true positives

## 🔬 SOC Investigation Workflow

1. Review authentication timeline

2. Identify repeated failures

3. Locate successful login event

4. Validate source IP consistency

5. Escalate confirmed compromise

## ✅ Expected Outcome

- True Positive confirmation

- Breach validation

- Faster containment decisions

- Reduced false positives

<br><br>

# 📈 Detection Rule 4 — Authentication Spike Visualization

## 🎯 Objective

Visualize attack patterns during brute force activity.

## SPL Query

```splunk
index=main EventCode=4625
| timechart span=1m count
```

## 🧠 Detection Logic

- Groups authentication events over time

- Creates attack timeline

- Identifies authentication bursts

- Improves incident reconstruction

## 🔬 SOC Analyst Usage

- Identify attack start time

- Identify attack duration

- Measure attack intensity

- Build incident timeline

## ✅ Expected Outcome

- Clear attack visualization

- Faster timeline creation

- Improved triage efficiency

<br><br>

# 🕵️ Detection Rule 5 — Post Compromise Process Hunting

## 🎯 Objective

Identify suspicious activity occurring after successful compromise.

## SPL Query

```splunk
index=main sourcetype="WinEventLog:Sysmon" EventCode=1
| table _time User Image CommandLine ParentImage
| sort -_time
```

## 🧠 Detection Logic

- Search process creation events

- Identify suspicious commands

- Hunt persistence attempts

- Detect malware execution

## 🔬 SOC Investigation Workflow

1. Identify process creation after login

2. Review command line arguments

3. Validate parent process

4. Search suspicious binaries

5. Determine attacker activity

## ✅ Expected Outcome

- Post-compromise visibility

- Persistence hunting

- Malware detection

- Improved incident response

<br><br>

# ⚔️ Detection Coverage Summary

| Detection Rule | Event IDs | Purpose | ATT&CK Mapping |
|---|---|---|---|
| Failed Authentication Detection | 4625 | Detect login failures | T1110 |
| Brute Force Detection | 4625 | Detect password guessing | T1110.001 |
| Success Correlation | 4624 + 4625 | Confirm compromise | T1078 |
| Authentication Spike Detection | 4625 | Visualize attack activity | T1110 |
| Post Compromise Hunting | Sysmon 1 | Investigate attacker activity | Multiple |

<br><br>

# 🧪 Validation Environment

These detection rules were validated using:

✅ Kali Linux attacker machine

✅ Windows Server 2022 target

✅ Splunk Enterprise SIEM

✅ Sysmon telemetry

✅ Splunk Universal Forwarder

✅ Controlled brute force simulation

<br><br>

# 🧠 Detection Engineering Lessons Learned

✅ Authentication logs require correlation for accurate detection

✅ Successful logins after failures require immediate investigation

✅ Time-based aggregation significantly improves brute force detection

✅ Sysmon improves post-compromise visibility

✅ Detection engineering requires context — not only queries

✅ Effective detections reduce analyst workload and triage time

<br><br>

# 📌 Incident Reference

**Incident ID:** INC-RDP-2026-001

**Classification:** Confirmed RDP Brute Force Attack

**Severity:** High

**Status:** Contained

<br><br>

**👩‍💻 Analyst:** Priyanka Rane | SOC Analyst L1

**📅 Date:** 26 May 2026

**🛡️ Detection Status:** Validated in Lab Environment
