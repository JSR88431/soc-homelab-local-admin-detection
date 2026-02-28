# Local Administrator Privilege Escalation Detection using Microsoft Sentinel
## Overview

This project demonstrates detection and investigation of a privilege escalation attack using Microsoft Sentinel. A simulated attacker created a new local user account and granted administrative privileges on a Windows host. Security telemetry was collected through Azure Arc and analyzed using Kusto Query Language (KQL).

The objective was to identify unauthorized privilege escalation behavior and map findings to the MITRE ATT&CK framework.

## Lab Architecture

* Windows 11 Virtual Machine (WIN-LAB)
* VMware Workstation Player
* Azure Arc Connected Machine
* Azure Monitor Agent (AMA)
* Log Analytics Workspace
* Microsoft Sentinel (SIEM)


<img width="800" height="1200" alt="VM_Lab_Diagram" src="https://github.com/user-attachments/assets/f87cb1a9-08a4-4a78-ba2e-2e2ae3f968c0" />


## Attack Simulation
The following commands were executed on the Windows VM to simulate attacker behavior:
```
net user socadmin /add
net user socadmin ThePassword1!
net localgroup administrators socadmin /add
```

## What This Simulates
An attacker creates a backdoor account and assigns administrator privileges to maintain elevated access on the system.

## Data Collection
Windows Security Events were ingested into Microsoft Sentinel using:
* Azure Arc onboarding
* Azure Monitor Agent
* Data Collection Rule configured for Security logs

Relevant Windows Event IDs:
* 4720 — User account created
* 4732 — User added to privileged group
  
<img width="1257" height="730" alt="01-security-events" src="https://github.com/user-attachments/assets/ba0ec2f8-c57c-4206-b116-5c1274e3b7d6" />
<img width="1258" height="731" alt="02-security-events" src="https://github.com/user-attachments/assets/4c2fd79a-b0a4-4268-a289-077d02e62e8d" />



# Detection Logic
### KQL Detection Query
```
Event
| where EventID in (4720, 4732)
| sort by TimeGenerated desc
```
This query identifies accounts added to privileged security groups such as Administrators.


# Investigation Process
1. Queried SecurityEvent logs in Microsoft Sentinel.

2. Identified Event ID 4720 indicating account creation.

3. Correlated Event ID 4732 showing administrator group membership change.

4. Confirmed activity originated from host WIN-LAB.

5. Reviewed event timestamps to validate escalation sequence.

# MITRE ATT&CK Mapping
**Tactic: Privilege Escalation**

**Technique: T1136 — Create Account**

**Evidence:**

Event ID 4720 shows creation of a new local account used for elevated access.

# Response Actions
* Removed account from Administrators group.
* Disabled unauthorized account.
* Recommended monitoring privileged group membership changes.
* Suggested alert tuning for privileged account activity.

<img width="1524" height="666" alt="detection_rule" src="https://github.com/user-attachments/assets/fc27bf19-648f-4e01-bda5-62cd14892c20" />

# Lessons Learned
* Privileged group monitoring is critical for early compromise detection.
* Sentinel analytics rules allow rapid detection of privilege escalation attempts.

# Skills Demonstrated
* Security log analysis
* Microsoft Sentinel investigation workflow
* Kusto Query Language (KQL)
* Detection rule creation
* Privilege escalation analysis
* MITRE ATT&CK mapping
* Incident documentation

## Author

Josephson Reynoso

https://www.linkedin.com/in/josephson-reynoso/




