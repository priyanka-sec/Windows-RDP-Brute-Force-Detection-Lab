## 🛡️ Mitigation Phase

Demonstrates defensive actions taken to contain the attack.

Includes:
- RDP disablement via PowerShell
- Registry modification
- Security posture improvement

<br><br>

## PowerShell Command
![PowerShell Command](Screenshots/images/disable-rdp-powershell.png)
<br>
### PowerShell command executable to disable RDP access as an immediate containment measure

<br><br>

## Registry Proof Screenshot
![Registry Proof Screenshot](Screenshots/images/rdp-disabled-registry-proof.png)
<br>
- ### Registry path verification showing navigation to Terminal Server Configuration Settings.
- ### Registry value ```fDenyTSConnections = 1``` confirming that Remote Desktop access has been successfully disabled
