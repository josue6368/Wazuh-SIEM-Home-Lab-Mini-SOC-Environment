# Wazuh-SIEM-Home-Lab-Mini-SOC-Environment
Built a SIEM-based home lab using Wazuh on Ubuntu Server to simulate a mini Security Operations Center (SOC). A Windows 11 endpoint was onboarded to collect and analyze security logs. The lab demonstrates authentication monitoring, PowerShell activity detection, brute force simulation, custom detection rules, and MITRE ATT&CK mapping.
## Overview
This project demonstrates the deployment of a SIEM-based home lab using Wazuh to simulate a real-world SOC environment. A centralized Wazuh server was installed on Ubuntu Server and configured to collect, process, and analyze logs from a Windows 11 endpoint.

The lab validates log ingestion and detection capabilities by generating authentication failures and PowerShell activity, providing visibility into endpoint behavior and security events.
## Lab Architecture  
* Wazuh Server: Ubuntu Server (SIEM, dashboard, indexer)
* Endpoint: Windows 11 VM (log source)
* Attacker: Kali Linux VM
* Platform: VMware

## Tools and Technologies
* Wazuh SIEM
* Ubuntu Server
* Windows 11
* VMware
* PowerShell

## Key Features
* Centralized log collection and analysis
* Endpoint monitoring via Wazuh agent
* Detection of failed authentication attempts
* PowerShell activity monitoring
* Real-time security event visibility
* Custom detection rule development

## Setup Summary
* Installed Wazuh on Ubuntu Server
* Accessed Wazuh dashboard via web interface
* Deployed Wazuh agent on Windows 11 endpoint
* Verified agent connectivity and log ingestion
* Generated test events (failed logins, PowerShell commands)

## Artifacts

### Wazuh Server Status (Ubunto Server)
<br />
<img width="1555" height="1088" alt="Screenshot 2026-04-29 091256" src="https://github.com/user-attachments/assets/05f8ef0e-5445-4010-9819-95178298af27" />
<br />
The following command was used to verify that all Wazuh components were running:

```
echo "Manager: $(systemctl is-active wazuh-manager) | Dashboard: $(systemctl is-active wazuh-dashboard) | Indexer: $(systemctl is-active wazuh-indexer)"
```
<br />
<img width="1426" height="70" alt="Screenshot 2026-04-29 093528" src="https://github.com/user-attachments/assets/f361265c-0528-40d0-b0a1-97e4e46a6663" />
<br />

### Wazuh Dashboard (Log Ingestion)
Logs from the Windows endpoint were successfully ingested and visualized in the Wazuh dashboard.
<br />
<img width="2519" height="1387" alt="Screenshot 2026-04-29 074434" src="https://github.com/user-attachments/assets/b5bf6f5c-1516-48eb-bda1-309f48d453bd" />
<br />

Multiple failed login attempts were generated on the Windows VM and detected by Wazuh.

* Event ID: 4625
* rule.id: 60122
* Description: Login failure - Unknown user or bad password.
<br />
<img width="2529" height="1356" alt="Screenshot 2026-04-29 074549" src="https://github.com/user-attachments/assets/58133c21-871a-4a06-a14c-412da3e282e3" />
<br />
Additional event details provide contextual mappings such as MITRE ATT&CK and NIST references.
<br />
<img width="1442" height="223" alt="Screenshot 2026-04-29 075129" src="https://github.com/user-attachments/assets/b6f61cd7-0320-4fb1-b70f-3e7aac5b85a5" />
<br />

### PowerSehll Activity Monitoring
PowerShell Script Block Logging was enabled on the Windows endpoint to improve visibility into command execution. Several administrative and enumeration commands were executed to simulate common attacker behavior, including user and privilege discovery.
<br />
<img width="867" height="623" alt="image" src="https://github.com/user-attachments/assets/a2a4dfa9-1667-4325-ab50-8026fd1ae6a9" />
<br />
These activities were successfully captured and visualized within the Wazuh dashboard, demonstrating the ability to monitor and analyze PowerShell-based activity in a centralized SIEM environment.
Wazuh successfully ingested and correlated these events, providing centralized visibility into PowerShell activity. Relevant logs included process creation events (Event/rule ID 598) and PowerShell execution data, demonstrating the ability to monitor and analyze endpoint activity within a SIEM environment.
<br />
<img width="1008" height="558" alt="image" src="https://github.com/user-attachments/assets/f3c7f465-163a-4cb9-ae96-e0eb32b335e5" />
<br />

#### Detection Context
The observed PowerShell activity aligns with common adversary techniques such as:

* Account Discovery
* Permission Group Enumeration
* System Information Discovery

These behaviors are frequently associated with post-exploitation and lateral movement phases in real-world attacks.
<br />
### Brute Force Attack Simulation

A brute force attack was simulated using Kali Linux and Hydra against the Windows 11 endpoint over RDP. A password wordlist was used to generate repeated authentication attempts against a target account.

Despite minor connection limitations inherent to RDP in virtualized environments, the attack successfully triggered multiple failed login attempts on the Windows system.

Wazuh ingested and correlated these events, detecting repeated authentication failures (Event ID 4625) occurring within a short time frame.

Hydra brute force execution from Kali:
<img width="393" height="200" alt="image" src="https://github.com/user-attachments/assets/86bec1ce-bd15-4ff1-9df6-daff660ba9bb" />

<br />
Wazuh logs showing brute force activity:
<img width="2034" height="1344" alt="Screenshot 2026-04-29 125755" src="https://github.com/user-attachments/assets/e5a91a7c-ad07-4d77-b3bc-ac6e4259bbbe" />
<br />
Detailed event view (source IP, username, timestamps):
<img width="2027" height="1368" alt="Screenshot 2026-04-29 125847" src="https://github.com/user-attachments/assets/d5a375f7-f709-4cbf-b1dd-9631099d150a" />
<br />

#### Detection Insight

The pattern of rapid, repeated authentication failures from a single source IP is consistent with brute force attack behavior. This demonstrates the importance of monitoring authentication logs to detect unauthorized access attempts.

#### Custom Brute Force Detection Rule

A custom Wazuh detection rule was developed to identify brute force attack behavior based on repeated failed authentication attempts.

The rule was configured to trigger when multiple failed login events occur within a defined time window, generating a high-severity alert:
<img width="1104" height="286" alt="Screenshot 2026-04-29 175149" src="https://github.com/user-attachments/assets/5daa2940-4533-40f7-8628-cdc0eabf4e2c" />


The rule was tuned to correlate events using the appropriate Wazuh rule ID associated with Windows Event ID 4625:
<img width="2025" height="1792" alt="Screenshot 2026-04-29 175538" src="https://github.com/user-attachments/assets/1c4dfe80-536d-4740-8f26-e1ab13cc2b97" />

#### MITRE ATT&CK Mapping
This detection was mapped to:
* T1110 – Brute Force
<img width="1630" height="665" alt="Screenshot 2026-04-29 175654" src="https://github.com/user-attachments/assets/bab3c13d-b13d-4779-bc42-35aaf79bdd40" />

### Skills Demonstrated
* SIEM deployment and configuration
* Endpoint log collection and analysis
* Threat detection and event correlation
* Brute force attack simulation
* PowerShell activity monitoring
* Custom detection rule creation
* MITRE ATT&CK mapping

Author: josue6368








