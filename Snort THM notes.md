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
