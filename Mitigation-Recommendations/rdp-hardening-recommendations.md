# RDP Hardening Recommendations
## INC-RDP-2026-001 — Post-Incident Security Hardening

**Analyst:** Priyanka Rane — SOC Analyst L1
**Date:** 26 May 2026
**Incident Reference:** INC-RDP-2026-001

---

## Overview

Following the confirmed RDP brute force attack against Windows
Server 2022, the following hardening recommendations have been
produced to prevent recurrence and reduce the overall RDP
attack surface.

These recommendations are prioritized by impact and ease
of implementation.

---

## Priority 1 — CRITICAL (Implement Immediately)

### 1.1 Enable Account Lockout Policy

**Why:** The absence of an account lockout policy allowed
142 failed login attempts without any automatic blocking.
One policy change stops this entire attack class.

**How to implement:**

```powershell
# Open Group Policy Editor
gpedit.msc

# Navigate to:
# Computer Configuration → Windows Settings → Security Settings
# → Account Policies → Account Lockout Policy

# Set these values:
# Account lockout threshold: 5 invalid logon attempts
# Account lockout duration: 30 minutes
# Reset account lockout counter after: 15 minutes
```

Or via PowerShell:
```powershell
net accounts /lockoutthreshold:5
net accounts /lockoutduration:30
net accounts /lockoutwindow:15
```

**Expected Outcome:** After 5 failed attempts the account
locks for 30 minutes — making brute force attacks
computationally impractical.

---

### 1.2 Restrict RDP Behind VPN Only

**Why:** RDP exposed directly on any network interface
is one of the highest-risk configurations in Windows
environments. Attackers actively scan for open port 3389.

**How to implement:**

```powershell
# Block RDP from all sources except VPN subnet
New-NetFirewallRule -DisplayName "Block-RDP-External" `
  -Direction Inbound `
  -Protocol TCP `
  -LocalPort 3389 `
  -Action Block

New-NetFirewallRule -DisplayName "Allow-RDP-VPN-Only" `
  -Direction Inbound `
  -Protocol TCP `
  -LocalPort 3389 `
  -RemoteAddress "10.0.0.0/8" `
  -Action Allow
```

**Expected Outcome:** RDP is only reachable through the
VPN tunnel — attackers cannot reach port 3389 at all
without first authenticating to the VPN.

---

### 1.3 Enable Multi-Factor Authentication on RDP

**Why:** Even if an attacker obtains valid credentials,
MFA prevents login without the second factor.

**How to implement:**
- Deploy Microsoft Authenticator via Azure AD Conditional Access
- Or deploy Duo Security MFA for Windows RDP
- Or use Windows Hello for Business with PIN + biometric

**Expected Outcome:** Stolen or brute-forced credentials
become useless without the second factor.

---

## Priority 2 — HIGH (Implement Within 48 Hours)

### 2.1 Replace NTLM with Kerberos Authentication

**Why:** NTLM authentication observed in this attack
(EventCode 4776) is vulnerable to:
- Pass-the-Hash attacks
- NTLM relay attacks
- Credential capture via Responder

**How to implement:**

```powershell
# Disable NTLM authentication via Group Policy
# Computer Configuration → Windows Settings → Security Settings
# → Local Policies → Security Options

# Set: Network security: Restrict NTLM:
# Incoming NTLM traffic → Deny all accounts
# Outgoing NTLM traffic to remote servers → Deny all

# Or via registry:
Set-ItemProperty -Path `
"HKLM:\SYSTEM\CurrentControlSet\Control\Lso\MSV1_0" `
-Name "RestrictSendingNTLMTraffic" -Value 2
```

**Expected Outcome:** All authentication uses Kerberos —
eliminates NTLM-based attack vectors entirely.

---

### 2.2 Enforce Strong Password Policy

**Why:** The socuser account password was cracked within
8 minutes using a common wordlist. Stronger passwords
increase brute force time from minutes to years.

**How to implement:**

```powershell
# Set minimum password length to 14 characters
net accounts /minpwlen:14

# Enable password complexity requirements via Group Policy
# Computer Configuration → Windows Settings → Security Settings
# → Account Policies → Password Policy
# Password must meet complexity requirements: Enabled
# Minimum password length: 14
# Maximum password age: 90 days
# Password history: 10 passwords
```

**Expected Outcome:** 14-character complex passwords
make dictionary and brute force attacks computationally
infeasible with standard hardware.

---

### 2.3 Change Default RDP Port

**Why:** Attackers scan specifically for port 3389.
Changing the port eliminates automated scanner hits.

**Note:** This is security through obscurity and should
never replace proper controls — use in addition to them.

```powershell
# Change RDP port from 3389 to custom port (e.g. 54321)
Set-ItemProperty -Path `
"HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" `
-Name "PortNumber" -Value 54321

# Update firewall rule for new port
New-NetFirewallRule -DisplayName "Allow-RDP-Custom-Port" `
  -Direction Inbound `
  -Protocol TCP `
  -LocalPort 54321 `
  -Action Allow
```

---

## Priority 3 — MEDIUM (Implement Within 1 Week)

### 3.1 Deploy SIEM Alerting for EventCode 4625

**Why:** This attack was detected reactively. Real-time
alerting would have fired within the first minute.

**Splunk Alert Rule:**

```splunk
index=main EventCode=4625
| bucket _time span=5m
| stats count by _time, host
| where count > 5
| eval severity=if(count>50,"CRITICAL",if(count>20,"HIGH","MEDIUM"))
```

Set this as a **Scheduled Alert** in Splunk:
- Run every 5 minutes
- Trigger if results > 0
- Send email or webhook notification to SOC team

**Expected Outcome:** SOC analyst is notified within
5 minutes of attack start — not after the fact.

---

### 3.2 Enable Network Level Authentication (NLA)

**Why:** NLA requires users to authenticate before
establishing a full RDP session — reducing server load
and adding a pre-session authentication layer.

```powershell
# Enable NLA via PowerShell
Set-ItemProperty -Path `
"HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" `
-Name "UserAuthentication" -Value 1
```

---

### 3.3 Restrict RDP Access to Specific Admin Accounts Only

**Why:** General user accounts like `socuser` should
never have RDP access in production environments.
RDP should be limited to named administrator accounts only.

```powershell
# Remove socuser from Remote Desktop Users group
Remove-LocalGroupMember -Group "Remote Desktop Users" `
  -Member "socuser"

# Add only specific admin accounts
Add-LocalGroupMember -Group "Remote Desktop Users" `
  -Member "rdp-admin-01"
```

---

## Priority 4 — LOW (Implement Within 1 Month)

### 4.1 Disable RDP on Non-Essential Servers

```powershell
# Disable RDP completely on servers that don't need it
Set-ItemProperty -Path `
"HKLM:\System\CurrentControlSet\Control\Terminal Server" `
-Name "fDenyTSConnections" -Value 1
```

### 4.2 Enable RDP Session Timeout Policies

```powershell
# Disconnect idle sessions after 15 minutes
# End disconnected sessions after 30 minutes
# Via Group Policy:
# Computer Configuration → Administrative Templates
# → Windows Components → Remote Desktop Services
# → Remote Desktop Session Host → Session Time Limits
```

### 4.3 Deploy Windows Defender Credential Guard

Prevents credential theft even if attacker gains access.
Isolates LSASS credentials from direct memory access —
blocks Mimikatz-style attacks post-compromise.

---

## Hardening Checklist Summary

| Control | Priority | Status |
|---|---|---|
| Account lockout policy (5 attempts) | CRITICAL | ⬜ Pending |
| RDP behind VPN only | CRITICAL | ⬜ Pending |
| MFA on all RDP accounts | CRITICAL | ⬜ Pending |
| Replace NTLM with Kerberos | HIGH | ⬜ Pending |
| 14-character password minimum | HIGH | ⬜ Pending |
| Change default RDP port | HIGH | ⬜ Pending |
| Splunk real-time alert for 4625 | MEDIUM | ⬜ Pending |
| Enable NLA | MEDIUM | ⬜ Pending |
| Restrict RDP to admin accounts | MEDIUM | ⬜ Pending |
| Disable RDP on non-essential servers | LOW | ⬜ Pending |
| Session timeout policies | LOW | ⬜ Pending |
| Deploy Credential Guard | LOW | ⬜ Pending |

---

## References

- [MITRE ATT&CK T1110.001 — Brute Force](https://attack.mitre.org/techniques/T1110/001/)
- [MITRE ATT&CK T1021.001 — Remote Desktop Protocol](https://attack.mitre.org/techniques/T1021/001/)
- [Microsoft RDP Security Best Practices](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/rds-plan-secure-data-storage)
- [CIS Benchmark — Windows Server 2022](https://www.cisecurity.org/benchmark/microsoft_windows_server)
- [NIST SP 800-46 — Remote Access Guide](https://csrc.nist.gov/publications/detail/sp/800-46/rev-2/final)

---

**Analyst:** Priyanka Rane | SOC Analyst L1
**Date:** 26 May 2026
