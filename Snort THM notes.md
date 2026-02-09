# Snort (TryHackMe) — Notes

**Room:** [https://tryhackme.com/room/snort](https://tryhackme.com/room/snort)
**Goal:** Learn Snort basics: sniffing traffic, logging packets, running IDS detection, and writing simple rules.

---

## Quick Overview

**Snort** is a rule-based **NIDS/NIPS** tool:

* **NIDS**: detects suspicious traffic and alerts
* **NIPS**: can run inline to drop/block traffic (when configured)

### Snort can run in 3 common ways

* **Sniffer mode**: print packets to terminal
* **Packet logger mode**: save traffic to logs/pcaps for later review
* **IDS/IPS mode**: generate alerts based on rules + config

---

## Task 2 (Warm-up)

Run the script:

```bash
cd Task-Exercises
./.easy.sh
```

Output: **Too Easy!**

---

## IDS vs IPS (short)

### IDS types

* **NIDS**: monitors network traffic across a subnet
* **HIDS**: monitors a single host

### IPS types

* **NIPS**: blocks traffic at the network level (inline)
* **HIPS**: blocks activity on a single host
* **WIPS**: focuses on wireless threats
* **Behavior-based (NBA)**: learns “normal” traffic (baselining) then flags anomalies

### Detection techniques

* **Signature-based**: matches known patterns
* **Behavior-based**: detects abnormal vs baseline
* **Policy-based**: detects policy violations

---

## Task 4 — First Interaction with Snort

### Check Snort version/build

```bash
snort -V
```

Build number: **149**

### Test config + count rules (default config)

```bash
snort -T -c /etc/snort/snort.conf
```

Rules loaded: **4151**

### Test second config

```bash
snort -T -c /etc/snort/snortv2.conf
```

Rules loaded: **1**

---

## Task 5 — Mode 1: Sniffer Mode

Useful flags:

* `-v` verbose (headers)
* `-d` payload
* `-e` link-layer headers
* `-X` full packet (hex + ascii)
* `-i <iface>` choose interface

Examples:

```bash
sudo snort -v
sudo snort -d
sudo snort -de
sudo snort -X
sudo snort -v -i eth0
```

---

## Task 6 — Mode 2: Packet Logger Mode

Useful flags:

* `-l <dir>` output directory (default: `/var/log/snort`)
* `-K ASCII` log in readable text
* `-r <file>` read from log/pcap
* `-n <num>` stop after N packets

Capture to current directory:

```bash
sudo snort -dev -K ASCII -l .
```

Generate traffic (choose TASK-6 exercise):

```bash
sudo ./traffic-generator.sh
```

### Findings (from generated logs)

* Source port used to connect to **port 53**: **3009**
* IP ID of the **10th packet**: **49313**
* Referer of the **4th packet**: **[http://www.ethereal.com/development.html](http://www.ethereal.com/development.html)**
* Ack number of the **8th packet**: **0x38AFFFF3**
* Number of packets to **TCP port 80**: **41**

Examples used:

```bash
snort -r snort.log.1640048004 -n 10
sudo snort -Xr snort.log.1640048004 -n 4
sudo snort -r snort.log.1640048004 -n 8
sudo snort -r snort.log.1640048004 'tcp port 80'
```

---

## Task 7 — Mode 3: IDS/IPS Mode

Key IDS flags:

* `-c <conf>` config file
* `-A <mode>` alert format (`console`, `fast`, `full`, `cmg`, `none`)
* `-N` disable logging
* `-D` background
* `-l <dir>` write logs

Example run:

```bash
sudo snort -c /etc/snort/snort.conf -A full -l .
```

Generate traffic (choose TASK-7 exercise):

```bash
sudo ./traffic-generator.sh
```

Result:

* Detected **HTTP GET** methods: **2**

### IPS / inline example (advanced)

```bash
sudo snort -c /etc/snort/snort.conf -q -Q --daq afpacket -i eth0:eth1 -A console
```

---

## Task 8 — PCAP Investigation Mode

Read PCAP with config + alert output:

```bash
sudo snort -c /etc/snort/snort.conf -q -r icmp-test.pcap -A console -n 10
```

Read multiple pcaps:

```bash
sudo snort -c /etc/snort/snort.conf -q --pcap-list="icmp-test.pcap http2.pcap" -A console --pcap-show
```

### Results (mx pcaps)

**mx-1.pcap (default config)**

* Alerts: **170**
* TCP Segments Queued: **18**
* HTTP response headers extracted: **3**

**mx-1.pcap (snortv2.conf)**

* Alerts: **68**

**mx-2.pcap (default config)**

* Alerts: **340**
* TCP packets detected: **82**

**mx-2 + mx-3 (default config)**

* Alerts: **1020**

---

## Task 9 — Rule Structure (what I actually used)

Rule format:

```
action proto src_ip src_port direction dst_ip dst_port (options)
```

Common options:

* `msg` alert message
* `sid` unique rule id (custom rules use high numbers)
* `rev` rule version (increase when changing rule)

### 1) Filter by IP ID (35369)

`local.rules`:

```snort
alert tcp any any <> any any (msg:"ID Test"; id:35369; sid:10000000001; rev:1;)
```

Run:

```bash
sudo snort -c local.rules -A full -l . -r task9.pcap
```

### 2) SYN flag packets

```snort
alert tcp any any <> any any (msg:"SYN FLAG"; flags:S; sid:10000000002; rev:1;)
```

Detected: **1**

### 3) Push-Ack packets

```snort
alert tcp any any <> any any (msg:"Push-Ack FLAG"; flags:PA; sid:10000000003; rev:1;)
```

Detected: **216**

Count alerts quickly:

```bash
cat alert | grep "Push-Ack" | wc -l
```

### 4) Same source + destination IP

```snort
alert ip any any <> any any (msg:"SAME IP"; sameip; sid:10000000004; rev:1;)
```

Detected: **10**

**Note:** after modifying a rule, update: **`rev`**

---

## Task 10 — Practical reminders

* Main config: **`/etc/snort/snort.conf`**
* Custom rules: **`/etc/snort/rules/local.rules`**
* Test config before running:

```bash
snort -T -c /etc/snort/snort.conf
```

* If a rule works but you don’t want it active, **comment it out** (don’t delete)
* Build rules step-by-step to avoid syntax mistakes

