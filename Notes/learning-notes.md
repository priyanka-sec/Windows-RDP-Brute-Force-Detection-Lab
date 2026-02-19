# 📘 Learning Notes – Windows RDP Attack Lab

This section documents key technical insights, challenges, and lessons learned while performing the Windows RDP attack simulation and blue-team investigation.

The purpose of this section is to reflect on practical learning outcomes and demonstrate analytical thinking beyond execution.

<br><br>



## 🧠 Key Technical Learnings

### 1️. Understanding Windows Authentication Logs

- Event ID 4625 → Failed logon attempt  
- Event ID 4624 → Successful logon  
- Logon Type provides context about access method  

One important observation during the lab:

- Failed RDP attempts were logged as **Logon Type 3 (Network Logon)**  
- Successful RDP session was logged as **Logon Type 10 (Remote Interactive)**  

This highlighted that authentication logging depends on the stage of session establishment.

📌 Learning:  
Never assume log behavior — always validate in a real environment.

<br>


### 2️. Brute-Force Attack Patterns

Repeated failed authentication attempts from a single source IP indicate:

- Automated attack tools
- Password spraying attempts
- Credential brute-force activity

High-frequency login failures are strong early indicators of compromise.

📌 Learning:  
Detection is often about identifying abnormal patterns, not single events.

<br>


### 3️. Importance of Event Correlation

A single failed login is not suspicious.

However:

- Multiple 4625 events  
- Followed by a 4624 event  
- From the same source IP  

Strongly indicates successful compromise.

📌 Learning:  
SOC analysis requires connecting multiple events into one narrative.

<br>


### 4️. RDP as a High-Risk Service

Remote Desktop Protocol (RDP):

- Is commonly targeted in real-world attacks  
- Frequently exposed to the internet  
- Often protected by weak credentials  

This lab reinforced the importance of:

- Disabling unused services  
- Enforcing strong authentication  
- Restricting remote access exposure  

<br>


### 5️. Registry-Level Security Controls

Using PowerShell to disable RDP via the Windows Registry demonstrated:

- How system configurations impact security posture  
- How quickly containment actions can be applied  
- The importance of understanding system internals

📌 Learning:  
Blue team response often involves system-level configuration changes.

<br><br>





## 🔍 Investigation Mindset Development

This lab helped develop a structured investigation workflow:

1. Identify suspicious activity  
2. Filter relevant logs  
3. Analyze event attributes  
4. Correlate timestamps and source IP  
5. Confirm compromise  
6. Apply mitigation steps  

This structured thinking is critical in SOC environments.

<br><br>





## ⚠️ Challenges Faced

- Interpreting logon types correctly  
- Understanding why failed RDP attempts appeared as Logon Type 3  
- Distinguishing between theory and real lab behavior  

These challenges reinforced the importance of:

- Hands-on validation  
- Practical experimentation  
- Log interpretation accuracy  

<br><br>




## 🛡️ Defensive Security Takeaways

- Monitoring authentication logs is essential  
- RDP should never be exposed directly to the internet  
- Account lockout policies reduce brute-force risk  
- Multi-layered defense (firewall + strong credentials + monitoring) is necessary  
- Rapid containment is crucial once compromise is detected  

<br><br>




## 📈 Skill Development Through This Lab

This project improved:

- Log analysis skills  
- Event correlation techniques  
- Windows Security Event understanding  
- PowerShell-based system hardening  
- Incident documentation practices  

<br><br>




## 🧪 Real-World Relevance

The techniques practiced in this lab align with real SOC analyst responsibilities:

- Monitoring authentication logs  
- Detecting brute-force attempts  
- Investigating suspicious login behavior  
- Documenting incidents  
- Applying mitigation measures  

This lab simulates entry-level blue team workflows in a controlled environment.

<br><br>





## 🏁 Final Reflection

This Windows RDP Attack Lab demonstrated how even simple services can become attack vectors when exposed improperly.

Through hands-on experimentation, log analysis, and mitigation implementation, this exercise reinforced the importance of proactive monitoring and defense-in-depth strategies in cybersecurity operations.

The key takeaway is clear:

Security is not just about preventing attacks —  
It is about detecting, understanding, and responding effectively.
