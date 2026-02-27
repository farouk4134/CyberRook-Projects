internet, attackers scan for SSH and attempt access using stolen keys or weak passwords. In MITRE ATT&CK, this maps to External Remote Services under Initial Access.

Question: When did the Ubuntu user log in via SSH for the first time?

Answer: 2024-10-22

To answer this, we need authentication logs. On Ubuntu, SSH activity is recorded in:

/var/log/auth.log

Filtering for SSH daemon activity:
