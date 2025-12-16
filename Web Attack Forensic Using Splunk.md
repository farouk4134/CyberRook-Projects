Blue Team Investigation: Web Command Injection via Apache (Splunk)
Objective

Investigate suspicious Apache web activity, identify command injection attempts, trace OS-level execution, decode Base64 payloads, and reconstruct the full attack chain using Splunk.

Tools & Environment

SIEM: Splunk

Web Logs: Apache Access & Error Logs

Host Logs: Sysmon

Attack Vector: Web-based command injection via CGI

Index Names:

windows_apache_access

windows_apache_error

windows_sysmon

Step 1: Log in to Splunk

Open Firefox on the AttackBox.

Navigate to:

http://MACHINE_IP:8000


Log in with:

Username: Blue

Password: Pass1234

Set the time range to Last 7 days or All time.

📸 Screenshot: Splunk login page
📸 Screenshot: Splunk Search dashboard

Step 2: Detect Suspicious Web Commands (Apache Access Logs)
Goal

Identify HTTP requests attempting command execution through web parameters.

Splunk Query
index=windows_apache_access (cmd.exe OR powershell OR "powershell.exe" OR "Invoke-Expression")
| table _time host clientip uri_path uri_query status

What to Look For

Long or suspicious query strings

Base64-encoded PowerShell commands

Requests targeting CGI scripts (e.g. hello.bat)

📸 Screenshot: Apache access log results

Step 3: Decode Base64 Payloads
Identified Encoded String
VABoAGkAcwAgAGkAcwAgAG4AbwB3ACAATQBpAG4AZQAhACAATQBVAEEASABBAEEASABBAEEA

Decode Using

https://www.base64decode.org/

Decoded Result
This is now Mine! MUHAHAHAHA

Conclusion

The payload confirms attacker intent and successful command execution attempt.

📸 Screenshot: Base64 decoding result

Step 4: Inspect Apache Error Logs
Goal

Confirm whether malicious requests reached backend execution.

Splunk Query
index=windows_apache_error ("cmd.exe" OR "powershell" OR "Internal Server Error")

Actions

Switch View → Raw

Look for:

500 Internal Server Error

References to cmd.exe or powershell

Interpretation

A 500 error after a malicious request indicates backend processing and likely exploitation.

📸 Screenshot: Apache error log showing 500 errors

Step 5: Trace Malicious Process Creation (Sysmon)
Goal

Identify OS-level processes spawned by Apache.

Splunk Query
index=windows_sysmon ParentImage="*httpd.exe"

Switch View

View → Table

Red Flags

httpd.exe spawning:

cmd.exe

powershell.exe

Example Indicator
ParentImage: C:\Apache24\bin\httpd.exe
Image:       C:\Windows\System32\cmd.exe


📸 Screenshot: Sysmon showing Apache spawning cmd.exe

Step 6: Confirm Attacker Enumeration Activity
Goal

Detect post-exploitation reconnaissance.

Splunk Query
index=windows_sysmon *cmd.exe* *whoami*

Why This Matters

Attackers commonly run whoami to confirm execution context.

Confirms successful command execution on host.

📸 Screenshot: whoami command execution in Sysmon

Step 7: Identify Encoded PowerShell Execution
Goal

Check if encoded PowerShell payloads actually ran.

Splunk Query
index=windows_sysmon Image="*powershell.exe"
(CommandLine="*enc*" OR CommandLine="*-EncodedCommand*" OR CommandLine="*Base64*")

Expected Result

No results → encoded payload did NOT execute

Results found → decode and inspect immediately

📸 Screenshot: Encoded PowerShell detection (or no results)

Attack Chain Summary
Stage	Evidence
Initial Access	Malicious HTTP requests via Apache CGI
Execution	Apache spawned cmd.exe
Obfuscation	Base64-encoded PowerShell payload
Reconnaissance	whoami command executed
Impact	Confirmed command injection attempt
Key Takeaways

Long Base64 strings in HTTP requests are high-risk indicators

Apache spawning system binaries confirms exploitation

Sysmon process lineage is critical for validation

Correlating web + host logs gives full attack visibility

Status

✔ Command injection attempt identified
✔ Payload decoded
✔ Compromised host confirmed
✔ Attack chain reconstructed
