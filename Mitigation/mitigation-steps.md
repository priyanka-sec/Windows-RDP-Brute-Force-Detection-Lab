# 🛡️ RDP Attack Mitigation & Hardening Guide

This section documents the defensive measures applied after detecting suspicious RDP activity during the lab.

The goal is to simulate how a Security Operations Center (SOC) analyst responds to unauthorized remote access attempts and applies containment and hardening strategies.

<br><br>

## 🎯 Objectives

- Contain unauthorized access
- Reduce RDP attack surface
- Prevent brute-force attacks
- Strengthen authentication security
- Apply real-world blue team practices

<br><br>

## 🚨 Threat Identified

During log analysis, the following indicators were observed:

- Multiple failed authentication attempts (Event ID 4625)
- Same source IP across failures
- Successful RDP login detected (Event ID 4624)
- Logon Type 10 confirming remote interactive session

<br><br>




# 🛠️ Mitigation Steps Applied

## 1️. Immediate Containment — Disable RDP Access

To stop further unauthorized access, RDP was disabled via registry modification.

### 🔧 Command Used

```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections" -Value 1
```

This PowerShell command disables Remote Desktop (RDP) on the Windows system.

- Set-ItemProperty → Used to change a Windows Registry setting

- Terminal Server registry path → Location where RDP settings are stored

- fDenyTSConnections = 1 → Blocks all Remote Desktop connections

📌 In simple terms:
This command turns OFF RDP access, preventing attackers from connecting remotely.



<br><br>



📌 Conclusion:  
A brute-force or credential-based RDP compromise was successfully simulated.
