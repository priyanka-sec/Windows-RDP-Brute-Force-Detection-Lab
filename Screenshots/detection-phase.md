## 🔍 Detection Phase

Highlights how the attack was detected using Windows Security Logs.

Includes:
- Event Viewer logs
- Event ID 4625 (Failed Logons)
- Event ID 4624 (Successful Logon)
- Log correlation indicators

<br><br>

![Event Viewer Navigation](images/event-viewer-navigation.png)
<br>
### Windows Event Viewer navigation showing access to Security logs for authentication monitoring

<br><br>

![Multiple Failed Attempts](images/multiple-failed-logins.png)
<br>
### Multiple Event ID 4625 entries observed, highlighting repeated failed login attempts typical of brute-force activity

<br><br>

![Failed Logon Event (4625)](images/eventid-4625-failed-login.png)
<br>
### Detailed view of Event ID 4625 showing failed authentication attempts and associated logon metadata

<br><br>

![Successful Logon (4624)](images/eventid-4624-successful-login.png)
<br>
### Event ID 4624 confirming successful logon, correlated with prior brute-force attempts
