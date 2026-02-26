Executive summary

Linux hosts generate a lot of useful telemetry: system events, authentication activity, package installs, shell history, and deep audit/kernel traces. For a SOC analyst, these logs are the “story” of what happened. When you correlate them, you can rebuild timelines, catch suspicious behavior, and back your containment decisions with evidence.

This is a polished walkthrough of the TryHackMe: Linux Logging for SOC room, written like practical SOC notes.

Task 2 — Working with text logs
Question: Which time server domain did the VM contact to sync its time?

Answer: ntp.ubuntu.com

Time sync is a big deal because if clocks drift, correlation across hosts becomes unreliable. On Debian/Ubuntu, time sync events commonly appear in /var/log/syslog. The fastest approach is to search for NTP-related entries.

Command used:

grep -i ntp /var/log/syslog

This returns entries showing the VM contacting ntp.ubuntu.com.

Why it matters

Confirms the host is using a trusted time source

Helps detect suspicious time config changes (often used to mess with investigations)

Question: What is the kernel message from Yama in /var/log/syslog?

Answer: Yama: becoming mindful.

Yama is a Linux Security Module (LSM) that tightens ptrace/debugging behavior. To find its kernel messages, search syslog directly.

Command used:

grep -i yama /var/log/syslog

The log contains the message: Yama: becoming mindful. which indicates Yama has initialized.

Why it matters

Kernel security modules affect attacker capability (like memory inspection / debugging)

Seeing Yama suggests an extra defensive layer is active on the host

Task 3 — Authentication logs

Authentication logs are top priority because they include SSH attempts, sudo activity, and user management changes. On Debian/Ubuntu, the main file is:

/var/log/auth.log

Question: Which IP address failed to log in on multiple users via SSH?

Answer: 10.14.94.82

This is classic brute-force probing: repeated failures against multiple usernames. Filter SSH failures in the auth log.

Command used:

cat /var/log/auth.log | grep sshd | grep -E "Failed"

You’ll see multiple Failed password entries from 10.14.94.82 targeting different users.

Why it matters

Likely brute force / credential stuffing

SOC actions: block IP, check for successful login from same IP, pivot into other telemetry

Question: Which user was created and added to the sudo group?

Answer: xerxes

User creation and privilege changes are logged in the same auth log. Search for user management commands.

Command used:

cat /var/log/auth.log | grep -E "(useradd|usermod)"

You’ll find entries showing the user creation and that xerxes was added to sudo.

Why it matters

A new privileged user is high severity if unauthorized

Often indicates persistence after compromise

Task 4 — Common Linux logs

This task pivots into logs that show system changes (packages installed) and user activity (shell history).

Question: According to the VM’s package manager logs, which version of unzip was installed?

Answer: 6.0-28ubuntu4.1

On Debian-based systems, package installs are tracked in /var/log/dpkg.log.

Command used:

cat /var/log/dpkg.log | grep unzip

The output includes the version string: 6.0-28ubuntu4.1.

Why it matters

Shows what tooling appeared on the box (legit or attacker-installed)

Helps detect attacker staging (installing utilities for extraction, scanning, exfil, etc.)

Question: What is the flag you see in one of the users’ bash history?

Answer: THM{note_to_remember}

Bash history usually lives in ~/.bash_history, but not every user will have it populated. If user histories come up empty, check root.

Command used:

sudo grep -i "THM{" /root/.bash_history

This returns the line containing the flag.

Why it matters

History artifacts mimic real-world “operator traces”

Root history can expose privileged attacker actions, so it’s always worth checking

Task 5 — Runtime Monitoring

This section explains why “normal logs” aren’t enough for runtime events like process execution, file access, and network activity.

Question: Which Linux system call is commonly used to execute a program?

Answer: execve

Explanation
execve is the syscall used to execute a program by replacing the current process image. If you’re monitoring process creation at a low level, execve is one of the core signals.

Question: Can a typical program open a file or create a process bypassing system calls?

Answer: Nay

Explanation
On a normal Linux system, user-space apps must go through the kernel using syscalls for actions like open/read/exec/connect. This is why syscall-based logging (like auditd) is powerful.

Task 6 — Using auditd

auditd records syscall activity in a detailed and consistent way: process execution, file access, arguments, user context, and timestamps.

Question: When was the secret.thm file opened for the first time?

Answer: 08/13/25 18:36:54

This event is logged using the file_thmsecret key in the lab. The cleanest approach is filtering by the key with ausearch.

Command used:

ausearch -i -k file_thmsecret

From the output, the first access timestamp is 08/13/25 18:36:54.

Why it matters

Audit logs give authoritative timestamps for sensitive file access

You get the process and user context, not just “a file changed”

Question: What is the original file name downloaded from GitHub via wget?

Answer: naabu_2.3.5_linux_amd64.zip

The wget process creation is logged with the proc_wget key.

Command used:

ausearch -i -k proc_wget

In PROCTITLE / EXECVE, you’ll see the full command including the filename naabu_2.3.5_linux_amd64.zip.

Why it matters

Downloaded tooling is a strong signal of recon or post-compromise activity

Helps identify what capability the attacker brought onto the host

Question: Which network range was scanned using the downloaded tool?

Answer: 192.168.50.0/24

Search audit events for naabu execution.

Command used:

ausearch -i | grep -i naabu

The command line includes the target range 192.168.50.0/24.

Why it matters

Internal scanning can indicate recon for lateral movement

SOC should correlate with firewall/IDS logs and investigate targets in that CIDR

Conclusion & practical takeaways

This room shows a layered Linux triage approach:

Start with syslog for system-level events and time sync

Use auth.log for SSH failures, sudo usage, and user creation

Pivot to package logs and bash history for context and artifacts

Use auditd as “ground truth” for process execution, arguments, and file access

Practical reminders:

Validate time sources before trusting timelines

Treat new privileged users as high severity until proven legitimate

If user history is empty, check root and other shells

Audit logs are your best source for command arguments and first-seen access times
