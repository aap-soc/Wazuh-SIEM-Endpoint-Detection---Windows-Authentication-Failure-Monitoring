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



**Step 5 - Resolve a Login Error**

Hit an "Invalid username or password" error on the first login attempt. Diagnosed the issue by re-checking the credentials generated during installation and logged in successfully on retry.
![Reviewing the Wazuh dashboard overview](../screenshots/lab1-wazuh-server-install/05-image6.png)

Lab 1 complete ✅

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Lab 2 - Windows Agent Deployment

**Objective**: Deploy a Wazuh agent to a Windows 10 VM so it can be monitored as an endpoint.

**Step 1 - Open the Deploy New Agent Wizard**
With Wazuh successfully launched, clicked the **"Deploy new agent"** button.
![Reviewing the Wazuh dashboard overview](../screenshots/lab2-windows-agent-deployment/01-image7.png)


**Step 2 - Select the Windows Package**

Under "Select the package to download and install on your system," selected the Windows option. In the "Assign an agent name" box, set the name to `windows-agent`, since this is a Windows 10 VM. Then entered the Wazuh server address in the "Assign a server address" box.

![Reviewing the Wazuh dashboard overview](../screenshots/lab2-windows-agent-deployment/02-image8.png)




**Step 3 - Run the Generated Command**

Copied the generated `Invoke-WebRequest` command into the PowerShell terminal on the Windows 10 VM and successfully installed the agent.
![Reviewing the Wazuh dashboard overview](../screenshots/lab2-windows-agent-deployment/03-image9.png)



**Step 4 - Start the Wazuh Service**
Ran `NET START wazuh` in the PowerShell terminal. The Wazuh service started successfully.
![Reviewing the Wazuh dashboard overview](../screenshots/lab2-windows-agent-deployment/04-image10.png)


the Wazuh service started successfully.
![Reviewing the Wazuh dashboard overview](../screenshots/lab2-windows-agent-deployment/05-image11.png)


**Step 5 - Verify via Dashboard**

Verified success by browsing to `https://<ip-address>:443`, confirming an **Active** Windows service on the dashboard.
![Reviewing the Wazuh dashboard overview](../screenshots/lab2-windows-agent-deployment/06-image12.png)



**Step 6 - Deliberately Stop the Service**

Returned to the Windows VM and ran `NET STOP wazuh` to deliberately stop the agent service 
![Reviewing the Wazuh dashboard overview](../screenshots/lab2-windows-agent-deployment/07-image13.png)


then confirmed on the dashboard that the agent had successfully disconnected.
![Reviewing the Wazuh dashboard overview](../screenshots/lab2-windows-agent-deployment/08-image14.png)

Lab 2 complete ✅

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


## Lab 3 - Generating Windows Authentication Failure Logs

**Objective**: Deliberately generate real failed-login events so there's genuine data to detect, rather than relying on synthetic test data.

**Step 1 - Restart the Wazuh Service**

Opened a PowerShell terminal on the Windows 10 VM and ran `NET START wazuh` to ensure the service was running ahead of testing.
![Reviewing the Wazuh dashboard overview](../screenshots/lab3-generating-auth-failures/01-image15.png)


**Step 2 - Confirm Agent Status**

Returned to the Wazuh dashboard and confirmed the Windows agent showed as **Active** before proceeding.
![Reviewing the Wazuh dashboard overview](../screenshots/lab3-generating-auth-failures/02-image16.png)



**Step 3 - Generate Real Authentication Failures**

Restarted the Windows VM and deliberately entered the wrong password several times at the lock screen, generating genuine "incorrect PIN/password" authentication failure events.
![Reviewing the Wazuh dashboard overview](../screenshots/lab3-generating-auth-failures/03-image17.png)

Lab 3 complete ✅

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Lab 4 - Threat Hunting & MITRE ATT&CK Mapping

