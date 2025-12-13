Monitoring Insecure Ports and Services with Splunk (Netstat Use Case)

Netstat data is useful for identifying active ports and services on a target machine. Monitoring insecure or unnecessary services helps reduce the attack surface and detect unwanted activity. This lab walks through creating a Splunk SIEM use case to monitor insecure ports, specifically Telnet.

Lab Scenario

Attackers often abuse open or misconfigured services. A SOC analyst should be able to identify what needs to be monitored, the type of logs required, and the basic configuration needed to detect suspicious activity.

Objectives

Create a Splunk detection rule that monitors insecure ports using Netstat output forwarded from a Windows server.

Steps
1. Prepare the Windows Server (WinServer2012)

Log in using the Administrator account.

Create a script to run Netstat regularly:

Open Notepad, type:

netstat -ano


Save it as watch.bat in:
C:\Program Files\SplunkUniversalForwarder\bin\scripts

2. Configure Splunk Universal Forwarder

Edit:
C:\Program Files\SplunkUniversalForwarder\etc\system\default\inputs.conf

Add:

[script://$SPLUNK_HOME\bin\scripts\watch.bat]
disabled = false
interval = 10
source = netstat_mon
sourcetype = netstat_monitor


Save and restart SplunkForwarder Service.

3. Use Splunk on SIEM1 Machine

Log into Splunk Web at http://localhost:8000

Go to SplunkForwarder app.

Run search to detect Telnet traffic:

source=netstat_mon 
| multikv 
| table host Proto Local State PID 
| regex Local ="/*:23"


Set the time window to Last 5 minutes.

4. Create Alert for Telnet Detection

Save search as Alert.

Use the following settings:

Title: The Telnet port has been found opened

Description: Monitoring for Insecure Port

Alert Type: Real-time

Action: Add to Triggered Alerts

Severity: Low

Save the alert.

5. Trigger the Alert

On WinServer2012, enable and start the Telnet service.

Return to SIEM1 and verify that the alert appears under Activity → Triggered Alerts.

6. Disable & Clean Up

Go to Settings → Searches, Reports, and Alerts

Select Splunk Forwarder app.

Disable the alert.

Clear any triggered alerts.

Stop the Telnet service on WinServer2012.

