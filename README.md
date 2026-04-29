# Wazuh-SIEM-Home-Lab-Mini-SOC-Environment
Built a SIEM-based home lab using Wazuh on Ubuntu Server to simulate a mini SOC environment. Onboarded a Windows 11 endpoint to collect and analyze security logs. Generated authentication failures and PowerShell activity to validate detection, with plans for brute force simulation, custom rules, and MITRE ATT&amp;CK mapping.
## Overview
This project demonstrates the deployment of a SIEM-based home lab using Wazuh to simulate a mini Security Operations Center (SOC). A centralized Wazuh server was installed on Ubuntu Server and configured to collect and analyze logs from a Windows 11 endpoint.

The lab validates log ingestion and detection by generating authentication failures and PowerShell activity, providing visibility into endpoint behavior and security events.
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
* Endpoint monitoring with Wazuh agent
* Detection of failed authentication attempts
* Visibility into PowerShell command activity
* Real-time security event monitoring

## Setup Summary
* Installed Wazuh on Ubuntu Server
* Accessed Wazuh dashboard via web interface
* Deployed Wazuh agent on Windows 11 VM
* Verified agent connectivity and log ingestion
* Generated test events (failed logins, PowerShell commands)

## Artifacts

### Wazuh Server Status (Ubunto Server)
<img width="1555" height="1088" alt="Screenshot 2026-04-29 091256" src="https://github.com/user-attachments/assets/05f8ef0e-5445-4010-9819-95178298af27" />
The following command was used to verify all Wazuh components are running:

* echo "Manager: $(systemctl is-active wazuh-manager) | Dashboard: $(systemctl is-active wazuh-dashboard) | Indexer: $(systemctl is-active wazuh-indexer)"
<img width="1426" height="70" alt="Screenshot 2026-04-29 093528" src="https://github.com/user-attachments/assets/f361265c-0528-40d0-b0a1-97e4e46a6663" />

### Wazuh Dashboard
Logs from the Windows endpoint were successfully ingested and visualized in the Wazuh dashboard.
<img width="2519" height="1387" alt="Screenshot 2026-04-29 074434" src="https://github.com/user-attachments/assets/b5bf6f5c-1516-48eb-bda1-309f48d453bd" />


Multiple failed login attempts were generated on the Windows VM and detected by Wazuh.

* Event ID/rule.id: 60122
* Description: Login failure - Unknown user or bad password.

<img width="2529" height="1356" alt="Screenshot 2026-04-29 074549" src="https://github.com/user-attachments/assets/58133c21-871a-4a06-a14c-412da3e282e3" />

Details show MITRE and NIST possible related information on the event:
<img width="1442" height="223" alt="Screenshot 2026-04-29 075129" src="https://github.com/user-attachments/assets/b6f61cd7-0320-4fb1-b70f-3e7aac5b85a5" />












