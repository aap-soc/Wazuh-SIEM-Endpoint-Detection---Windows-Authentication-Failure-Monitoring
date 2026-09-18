### 🛡️ Wazuh-SIEM-Endpoint-Detection - Windows Authentication Failure Monitoring

- **Focus**: SIEM / Endpoint Detection & Response | Threat Hunting | MITRE ATT&CK
- **Environment**: Wazuh 4.14.7 (Ubuntu manager, VMware) | Windows 10 endpoint agent
- **Date**: 2026
- **Author**: Andre Patterson

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 📋 Overview

I present a hands-on Wazuh SIEM deployment: installing the Wazuh server stack (indexer, manager, dashboard) on Ubuntu, deploying a Wazuh agent to a Windows 10 endpoint, deliberately generating real failed-login events, and using the Threat Hunting and MITRE ATT&CK modules to confirm detection end-to-end, down to the specific adversary technique involved.

This lab was built end-to-end in a personal VMware lab environment (Ubuntu server VM, Windows 10 endpoint VM)

**Evidence integrity**: Screenshots are taken from my own completed console work. IP addresses and other identifying details have been redacted. Credentials are never shown in any screenshot in this repo.


**Project objective**

1. Stand up a working Wazuh server (indexer, manager, dashboard) from scratch.
2. Deploy and connect a Windows endpoint as a monitored agent.
3. Deliberately generate real failed-login events and confirm they appear in Wazuh's Threat Hunting module.
4. Map a detected event against MITRE ATT&CK to identify the specific adversary technique it represents
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


## 🗂️ Repo Structure

  
    Microsoft-365-Security-Tenant SC-200/
- │
- ├── README.md                        ← Project overview
- ├── docs/
- │   └── lab-notes.md                 ← Step-by-step notes mirroring every screenshot
- └── screenshots/
-       ├── lab1-tenant-entra-setup/           ←  10 screenshots
-       ├── lab2-e5-trial-domain/              ←   3 screenshots
-       ├── lab3-admin-accounts/               ←  12 screenshots
-       └── lab 4- standard-users/             ←   7 screenshots
-       └── lab 5- defender-xdr-alerts/        ←  11 screenshots






--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
