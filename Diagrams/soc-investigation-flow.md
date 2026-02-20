# SOC-Investigation-Flow

<br>

## What it shows:
How a SOC analyst detects an attack using log analysis.

<br><br>

## Structure:

```mermaid
graph TD
    A[Windows Security Logs]
    B[Log Review <br> Event Viewer]
    C[Identify 4625 Failures]
    D[Detect 4624 Success]
    E[Event Correlation]
    F[Timeline Reconstruction]
    G[Incident Confirmation]
    H[Response & Containment]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
    G --> H
```



<br>

## SOC Investigation Flow: 

This diagram represents how a Security Operations Center (SOC) analyst correlates authentication logs to detect brute-force activity and reconstruct an attack timeline.

