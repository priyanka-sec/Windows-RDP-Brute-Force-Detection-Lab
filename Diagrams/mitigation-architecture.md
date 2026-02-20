# Mitigation Architecture
<br>

## What it shows:
How security improved after disabling RDP.

<br><br>

## Structure:

## 1. LEFT — Before Mitigation ❌

```mermaid
graph TD
    A[RDP Enabled]
    B[Port 3389 Open]
    C[No Restrictions]
    D[Brute-force Possible]
   
    A --> B
    B --> C
    C --> D
```



<br><br>




## 2. RIGHT — After Mitigation ✅

```mermaid
graph TD
    A[RDP Disabled <br> Registry Change]
    B[Reduced Attack Surface]
    C[Restricted Remote Access]
    D[Improved Security Posture]
   
    A --> B
    B --> C
    C --> D
```

<br><br>


## Mitigation Architecture

This diagram compares the system security posture before and after mitigation, highlighting how disabling RDP significantly reduced the attack surface.
