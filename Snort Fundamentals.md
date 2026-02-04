IDS Fundamentals (TryHackMe) — Notes
Room: IDS Fundamentals
Focus: IDS basics + hands-on Snort (rules, logging, and PCAP review)
What I covered


What an IDS does (detect + alert, no blocking)


IDS deployment types: HIDS vs NIDS


Detection approaches: Signature, Anomaly, Hybrid


Snort basics:


Where configs and rules live


How to write and test a rule


How to run Snort on live traffic and PCAP files





Key Concepts (quick)


IDS = Detect + Alert
It doesn’t stop traffic automatically (that’s IPS).


Q: Can an IDS prevent a threat after detecting it?
A: Nay

IDS Types
Deployment


HIDS: agent on a single host, deep host visibility, harder at scale


NIDS: monitors traffic across the network, centralized detection


Q: Which IDS detects threats across the whole network?
A: Network Intrusion Detection System (NIDS)
Detection


Signature-based: fast, detects known patterns, weak on zero-days


Anomaly-based: detects unusual behavior, can catch unknown threats, more false positives


Hybrid: combines both


Q: Which IDS uses both signature + anomaly detection?
A: Hybrid IDS

Snort (IDS Example)
Snort can run in multiple modes, but the most common IDS use is NIDS mode.
Q: Which Snort mode logs traffic into PCAP format?
A: Packet Logging Mode
Q: What’s Snort’s primary mode?
A: Network Intrusion Detection System (NIDS) mode

Snort File Locations
Snort main directory:


/etc/snort


Rules directory:


/etc/snort/rules/


Custom rules file:


/etc/snort/rules/local.rules


Q: Where is the main Snort directory?
A: /etc/snort
Q: Which rule field indicates revision number?
A: rev

Rule Format (what matters)
Snort rules follow this structure:


Action (alert/log/pass/drop)


Protocol (tcp/udp/icmp)


Source (IP + port)


Direction (-> or <-)


Destination (IP + port)


Metadata inside (...) like:


msg:"..." (alert message)


sid: (signature ID)


rev: (rule revision)





Practical: Create + Test a Custom Rule
1) Edit custom rule file
sudo nano /etc/snort/rules/local.rules

2) Add this rule (ICMP loopback ping detection)
alert icmp any any -> 127.0.0.1 any (msg:"Loopback Ping Detected"; sid:10003; rev:1;)

3) Run Snort for detection (live capture)
sudo snort -q -l /var/log/snort -i lo -A console -c /etc/snort/snort.conf

4) Trigger it (ping loopback)
ping 127.0.0.1

Expected: Snort prints "Loopback Ping Detected" alerts.
Q: Which protocol is in the sample rule?
A: ICMP

Snort on PCAP Files (forensics workflow)
Run Snort against a capture:
sudo snort -q -l /var/log/snort -r Task.pcap -A console -c /etc/snort/snort.conf


Lab Exercise: Intro_to_IDS.pcap
Steps
cd /etc/snort
sudo snort -q -r Intro_to_IDS.pcap -A console -c /etc/snort/snort.conf

Findings
Q: IP that attempted SSH connection?
A: 10.11.90.211
Q: Other rule message detected (besides SSH)?
A: Ping Detected
Q: SID of the SSH rule?
A: 1000002
