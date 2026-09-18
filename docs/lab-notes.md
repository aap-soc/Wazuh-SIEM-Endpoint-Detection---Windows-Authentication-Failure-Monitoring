# 📄 Lab Notes: Wazuh SIEM - Endpoint Detection Lab

- **Author**: Andre Patterson
- **Environment**: Wazuh 4.14.7 | Ubuntu manager VM (VMware) | Windows 10 endpoint VM

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Lab 1 -  Wazuh Server Installation (Ubuntu)

**Objective**: Install the Wazuh indexer, manager, dashboard on Ubuntu and get the dashboard accessible.


**Step 1 - Review the Wazuh Dashboard Overview**
Reviewed Wazuh's official website, which showcases the dashboard's features: regulatory compliance, vulnerability management, installation guides and single universal agent capabilities for endpoint protection.
![Reviewing the Wazuh dashboard overview](../screenshots/lab1-wazuh-server-install/01-image1.png)



**Step 2 - Review Quickstart Documentation**
Selected Quickstart from Wazuh's documentation, which provides a single installation-assistant command covering the indexer, manager, Filebeat and dashboard in one step.
![Reviewing the Wazuh dashboard overview](../screenshots/lab1-wazuh-server-install/02-image2.png)



**Step 3 - Run the Installer**
Downloaded and ran the Wazuh installation assistant in the Ubuntu terminal via the **'curl'** command. The installer flagged that the current system (outside the officially recommended list) might not work perfectly, but proceeded regardless, a good reminder that "recommended" isn't the same as "required." The installer then generated certificates for the root CA, admin, indexer, Filebeat, and dashboard, and installed and started each service in sequence.
![Reviewing the Wazuh dashboard overview](../screenshots/lab1-wazuh-server-install/03-image3.png)



**Step 4 - Log Into the Dashboard**
Installation completed successfully, producing Wazuh credentials. Used these credentials from the Ubuntu terminal to log into the Wazuh dashboard.
![Reviewing the Wazuh dashboard overview](../screenshots/lab1-wazuh-server-install/04-image5.png)



**Step 5 — Resolve a Login Error**
Hit an "Invalid username or password" error on the first login attempt. Diagnosed the issue by re-checking the credentials generated during installation and logged in successfully on retry.
![Reviewing the Wazuh dashboard overview](../screenshots/lab1-wazuh-server-install/05-image6.png)

Lab 1 complete ✅

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Lab 2 - Windows Agent Deployment

**Objective**: Deploy a Wazuh agent to a Windows 10 VM so it can be monitored as an endpoint.

**Step 1 - Open the Deploy New Agent Wizard**
With Wazuh successfully launched, clicked the "Deploy new agent" button.
![Reviewing the Wazuh dashboard overview](../screenshots/lab2-windows-agent-deployment/01-image7.png)


**Step 2 - Select the Windows Package**
Under "Select the package to download and install on your system," selected the Windows option.
![Reviewing the Wazuh dashboard overview](../screenshots/lab2-windows-agent-deployment/02-image8.png)


**Step 3 - Configure Server Address**
Entered the Wazuh server address in the "Assign a server address" box.




**Step 4 - Name the Agent**
In the "Assign an agent name" box, set the name to `windows-agent`, since this is a Windows 10 VM.



**Step 5 - Run the Generated Command**
Copied the generated `Invoke-WebRequest` command into the PowerShell terminal on the Windows 10 VM and successfully installed the agent.



**Step 6 - Start the Wazuh Service**
Ran `NET START wazuh` in the PowerShell terminal. The Wazuh service started successfully.



**Step 7 - Verify via Dashboard**
Verified success by browsing to `https://<ip-address>:443`, confirming an **Active** Windows service on the dashboard.




**Step 8 - Deliberately Stop the Service**
Returned to the Windows VM and ran `NET STOP wazuh` to deliberately stop the agent service, then confirmed on the dashboard that the agent had successfully disconnected.

