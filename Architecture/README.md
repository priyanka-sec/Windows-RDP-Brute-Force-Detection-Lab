# 🏗️ Architecture — Lab Design & Environment Configuration
## Windows RDP Brute Force Detection & SIEM Investigation Lab

<br><br>

## 📁 Folder Contents

| File | Description |
|---|---|
| README.md | Complete lab architecture documentation |

<br><br>

## 📌 Purpose

This folder documents the complete lab architecture designed
to simulate a real-world RDP brute force attack and SOC
detection workflow.

The environment was built to demonstrate:

- Real attack traffic generation from Kali Linux
- Live log forwarding to Splunk Enterprise SIEM
- Deep endpoint visibility via Sysmon
- SOC analyst detection and response workflow
- End-to-end incident investigation from attack to containment

<br><br>

## 🖥️ Environment Configuration

| Component | Details |
|---|---|
| Attacker Machine | Kali Linux — 192.168.56.10 |
| Victim Machine | Windows Server 2022 — 192.168.56.110 |
| SOC / SIEM | Splunk Enterprise — Host Machine 192.168.56.1 |
| Virtualization | VirtualBox — NAT + Host-Only Adapter |
| Network | Host-Only: 192.168.56.0/24 |
| Log Forwarding | Splunk Universal Forwarder → Port 9997 |
| Deep Logging | Sysmon (SwiftOnSecurity config) |
| Targeted Protocol | RDP — Port 3389 |

<br><br>

## 🖼️ Architecture Diagram

```
┌───────────────────────────────────────────────────────────────┐
│                  VirtualBox Lab Environment                   │
│                                                               │
│  ┌──────────────────┐        ┌──────────────────────────┐     │
│  │   Kali Linux     │──RDP──▶│   Windows Server 2022    │     │
│  │  192.168.56.10   │ :3389  │    192.168.56.110        │     │
│  │   ATTACKER       │        │  Sysmon + Splunk UF      │     │
│  └──────────────────┘        └────────────┬─────────────┘     │
│                                           │ Logs              │
│                                           │ Port 9997         │
│                                           ▼                   │
│                              ┌──────────────────────────┐     │
│                              │    Splunk Enterprise      │    │
│                              │      192.168.56.1         │    │
│                              │    localhost:8000         │    │
│                              │    SOC ANALYST VIEW       │    │
│                              └──────────────────────────┘     │
└───────────────────────────────────────────────────────────────┘
```

<br><br>

## 🔄 Data Flow

```
Kali Linux                Windows Server 2022           Splunk Enterprise
    │                            │                            │
    │── xfreerdp RDP attack ────▶│                            │
    │                            │── EventCode 4625 ─────────▶│
    │                            │── EventCode 4624 ─────────▶│
    │                            │── Sysmon EventCode 1 ─────▶│
    │                            │── Sysmon EventCode 3 ─────▶│
    │                            │                             │
    │                            │         SOC Analyst         │
    │                            │         runs SPL queries    │
    │                            │         detects CRITICAL    │
    │                            │         confirms TP         │
    │                            │         initiates contain   │
```

<br><br>

## 🛠️ Lab Components

### Attacker — Kali Linux (192.168.56.10)

| Component | Details |
|---|---|
| OS | Kali Linux |
| Role | Attack simulation |
| Tool Used | xfreerdp |
| Attack Type | RDP brute force — multiple failed auth attempts |
| Network | Host-Only: 192.168.56.10 |

### Victim — Windows Server 2022 (192.168.56.110)

| Component | Details |
|---|---|
| OS | Windows Server 2022 Standard Evaluation |
| Role | Target system |
| RDP | Enabled — Port 3389 |
| Test Account | socuser — added to Remote Desktop Users |
| Sysmon | Deployed — SwiftOnSecurity config |
| Splunk UF | Installed — forwarding to 192.168.56.1:9997 |
| Network | Host-Only: 192.168.56.110 |

### SOC / SIEM — Splunk Enterprise (192.168.56.1)

| Component | Details |
|---|---|
| Tool | Splunk Enterprise |
| Location | Host Machine |
| Web Interface | localhost:8000 |
| Receiving Port | 9997 |
| Index | main |
| Log Sources | Security, System, Application, Sysmon |

<br><br>

## 📋 Log Sources Configured

| Log Source | sourcetype | EventCodes Monitored |
|---|---|---|
| Windows Security Log | XmlWinEventLog:Security | 4624, 4625, 4672, 4776 |
| Windows System Log | WinEventLog:System | Service events |
| Windows Application Log | WinEventLog:Application | Application events |
| Sysmon Operational | WinEventLog:Sysmon | 1, 3, 7, 10, 11, 13 |

<br><br>

## 🔧 Network Configuration

| Setting | Value |
|---|---|
| Network Type | VirtualBox Host-Only |
| Subnet | 192.168.56.0/24 |
| Host Machine IP | 192.168.56.1 |
| Kali Linux IP | 192.168.56.10 |
| Windows Server IP | 192.168.56.110 |
| Firewall Rules | ICMP + Port 9997 allowed from subnet |
| Internet Access | NAT adapter (separate) |

<br><br>

## ⚙️ Sysmon Configuration

Sysmon was deployed using the **SwiftOnSecurity** configuration
which is the industry-standard baseline for endpoint telemetry.

| Setting | Details |
|---|---|
| Config File | sysmonconfig-export.xml |
| Source | github.com/SwiftOnSecurity/sysmon-config |
| Install Command | `.\Sysmon64.exe -accepteula -i sysmonconfig.xml` |
| Log Location | Microsoft-Windows-Sysmon/Operational |

### Key Sysmon EventCodes Captured

| EventCode | Description | SOC Value |
|---|---|---|
| 1 | Process Creation | Full command line visibility |
| 3 | Network Connection | Attacker outbound connections |
| 7 | Image Loaded | DLL injection detection |
| 10 | Process Access | Mimikatz/LSASS detection |
| 11 | File Created | Malware drop detection |
| 13 | Registry Value Set | Persistence detection |

<br><br>

## 📦 Splunk Universal Forwarder Configuration

### inputs.conf

```ini
[WinEventLog://Security]
index = main
disabled = false

[WinEventLog://System]
index = main
disabled = false

[WinEventLog://Application]
index = main
disabled = false

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
index = main
sourcetype = WinEventLog:Sysmon
disabled = false
```

### outputs.conf

```ini
[tcpout]
defaultGroup = splunk-indexer

[tcpout:splunk-indexer]
server = 192.168.56.1:9997
```

<br><br>

## ✅ Lab Setup Verification

| Check | Command | Expected Result |
|---|---|---|
| Kali → Windows ping | `ping 192.168.56.110` | Reply from 192.168.56.110 |
| Windows → Kali ping | `ping 192.168.56.10` | Reply from 192.168.56.10 |
| Windows → Splunk ping | `ping 192.168.56.1` | Reply from 192.168.56.1 |
| Forwarder status | `Get-Service SplunkForwarder` | Status: Running |
| Forwarder connected | `.\splunk.exe list forward-server` | Active: 192.168.56.1:9997 |
| Splunk receiving | `index=main \| stats count by sourcetype` | All 4 sourcetypes visible |

<br><br>

## 🔗 Related Files

| File | Location |
|---|---|
| Full Incident Report | [Reports/soc-investigation-report.md](../Reports/soc-investigation-report.md) |
| Detection Rules | [Detection-Logic/Splunk-Detection-Rules.md](../Detection-Logic/Splunk-Detection-Rules.md) |
| Sigma Rule | [Detection-Logic/sigma-rdp-bruteforce.yml](../Detection-Logic/sigma-rdp-bruteforce.yml) |
| Incident Timeline | [Timeline/incident-timeline.md](../Timeline/incident-timeline.md) |
| Hardening Guide | [Mitigation-Recommendations/rdp-hardening-recommendations.md](../Mitigation-Recommendations/rdp-hardening-recommendations.md) |
| All Screenshots | [Screenshots/](../Screenshots/) |

<br><br>

**Analyst:** Priyanka Rane | SOC Analyst L1
**Date:** 26 May 2026
