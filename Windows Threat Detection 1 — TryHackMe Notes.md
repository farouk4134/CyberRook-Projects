Windows Threat Detection 1 — TryHackMe Notes

Room: Windows Threat Detection 1
Focus: Initial access, phishing, brute force, malware execution, and post-compromise activity using Windows logs.

Overview

This room builds on Windows logging fundamentals and focuses on detecting real-world attack techniques, especially phishing-based initial access and post-exploitation behavior. The analysis relies heavily on Security logs, Sysmon, DNS events, and file activity.

Task Walkthrough & Key Findings
1. MITRE Technique for Initial Access via Vulnerable Mail Server

Technique ID: T1190
Technique: Exploit Public-Facing Application

This technique covers exploitation of externally accessible services, including vulnerable mail servers.

2. Initial Access Method via Malicious Email Attachment

Answer: Phishing

This aligns with user-assisted execution through email attachments.

3. Most Actively Brute-Forced User

Answer: Administrator

Identified by filtering:

Security logs

Event ID 4625 (Failed logon attempts)

4. Source IP That Successfully Breached Host via RDP

Logon Type: 10 (RDP)
Event ID: 4624 (Successful logon)

Answer: 203.205.34.107

📸 Screenshot placeholder: Successful RDP login event

5. Real Workstation Name of Threat Actor

Derived from the same successful RDP log event.

Answer: DESKTOP-QNBC4UU

6. Phishing Case 1 — Executing Misleading File

Action: Executed www.zoom.com (COM file)

Flag:

THM{misleading_extension}

Demonstrates abuse of misleading file extensions.

7. Phishing Case 2 — LNK Download Source

Analyzed LNK file properties and target command.

Malicious URL:

http://wp16.hqywlqpa.thm:8000/cgi-bin/f
8. Phishing Case 3 — Double Extension File

Answer:

best-cat.jpg.exe

Classic double-extension social engineering technique.

9. File Downloaded via Web Browser

Identified by:

Parent process: explorer.exe

File-related event logs

Answer:

C:\Users\Administrator\Downloads\top-cats.zip

📸 Screenshot placeholder: File download event

10. Folder Where Suspicious File Was Extracted

Correlated archive extraction with later file and process activity.

Answer:

C:\Users\Administrator\Pictures
11. Process ID of Launched Phishing Malware

Correlated process creation with extracted files.

Answer: 5484

12. Malicious Domain Contacted by Malware

Identified using DNS and network-related logs.

Answer:

rjj.store
13. USB File Launched by User

Detected through file execution events on removable media.

Answer:

E:\Open Sandisk 4GB USB.exe
14. Malicious File Dropped to Disk

Detected via file creation activity.

Answer:

C:\Users\Public\Documents\winupdate.exe
15. USB Device Malware Propagated To

Observed in removable media activity logs.

Answer:

F:
Key SOC Takeaways

Phishing remains a dominant initial access vector

Double extensions and misleading file names are still effective

Administrator accounts are prime brute-force targets

RDP logons (Logon Type 10) are critical to monitor

DNS logs are essential for identifying C2 infrastructure

USB devices remain a common malware propagation path

Skills Reinforced

Windows Security log analysis (4624 / 4625)

RDP attack detection

Phishing attachment analysis

Process and file correlation

DNS-based threat identification

USB malware investigation
