Network Threat Hunting Case Study – C2 Detection with Zeek & RITA
Objective

Identify potential Command and Control activity by converting raw PCAP traffic into Zeek logs and analyzing it with RITA. The goal was to surface suspicious external communications without relying on signatures.

Tools & Environment

Zeek (PCAP → structured network telemetry)

RITA (C2 analytics and scoring)

Linux terminal (TryHackMe lab)

Threat intel validation via VirusTotal

Investigation Workflow
1. PCAP Normalization with Zeek

I started with a raw packet capture containing suspected malware traffic. Instead of inspecting packets manually, I converted the PCAP into structured Zeek logs to extract higher-level network behavior.

Command used:

zeek readpcap pcaps/AsyncRAT.pcap zeek_logs/asyncrat


This produced core logs such as:

conn.log for connection metadata

dns.log for name resolution behavior

http.log and ssl.log for application-layer context

This step turned unstructured packet data into analyzable telemetry.

2. RITA Ingestion & Analysis

With the Zeek logs ready, I imported them into RITA to apply behavioral analytics rather than static indicators.

rita import --logs ~/zeek_logs/asyncrat/ --database asyncrat


RITA correlated connection timing, frequency, DNS patterns, certificate metadata, and external reputation data to score potential C2 behavior.

3. Findings & Interpretation

Using rita view asyncrat, two high-interest entries immediately stood out.

Finding 1 – Suspicious FQDN

Destination: sunshine-bizrate-inc-software[.]trycloudflare[.]com

Indicators:

Unusually long FQDN

Low prevalence (few internal hosts communicating)

Rare TLS signature

Validation:

External threat intel confirmed the domain as malicious

This strongly aligned with C2 infrastructure hiding behind a cloud tunneling service.

Finding 2 – Suspicious External IP

Destination IP: 91.134.150.150

Indicators:

Long-lived connections

Non-standard ports

High connection duration

Validation:

Flagged as malicious on VirusTotal

The traffic pattern was consistent with beaconing behavior rather than normal application traffic.

Analyst Assessment

Even with a small dataset, RITA successfully highlighted:

Beacon-like communication intervals

Low-prevalence external endpoints

Anomalous TLS and port usage

This demonstrates how behavioral analytics can surface threats even when payloads are encrypted and signatures are unavailable.

Key Skills Demonstrated

PCAP to Zeek log conversion

Network telemetry interpretation

C2 detection using behavioral analytics

Threat intel correlation

Analyst-driven triage and prioritization



Finding 1 – Suspicious FQDN

Destination: sunshine-bizrate-inc-software[.]trycloudflare[.]com

Indicators:

Unusually long FQDN

Low prevalence (few internal hosts communicating)

Rare TLS signature

Validation:

External threat intel confirmed the domain as malicious

This strongly aligned with C2 infrastructure hiding behind a cloud tunneling service.

Finding 2 – Suspicious External IP

Destination IP: 91.134.150.150

Indicators:

Long-lived connections

Non-standard ports

High connection duration

Validation:

Flagged as malicious on VirusTotal

The traffic pattern was consistent with beaconing behavior rather than normal application traffic.

Analyst Assessment

Even with a small dataset, RITA successfully highlighted:

Beacon-like communication intervals

Low-prevalence external endpoints

Anomalous TLS and port usage

This demonstrates how behavioral analytics can surface threats even when payloads are encrypted and signatures are unavailable.

Key Skills Demonstrated

PCAP to Zeek log conversion

Network telemetry interpretation

C2 detection using behavioral analytics

Threat intel correlation

Analyst-driven triage and prioritization

Notes

Screenshots included:

Zeek log generation output

RITA severity dashboard

Threat modifier breakdown

External IoC validation
