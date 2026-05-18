# Detection Logic

This folder contains PowerShell-based detection queries and authentication hunting logic used during the investigation of RDP authentication activity.

The detection methods were focused on identifying:

- Failed RDP authentication attempts
- Successful RDP logins
- Authentication patterns
- Remote access activity
- Suspicious login behavior

## Detection Areas Covered

- Event ID 4625 Detection
- Event ID 4624 Detection
- Logon Type 10 Analysis
- Authentication Correlation
- Source IP Investigation

## Tools Used

- Windows Event Viewer
- PowerShell
- Windows Security Logs

## Purpose

The goal of this folder is to demonstrate how SOC analysts use native Windows tools and PowerShell to investigate authentication events and identify suspicious RDP activity without relying heavily on enterprise SIEM platforms.
