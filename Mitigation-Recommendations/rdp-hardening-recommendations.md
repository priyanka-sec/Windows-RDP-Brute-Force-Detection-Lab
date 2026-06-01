# 🛡️ RDP Hardening Recommendations

## INC-RDP-2026-001 — Post-Incident Security Hardening

**👩‍💻 Analyst:** Priyanka Rane — SOC Analyst L1
**📅 Date:** 26 May 2026
**🎫 Incident Reference:** INC-RDP-2026-001

---

# 📌 Overview

Following the confirmed RDP brute force attack against Windows Server 2022, the following security hardening recommendations were produced to reduce attack surface, improve authentication security, strengthen monitoring, and prevent similar attacks.

Recommendations are prioritized by security impact and implementation effort.

---

# 🚨 Priority 1 — CRITICAL (Implement Immediately)

## 🔐 1.1 Enable Account Lockout Policy

### Why

The absence of an account lockout policy allowed hundreds of failed authentication attempts without automatic blocking mechanisms.

Implementing lockout policies significantly reduces brute force effectiveness.

### How to Implement

```powershell
gpedit.msc

# Navigate:

# Computer Configuration
# → Windows Settings
# → Security Settings
# → Account Policies
# → Account Lockout Policy

# Configure:

# Account lockout threshold = 5
# Account lockout duration = 30 minutes
# Reset counter after = 15 minutes
```

Or:

```powershell
net accounts /lockoutthreshold:5

net accounts /lockoutduration:30

net accounts /lockoutwindow:15
```

### Expected Outcome

✅ Accounts automatically lock after repeated failures

✅ Brute force attacks become significantly more difficult

---

## 🔒 1.2 Restrict RDP Behind VPN Only

### Why

RDP exposed directly to networks significantly increases attack surface.

Restricting access behind VPN reduces exposure.

### How to Implement

```powershell
New-NetFirewallRule `
-DisplayName "Block-RDP-External" `
-Direction Inbound `
-Protocol TCP `
-LocalPort 3389 `
-Action Block


New-NetFirewallRule `
-DisplayName "Allow-RDP-VPN-Only" `
-Direction Inbound `
-Protocol TCP `
-LocalPort 3389 `
-RemoteAddress "10.0.0.0/8" `
-Action Allow
```

### Expected Outcome

✅ Attackers cannot directly reach RDP

✅ Authentication occurs only after VPN access

---

## 🔑 1.3 Enable Multi-Factor Authentication (MFA)

### Why

Passwords alone are insufficient protection.

MFA protects even if credentials are stolen or guessed.

### Implementation Options

* Microsoft Authenticator
* Duo MFA for Windows RDP
* Windows Hello for Business

### Expected Outcome

✅ Compromised passwords alone cannot provide access

---

# ⚠️ Priority 2 — HIGH (Implement Within 48 Hours)

## 🏢 2.1 Reduce or Eliminate NTLM Usage

### Why

NTLM observed during investigation increases risk of:

* Pass-the-Hash attacks
* NTLM relay attacks
* Credential theft

### How to Implement

```powershell
Set-ItemProperty `
-Path "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa\MSV1_0" `
-Name "RestrictSendingNTLMTraffic" `
-Value 2
```

Group Policy:

```text
Computer Configuration

→ Windows Settings

→ Security Settings

→ Local Policies

→ Security Options
```

Configure:

```text
Network Security:

Restrict NTLM

Incoming Traffic

Outgoing Traffic
```

### Expected Outcome

✅ Reduced exposure to legacy authentication attacks

---

## 🔑 2.2 Enforce Strong Password Policy

### Why

Weak passwords increase brute force success probability.

### How to Implement

```powershell
net accounts /minpwlen:14
```

Configure:

```text
Password Complexity = Enabled

Minimum Length = 14

Password History = 10

Maximum Age = 90 Days
```

### Expected Outcome

✅ Stronger resistance against password guessing attacks

---

## 🛡️ 2.3 Restrict RDP Using Firewall Allowlisting

### Why

Reducing attack surface is more effective than changing ports.

### How to Implement

```powershell
New-NetFirewallRule `
-DisplayName "Allow-RDP-Admin-Subnet" `
-Direction Inbound `
-Protocol TCP `
-LocalPort 3389 `
-RemoteAddress "192.168.56.0/24" `
-Action Allow
```

### Expected Outcome

✅ Only approved networks can reach RDP

---

# 🟡 Priority 3 — MEDIUM (Implement Within One Week)

## 📊 3.1 Deploy Splunk Real-Time Alerting

### Why

Reactive detection increases attacker dwell time.

### Splunk Query

```splunk
index=main EventCode=4625

| bucket _time span=5m

| stats count as failed_attempts by _time host

| where failed_attempts > 20

| eval severity=case(

failed_attempts>50,"CRITICAL",

failed_attempts>20,"HIGH",

true(),"MEDIUM")
```

### Configure Alert

* Schedule every 5 minutes
* Trigger when results >0
* Send email/webhook

### Expected Outcome

✅ SOC receives faster notification

---

## 🖥️ 3.2 Enable Network Level Authentication (NLA)

### Why

Authentication occurs before establishing full session.

### Implementation

```powershell
Set-ItemProperty `
-Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" `
-Name "UserAuthentication" `
-Value 1
```

### Expected Outcome

✅ Reduced attack surface

---

## 👤 3.3 Restrict RDP to Admin Accounts Only

### Why

General user accounts should not possess RDP permissions.

### Implementation

```powershell
Remove-LocalGroupMember `
-Group "Remote Desktop Users" `
-Member "socuser"


Add-LocalGroupMember `
-Group "Remote Desktop Users" `
-Member "rdp-admin-01"
```

### Expected Outcome

✅ Smaller attack surface

---

## 📁 3.4 Enable Security Log Retention

### Why

Log overwriting reduces investigation capability.

### Implementation

Increase:

* Security Event Log Size
* Retention Policies
* Log Backup Frequency

### Expected Outcome

✅ Longer forensic visibility

---

# 🟢 Priority 4 — LOW (Implement Within One Month)

## 🚫 4.1 Disable RDP on Non-Essential Systems

```powershell
Set-ItemProperty `
-Path "HKLM:\System\CurrentControlSet\Control\Terminal Server" `
-Name "fDenyTSConnections" `
-Value 1
```

---

## ⏱️ 4.2 Configure Session Timeouts

Configure:

```text
Disconnect Idle Sessions

Terminate Disconnected Sessions

Remote Desktop Session Limits
```

### Expected Outcome

✅ Reduced exposure from abandoned sessions

---

## 🛡️ 4.3 Deploy Credential Guard

### Why

Credential Guard isolates secrets from attackers.

### Expected Outcome

✅ Reduced credential dumping risk

---

# ✅ Hardening Checklist Summary

| Control                   | Priority | Status    |
| ------------------------- | -------- | --------- |
| Account Lockout Policy    | CRITICAL | ⬜ Pending |
| Restrict RDP Behind VPN   | CRITICAL | ⬜ Pending |
| MFA on RDP Accounts       | CRITICAL | ⬜ Pending |
| Reduce NTLM Usage         | HIGH     | ⬜ Pending |
| Strong Password Policy    | HIGH     | ⬜ Pending |
| Firewall Allowlisting     | HIGH     | ⬜ Pending |
| Splunk Real-Time Alerting | MEDIUM   | ⬜ Pending |
| Enable NLA                | MEDIUM   | ⬜ Pending |
| Restrict RDP Accounts     | MEDIUM   | ⬜ Pending |
| Security Log Retention    | MEDIUM   | ⬜ Pending |
| Disable Unnecessary RDP   | LOW      | ⬜ Pending |
| Session Timeout Policies  | LOW      | ⬜ Pending |
| Credential Guard          | LOW      | ⬜ Pending |

---

# 📚 References

* MITRE ATT&CK T1110.001 — Brute Force
* MITRE ATT&CK T1021.001 — Remote Desktop Protocol
* Microsoft RDP Security Guidance
* CIS Benchmark Windows Server
* NIST SP 800-46 Remote Access Guide

---

**👩‍💻 Analyst:** Priyanka Rane | SOC Analyst L1

**📅 Date:** 26 May 2026
