Focus: Alert triage, investigation, and log correlation in Microsoft Sentinel

**Objective**

Understand alert triaging and prioritisation

Review and analyse alerts in Microsoft Sentinel

Correlate alerts and logs to identify real malicious activity

Build investigation context and timelines

**Environment**

Microsoft Azure Portal

Microsoft Sentinel (Dedicated lab instance)

Custom log table: Syslog_CL

Lab-provided Azure tenant and credentials

**Step 1: Access Azure Portal**

Open the Microsoft Azure Portal

Log in using the provided Temporary Access Pass (TAP) credentials


Step 2: Open Microsoft Sentinel

In the Azure Portal search bar, search for Microsoft Sentinel

Click your assigned Sentinel workspace

![image.png](screenshots/Sentinel/dash1.png)

Step 3: Verify Log Ingestion (Syslog_CL)

In Sentinel, navigate to Logs

Select or manually query the custom table:

Syslog_CL


Run the query and wait for logs to render

**Note:** Log ingestion may take a few minutes. Refresh if needed.


![image.png](screenshots/Sentinel/dash2.png)

Step 4: Prepare Analytics Rules

In Sentinel, go to Configuration → Analytics

Select all enabled rules

Click Disable

Re-select the same rules and click Enable

This forces the rules to retrigger and generate incidents.



**Step 5: Open Incidents Queue**

In Sentinel, navigate to:
Threat management → Incidents

Set a custom date range if no incidents are visible

Refresh the page if needed


![image.png](screenshots/Sentinel/incidents.png)

Step 6: Alert Triaging Methodology

Each alert is evaluated using four core triage dimensions:

Factor	Question Answered
Severity	How bad is this?
Time	When did it happen?
Context	Where is this in the attack lifecycle?
Impact	What asset or user is affected?

This helps quickly decide whether to:

Escalate

Investigate further

Close as false positive

Step 7: Investigate High-Severity Incident

Select a High Severity incident:
Linux PrivEsc – Kernel Module Insertion

Review the summary panel:

Number of events

Affected entities

Tactic: Privilege Escalation

Click View full details


![image.png](screenshots/Sentinel/fulldetails.png)

Step 8: Analyse Related Alerts

Review Similar Incidents

Review Incident Timeline

Identify alerts sharing the same entity (host/user/IP)

Example correlation:

Alert	Meaning
Root SSH Login	Initial Access
SUID Discovery	Privilege Escalation Attempt
Kernel Module Insertion	Persistence



Step 9: Dive into Event Evidence

In Full Details, open Evidence → Events

Identify:

Kernel module name

Installation timestamp

Affected host


![image.png](screenshots/Sentinel/moduleinsert.png)

Step 10: Deep Log Analysis with KQL

Switch query mode to KQL

Run the following query to inspect logs from the affected host:

Syslog_CL
| where host_s == "app-02"
| project _timestamp_t, host_s, Message


![image.png](screenshots/Sentinel/kmilogs.png)

Step 11: Identify Suspicious Activity

From the logs, observe:

Shadow file backup creation (cp command)

User Alice added to sudoers

backupuser modified by root

Malicious kernel module insertion

Successful root SSH authentication

These events strongly indicate:

Privilege escalation

Persistence setup

Potential full system compromise

![image.png](screenshots/Sentinel/kql.png)

**Step 12: Investigation Outcome**

**NOTES:**
This incident represents real malicious activity, not a false positive.

**Indicators:**

PrivEsc via kernel module

Account manipulation

Root-level access

Persistence techniques observed

Next Action:

Escalate to Incident Response

Containment and remediation required

![image.png](screenshots/Sentinel/rootkql.png)

Key Takeaways

Alert triage prevents analyst overload

Correlating alerts reveals full attack chains

Sentinel enables fast pivoting from alert to raw logs

Proper documentation strengthens SOC operations
