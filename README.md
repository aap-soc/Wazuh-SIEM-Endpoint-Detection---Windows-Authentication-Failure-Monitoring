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

  
    Wazuh-SIEM-Endpoint-Detection - Windows Authentication Failure Monitoring/
- │
- ├── README.md                        ← Project overview
- ├── docs/
- │   └── lab-notes.md                 ← Step-by-step notes mirroring every screenshot
- └── screenshots/
-       ├── lab1-lab1-wazuh-server-install/           ←  10 screenshots
-       ├── windows-agent-deployment/                 ←   3 screenshots
-       ├── lab3-generating-auth-failures/            ←  12 screenshots
-       └── lab 4- threat-hunting-mitre/              ←   7 screenshots
-       

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 🧠 Why This Matters in Security Operations


|               **Component**                        |                  **Security Purpose**                                        |                Real-World Use                       |                                                                                    
|----------------------------------------------------|------------------------------------------------------------------------------|-----------------------------------------------------|
|         **Wazuh Manager/Indexer/Dashboard**        | Central platform for collecting, indexing and visualising security telemetry |     The core of any SOC's detection capability      | |                                                    |                                                                              |                                                     |
|                                                    |                                                                              |                                                     |
|         **Agent-Based Endpoint Monitoring**        | Extends visibility to individual devices, not just network infrastructure    | Detects compromise or misuse at the endpoint, where many attacks originate   |                           |                                                                              |                                                     |                                                      |
|                                                    |                                                                              |                                                     |
|          **Deliberate Detection Testing**          | Generating a real event to confirm a control actually works, rather than assuming it does|  Closes the loop between "configured" and "verified working",  a control nobody has tested is an assumption, not a control                                                                           |                                                     | |                                                    |                                                                              |                                                     |
|                                                    |                                                                              |                                                     |
|            **MITRE ATT&CK Mapping**                |      Ties a raw alert to a specific, named adversary technique               |  Gives analysts a structured, industry-standard way to reason about what an alert actually means and how serious it is                                                   |
|                                                    |                                                                              |                                                     |                                                               


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


## ✅ Labs Completed

### Lab 1 - Wazuh Server Installation (Ubuntu)

Installing the Wazuh indexer, manager and dashboard on an Ubuntu VM.

**What I configured:**
- Reviewed Wazuh's official website and selected Quickstart, which provides a single installation-assistant command covering the indexer, manager, Filebeat and dashboard in one step
- Ran the installation assistant in my Ubuntu terminal via the generated "curl" command, watching the installer generate certificates and install each service in sequence (indexer → manager → Filebeat → dashboard)
- Logged into the Wazuh dashboard using the credentials generated during installation
- Diagnosed and resolved an initial "Invalid username or password" error by re-checking the generated credentials


**Key security concepts:**
- Reading installation logs carefully to extract one-time-generated credentials, rather than assuming defaults
- A single installation-assistant command hides real complexity underneath (certificate generation, multi-service orchestration) - understanding what it actually does matters more than just running it

📁 **Screenshots → screenshots/lab1-wazuh-server-install/ 📄 Step-by-step notes → docs/lab-notes.md#lab-1**


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Lab 2 - Windows Agent Deployment

Deploying a Wazuh agent to a Windows 10 VM so it can be monitored as an endpoint.

**What I configured:**
- Clicked **Deploy new agent** from the Wazuh dashboard and selected the **Windows** package option
- Entered the Wazuh server address and set the agent name to **'windows-agent'**, since this VM is my Windows 10 endpoint
- Copied the generated **'Invoke-WebRequest'** command into a PowerShell terminal on the Windows 10 VM and successfully installed the agent
- Ran **NET START wazuh** to start the Wazuh service, and confirmed it started successfully by checking **'https://<server-ip>:443'** on the dashboard
- Returned to the Windows VM and ran `NET STOP wazuh` to deliberately stop the service, then confirmed on the dashboard that the agent showed as disconnected

**Key security concepts:**
- Agent-based architecture: the manager does not reach out to endpoints, endpoints connect out to the manager, which is friendlier to typical corporate firewall rules
- Deliberately testing both the "connected" and "disconnected" states confirms the dashboard's status reporting is trustworthy, not just checking the happy path

📁 **Screenshots → screenshots/lab2-windows-agent-deployment/ 📄 Step-by-step notes → docs/lab-notes.md#lab-2**


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Lab 3 - Generating Windows Authentication Failure Logs

Deliberately generating real failed-login events so there is something genuine to detect, rather than testing against synthetic data.

**What I configured:**
- Restarted the Wazuh service on the Windows 10 VM (`NET START wazuh`) ahead of the test
- Confirmed the Windows agent showed as **Active** on the Wazuh dashboard before proceeding
- Restarted the Windows VM and deliberately entered the wrong password several times at the lock screen to generate real "**incorrect PIN/password**" authentication failures

**Key security concepts:**
- Generating a real, verifiable event rather than assuming a detection rule works is the only way to actually validate a SIEM pipeline end-to-end
- Confirming agent health (Active status) immediately before a test isolates the test from unrelated connectivity issues

📁 **Screenshots → screenshots/lab3-generating-auth-failures/ 📄 Step-by-step notes → docs/lab-notes.md#lab-3**


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


### Lab 4 - Threat Hunting & MITRE ATT&CK Mapping

Confirming the deliberately-generated failures were detected, then mapping the detection to a named MITRE ATT&CK technique.

**What I configured:**
- Reviewed the Wazuh Overview dashboard, showing the full range of available modules (Threat Hunting, MITRE ATT&CK, Vulnerability Detection, File Integrity Monitoring, PCI DSS, and others)
- Found 4 Authentication Failure alerts on the Threat Hunting dashboard, then opened the Events tab to see the underlying **windows-agent** Logon Failure logs directly
- Opened the MITRE ATT&CK dashboard and mapped the **windows-agent** Logon Failure event against MITRE ATT&CK, identifying it under the **Impact** tactic with technique ID **T1531**
- Clicked into the T1531 technique to bring up its full name and description: **Account Access Removal** — adversaries interrupting availability of system/network resources by locking, deleting, or manipulating account access

**Key security concepts:**
- Closing the loop: an agent that's "Active" is not proof detection actually works - generating a real event and confirming it appears is the only way to know the pipeline functions end-to-end
- MITRE ATT&CK mapping turns a raw log line into something analytically useful: "a failed logon" becomes "a T1531 Account Access Removal technique attempt," which is the language SOC teams actually use to triage and prioritise
- Drilling into a technique's full description (not just its ID) is what separates recognising a MITRE code from actually understanding the adversary behaviour it represents

📁 **Screenshots → screenshots/lab4-threat-hunting-mitre/ 📄 Step-by-step notes → docs/lab-notes.md#lab-4**

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 📚 Key Security Concepts Demonstrated

- **End-to-end SIEM deployment** - server install through to confirmed working detection, not just installation
- **Agent-based endpoint monitoring** - extending visibility beyond network-level logging to individual devices, including deliberately testing both connected and disconnected states
- **Deliberate detection validation** -  generating real failed-login events specifically to test the pipeline, rather than assuming configuration equals working detection
- **MITRE ATT&CK-informed triage** - mapping a raw alert to a specific, named technique (T1531 - Account Access Removal) and understanding what that technique actually means
- **Verification discipline** - confirming a control actually works by testing it, not assuming it works because it was configured


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## 🔗 Related Projects

- 🖥️ Microsoft 365 Security Operations Tenant (SC-200) (https://github.com/aap-soc/Microsoft-365-Security-Operations-Tenant-SC-200)
- 🔐 AWS Security Controls Lab (https://github.com/aap-soc/AWS-Security-Controls-Lab)
- 🐧 Linux Security & Log Handling Portfolio (https://github.com/aap-soc/linux-security-portfolio)
 



