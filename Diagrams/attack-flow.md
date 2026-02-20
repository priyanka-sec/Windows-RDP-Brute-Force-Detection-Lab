# Attack-Flow

<br>

## What it shows:
End-to-end attacker journey.

## Structure:

```mermaid
graph TD
    A[Attacker - Kali Linux]
    B[Recon - IP Discovery + Port 3389]
    C[Brute Force Attempts<br>Event ID 4625]
    D[Successful Login<br>Event ID 4624]
    E[Unauthorized RDP Access]
    F[SOC Detection]
    G[Mitigation Applied]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
```
<br>

## Attack Flow Diagram

This diagram illustrates the full lifecycle of the simulated RDP attack, starting from reconnaissance and brute-force attempts to successful compromise, detection, and mitigation.
