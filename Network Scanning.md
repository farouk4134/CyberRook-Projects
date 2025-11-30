**Overview:** 
This documentation compiles all lab practices completed as part of the **EC-Council Certified SOC Analyst (CSA)** course. Each lab in this collection builds foundational and intermediate skills for Security Operations Center (SOC) analysis, progressing from Tier 1 to Tier 2 responsibilities. The labs cover various aspects of threat detection, incident response, and security information and event management (SIEM), providing practical experience in tools and techniques critical to SOC operations.

**Purpose**

The purpose of this document is to: 

- Record and review each step of the labs for better understanding.
- Serve as a structured portfolio of SOC analyst skills, showcasing hands-on experience with tools and incident handling.
- Facilitate continuous improvement by tracking challenges, solutions, and insights gained through each exercise.

**Scope**

1. **Objective**: The goal of the lab.
2. **Environment & Tools**: Key tools and configurations.
3. **Steps Taken**: Step-by-step actions with relevant screenshots or commands.
4. **Results**: Summary of findings or actions taken.
5. **Insights & Learning Points**: Key takeaways and possible improvements.

This documentation supports the transition from foundational SOC analyst skills to more advanced incident response capabilities, making it a useful reference for future studies and real-world applications in SOC environments.

# Project 3: Network  Level Threats - Network Scanning

**Understanding the Working of Network Scanning Attacks**

Network scanning attack helps attackers to organize an attack on the targeted network.

**Lab Scenario**

Attackers scan the target network for extracting valuable information about the hosts, ports, and services in the network. This enables attackers to decide on the various techniques and tools to be used to perform the desired attack. As a SOC analyst, you should be aware of such network scanning attempts. If detected at the early stage, it will help you to prevent future attacks.

**Lab Objectives**

In this lab, you will learn example of how an attacker runs various network scan attempts to find open ports and services running on the target and how the generated IoC will help you detect Network scanning attempts in the later modules of this course.

This lab will demonstrate the following network scan attempts to find open ports and services running on the target.

- SYN Scan Attempt
- TCP Full Connect Scan Attempt
- UDP Scan Attempt

**Environment & Tools**: 

- Windows server 2012
- Attacker’s Kali Linux Machine
- Nmap -sS 10.10.10.12
- nmap -sT -T4 10.10.10.12
- nmap -sU -T5 10.10.10.12

**Steps Taken:**

1. Launch attacker’s Kali Linux Virtual machine.
2. Login to the attacker’s Kali Linux machine providing username & password
3. To find exploitable vulnerabilities on targeted system **WinServer2012** attacker can perform various network scans. To demonstrate this, open **Terminal** (in Kali).
4. To perform **SYN** Scan on the target machine, type the command **nmap -sS 10.10.10.12** in terminal and then press **Enter**
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/c00812a2-fbb1-4ce5-bb83-c9097d7f0523/ddedbef9-7934-4b07-8405-193b2a3aacb4/image.png)
    
    > SYN Scan gets information from the remote host without the complete TCP handshake process.
    > 
5.  This displays the output of **SYN** scan giving details, state, and services running on the target system.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/c00812a2-fbb1-4ce5-bb83-c9097d7f0523/62a95626-1154-495f-99b7-12551f313aab/image.png)

1. To perform **TCP Full** connect scan, type the command **nmap -sT -T4 10.10.10.12** and press **Enter**.

> TCP Full scan will determine if a port is open on the target system.
> 

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/c00812a2-fbb1-4ce5-bb83-c9097d7f0523/174c2301-7c25-4401-81ca-4d6a1a01395e/image.png)

1. This displays the **TCP Full** scan giving details of state and services running on the target system.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/c00812a2-fbb1-4ce5-bb83-c9097d7f0523/4941a403-e48b-43a6-848b-4d43fb137931/image.png)

1. To perform **UDP scan** type **nmap -sU -T5 10.10.10.12** in the terminal and hit **Enter**.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/c00812a2-fbb1-4ce5-bb83-c9097d7f0523/e12bae27-45e1-40e3-a6ce-b93a660ca1bf/image.png)

1. This will display output of UDP scan. It shows that UDP port 137 is open. Close all the open windows

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/c00812a2-fbb1-4ce5-bb83-c9097d7f0523/5d6883c2-1151-4ff2-92ff-78d06721983a/image.png)

1. This infers that the attacker can get valuable information from such network scanning attempts.

**Results**:

> Any occurrence of such network scanning attempts found in the logs of preconfigured IDS /Firewalls can be treated as IoC of reconnaissance. In the labs of modules 3 and 4, you will see how IDS is configured to log such network scanning attempts and how to filter logs comprising such IoCs and monitor such IoCs to detect network / port scanning attempts.
> 

**Insights & Learning Points**: 

In this exercise you have learnt how attacker runs various network scan attempts to find port and services running on the target.

- SYN Scan Attempt
- TCP Full Connect Scan Attempt
- UDP Scan Attempt

**Assessment 2.3.1:**

Perform a SYN scan on 10.10.10.12 and identify the port that is open among 22, 80, and 111.

Answer: 80

**Assessment 2.3.2:**

Perform a TCP full connect scan on 10.10.10.12 and identify the port that is open among 443, 445, and 3389.

Answer: 445

**Assessment 2.3.3:**

Perform a UDP scan on 10.10.10.12 and identify the port that is open among 111, 137 and 139.

Answer: 137
