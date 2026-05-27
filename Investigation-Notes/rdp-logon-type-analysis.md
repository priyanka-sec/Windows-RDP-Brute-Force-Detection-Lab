# RDP Logon Type Analysis
## INC-RDP-2026-001

**Analyst:** Priyanka Rane — SOC L1
**Date:** 26 May 2026

---

## Why Logon Type Matters in SOC Investigations

When investigating authentication events, Logon Type tells
a SOC analyst HOW the login occurred. This is critical for
understanding the attacker's access method and impact.

---

## Complete Logon Type Reference

| Type | Name | Description | Risk Level |
|---|---|---|---|
| 2 | Interactive | Physical console login | Low |
| 3 | Network | SMB, file shares, net use | Medium |
| 4 | Batch | Scheduled tasks | Low |
| 5 | Service | Service account login | Low |
| 7 | Unlock | Workstation unlock | Low |
| 8 | NetworkCleartext | Plaintext password over network | HIGH |
| 9 | NewCredentials | runas command | Medium |
| 10 | RemoteInteractive | RDP — Terminal Services | HIGH |
| 11 | CachedInteractive | Cached domain credentials | Medium |

---

## Observed in This Investigation

**Logon Type 10 — RemoteInteractive (RDP)**

This means the attacker established a full Remote Desktop
session on the Windows Server 2022 machine. The attacker
had complete graphical access to the server — identical
to sitting physically in front of the machine.

---

## Impact of Logon Type 10 Access

With an active RDP session the attacker could:

- Browse all files and folders on the server
- Install malware or tools
- Create backdoor accounts
- Exfiltrate data
- Move laterally to other systems
- Establish persistence via scheduled tasks or registry

---

## Why This Escalates Severity

A failed login (4625) is an **attempted** breach — severity MEDIUM.
A successful login (4624) with Logon Type 10 is a **confirmed**
breach with full system access — severity **P2 HIGH minimum.**

**This is why the incident was escalated to L2 immediately.**
