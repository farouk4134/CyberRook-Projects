Objective

Analyze obfuscated data commonly used by attackers, identify the obfuscation techniques applied, safely recover plaintext using CyberChef, and validate findings through controlled script execution.

Tools Used

CyberChef (Online / Offline)

PowerShell

Visual Studio (Script Review)

Isolated Windows VM

Key Concepts Applied

Obfuscation vs Encoding vs Encryption

Pattern recognition in malicious payloads

Layered deobfuscation

Safe execution in isolated environments

Phase 1: Identifying Obfuscation Techniques
Observed Indicators

While reviewing suspicious strings, the following patterns were used to identify likely obfuscation methods:

ROT-based ciphers

Characters shifted but readable

Spaces preserved

Base64

Long alphanumeric strings

Often ends with = or ==

XOR

Non-readable symbols

Output length matches input length

Layered obfuscation

Base64 combined with XOR or compression

Screenshot:
[Screenshot – Example of suspicious encoded strings]

Phase 2: Deobfuscation with CyberChef
XOR Obfuscation Test

Open CyberChef

Paste input text:

carrot supremacy


Add XOR operation

Set:

Key: a

Key format: HEX

Observe obfuscated output

Result:

ikxxe~*yzxogkis!


Screenshot:
[Screenshot – CyberChef XOR operation and output]

Phase 3: Detecting and Reversing Obfuscation
Technique Identification
Pattern	Likely Technique
One-letter shift	ROT1
gur, naq	ROT13
Alphanumeric + =	Base64
Random symbols	XOR

Once identified, reverse operations were applied:

From Base64

ROT13

XOR (same key)

Screenshot:
[Screenshot – From Base64 / ROT / XOR reversal in CyberChef]

Phase 4: Using CyberChef Magic Operation

When the obfuscation method was unclear:

Added Magic operation

Enabled Intensive Mode

Reviewed decoded outputs manually

Selected meaningful plaintext results

Screenshot:
[Screenshot – CyberChef Magic operation results]

Phase 5: Layered Obfuscation Analysis
Observed Payload

A heavily obfuscated string combining multiple techniques:

Base64

XOR

Gzip compression

Deobfuscation Order Applied

From Base64

XOR (known key)

Gunzip

Screenshot:
[Screenshot – Chained operations in CyberChef]

Phase 6: PowerShell Script Analysis (SantaStealer.ps1)
Environment Setup

Script opened in Visual Studio

Reviewed comments under "Start here"

Execution performed in isolated VM

Screenshot:
[Screenshot – Script opened in Visual Studio]

Execution – Part 1
cd .\Desktop\
.\SantaStealer.ps1


Deobfuscated C2 URL

Script executed successfully

First Flag Identified:

THM{C2_De0bfuscation_29838}


Screenshot:
[Screenshot – PowerShell execution and first flag]

Execution – Part 2 (API Key Obfuscation)

XOR applied to API key as instructed in script comments

Script re-executed after modification

Second Flag Identified:

THM{API_Obfusc4tion_ftw_0283}


Screenshot:
[Screenshot – Modified script and final execution]

Outcome

Successfully identified multiple obfuscation techniques

Recovered plaintext safely using CyberChef

Analyzed and executed malicious PowerShell code in a controlled environment

Demonstrated real-world Blue Team workflow for handling obfuscated payloads

Skills Demonstrated

Obfuscation detection

CyberChef recipe building

Layered payload analysis

Safe malware script handling

SOC-style investigation workflow


