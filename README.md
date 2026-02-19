<h1>Windows RDP Attack Detection Lab</h1>
> 🔐 A Hands-On Blue Team Lab for RDP Attack Detection & Mitigation


<br><br>

## 🧠 Skills Demonstrated

- Windows Security Log Analysis
- RDP Attack Simulation
- Event Viewer Monitoring
- Timeline Analysis
- Incident Response Basics
- PowerShell Security Commands

<br><br>


## 📘 Introduction

This project demonstrates a hands-on Windows Remote Desktop Protocol (RDP) attack lab. 
It provides a safe virtual environment to understand how attackers attempt to gain access to Windows systems, and how defenders detect, mitigate, and monitor these attacks.  

This lab is designed for **cybersecurity students, SOC analysts, incident responders, and ethical hackers** to gain hands-on experience in monitoring, mitigation, and forensic analysis.
<br><br>



## 🔍 Overview

The lab simulates controlled brute-force and credential-based attack scenarios to analyze authentication logs and defensive responses.

The lab uses **Kali Linux as the attacker machine** and **Windows Server 2022 as the victim machine**.  
It focuses on **RDP attack attempts**, examining **Event Viewer logs**, applying **mitigation techniques**, and learning **SOC monitoring procedures**.

Key aspects of the lab include:

- Understanding Event IDs **4624 (successful login)** and **4625 (failed login)**  
- Performing controlled RDP attacks in a virtual environment  
- Practicing **timeline analysis** and **mitigation steps**  
- Learning **SOC and incident response workflows**
<br><br>



## 🎯 Objectives

- Learn how Windows Server 2022 logs successful and failed login attempts  
- Understand Event IDs 4624 and 4625 and their meaning  
- Learn to **detect suspicious login attempts**  
- Apply **mitigation techniques** for RDP vulnerabilities  
- Gain **hands-on experience** with Event Viewer, SOC monitoring, and incident response  
- Understand how attackers hide their activity and how to track them  
<br><br>



## ⚔️ Attack Scenario

In this lab, the attacker attempts multiple RDP login attempts against the Windows Server 2022 machine using valid and invalid credentials.

The objective of this lab is to:
- Generate failed login events (Event ID 4625)
- Observe successful authentication (Event ID 4624)
- Analyze logon type 10 (Remote Interactive)
- Identify suspicious login patterns
<br><br>



 

## 🛠️ Lab Setup / How to Run

Follow these steps to set up and run the lab safely:

### 1. Virtual Environment

- Install **VirtualBox** or **VMware** on your host machine  
- Create the following virtual machines:
  - **Kali Linux VM (Attacker)**
  - **Windows Server 2022 VM (Victim)**

<br><br>

### 2. Windows Server 2022 Configuration

#### 1.1 Enable **Remote Desktop Protocol (RDP)**:
- Open PowerShell as Administrator
```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections" -Value 0
```

- **Set-ItemProperty** → Modifies a Windows Registry value.

- **HKLM:\System\CurrentControlSet\Control\Terminal Server** → Registry location storing RDP settings.

- **fDenyTSConnections** → Controls whether Remote Desktop is allowed.

- **Value meanings:**
  - 0 = Allow RDP (Enable)
  - 1 = Deny RDP (Disable)



#### 2.2 Enable Firewall Rule for RDP (If Blocked)

```powershell
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
```

- Ensures that Windows Firewall allows RDP traffic so your Kali VM can connect to the victim VM.


<br><br>



### 3. Kali Linux Configuration (Attacker)

#### 1. Ensure both VMs are connected to the same virtual network (Host-only or Internal Network in VirtualBox).

#### 2. Install RDP client tools if not already installed:
```bash
sudo apt update
sudo apt install freerdp2-x11
```

#### 3. Test RDP connection using:
```bash
xfreerdp /v:<Windows-VM-IP> /u:Administrator /p:<Password>
```
- Replace <Windows-VM-IP> with the victim VM’s IP and <Password> with the Administrator password.
<br>

### 4. Verification and Monitoring
#### 1. Open **Event Viewer** on the Windows Server 2022 VM.  
#### 2. Navigate to:  
   **Windows Logs → Security**

#### 3. Look for the following events:
- **Event ID 4624 → Successful Login**
  - Logon Type 10 = Remote Interactive (RDP)

- **Event ID 4625 → Failed Login**
  - Logon Type 10 = Failed RDP attempt



## 🧰 Tools Used

- Kali Linux (Attacker Machine)
- Windows Server 2022 (Victim Machine)
- VirtualBox / VMware
- Windows Event Viewer
- PowerShell
- FreeRDP (RDP Client)





## ⚠️ Disclaimer

This lab is created for **educational purposes only**.  
All activities were performed in an **isolated virtual environment**.

Do NOT perform these techniques on real systems without proper authorization.




## 🚀 Future Improvements

- Add SIEM integration (Splunk / ELK)
- Automated log parsing scripts
- Detection rules for brute-force attacks
- Blue team playbook creation



<br><br>

⭐ If you found this project helpful, feel free to connect or provide feedback.
