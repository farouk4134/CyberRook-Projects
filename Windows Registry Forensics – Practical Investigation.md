**Objective**

Analyze offline Windows Registry hives from a compromised host to extract forensic artifacts relevant to incident response and system compromise analysis.

**Tools Used**

Registry Explorer (Eric Zimmerman)

Offline Registry Hives (SYSTEM, SOFTWARE, SAM, NTUSER.DAT, USRCLASS.DAT)

**Environment**

Target Host: dispatch-srv01

Registry Hive Location:
C:\Users\Administrator\Desktop\Registry Hives

Analysis Method: Offline registry analysis (non-intrusive)

**Step 1: Launch Registry Explorer**

Open Registry Explorer from the taskbar on the analysis machine.

![image.png](screenshots/splunk/login.png)

**Step 2: Load Offline Registry Hives**

Click File → Load Hive

Navigate to:

C:\Users\Administrator\Desktop\Registry Hives


Select a hive (example: SYSTEM)

Hold SHIFT and click Open

This forces transaction log replay (.LOG1, .LOG2) to load a clean hive state.

Confirm successful transaction replay

Repeat for all relevant hives:

SYSTEM

SOFTWARE

SAM

NTUSER.DAT

USRCLASS.DAT

📸 Screenshot: Transaction logs successfully replayed

**Step 3: System Identification (Hostname)**

Hive: SYSTEM
Registry Path:

ROOT\ControlSet001\Control\ComputerName\ComputerName


Result:

Hostname identified: DISPATCH-SRV01

📸 Screenshot: ComputerName registry key and value

**Step 4: Registry Key Navigation & Search**

Used Registry Explorer search bar for rapid pivoting

Validated keys via:

Manual path traversal

Bookmark shortcuts (Available Bookmarks tab)

This method speeds up investigation during time-sensitive incident response.

Step 5: Forensic Focus Areas Touched

During analysis, the following registry artifact categories were reviewed or validated as available for investigation:

System identity and configuration

Installed applications

User activity artifacts

Autostart and persistence locations

Historical execution traces

(No modification performed on original evidence)

Key Analyst Takeaways

Offline registry analysis prevents evidence contamination

Transaction log replay is critical for accurate hive reconstruction

Registry Explorer enables fast pivoting, binary parsing, and forensic-safe analysis

Registry artifacts provide strong context for timeline reconstruction

Investigation Context

Host showed abnormal activity starting 21 October 2025

Registry analysis supports broader incident timeline correlation alongside:

Event logs

Sysmon telemetry

Network artifacts

Skills Demonstrated

Windows Registry forensics

Offline evidence handling

Registry Explorer usage

Incident investigation methodology

Artifact-based system profiling
