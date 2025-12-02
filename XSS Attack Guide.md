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

# Project 2: Threat  Level Application - XSS Attacks

**Understanding the Working of XSS Attacks** 

Cross-site scripting ("XSS" or "CSS") attacks exploit vulnerabilities in dynamically generated web pages. This enables malicious attackers to inject client-side script into web pages viewed by other users.

**Lab Scenario**

As a SOC analyst, understanding XSS attack can help you identify the indication (IoC) of XSS attack on your organization’s website. Attacker takes advantage of improper data validation techniques and injects JavaScript, HTML, Flash, or any other type of executable code in the browser. Any such attempt could be a sign of XSS attack.

**Lab Objectives**

In this documentation, you will learn example of how an attacker can initiate XSS attack on target website and how the generated IoC will help you detect XSS Attack attempts in the later modules of this course.

**Environment & Tools**: 

- Windows server 2012
- Attacker’s Kali Linux Machine
- ***<Script> alert(”You are hacked”); </Script>***

**Steps Taken:**

1. Launch attacker’s Kali Linux Virtual machine.
2. Login to the attacker’s Kali Linux machine providing username & password
3. Launch **Firefox** browser and browse **www.luxurytreats.com** website. Suppose **bob** is an attacker and is already registered member in Luxury treats website. The username and password of bob is:
    
    **USERNAME**: bob **PASSWORD**: Passw0rd
    
4. Login to **www.luxurytreats.com** with bob credentials mentioned in previous step.
    
    > If save password pop-up appears, close it.
    > 
    
    If **Reader View** pop-up appears, close it.
    
5. LuxuryTreats home page appears. Click on Contact menu. To check if the contact page is vulnerable to XSS attack, an attacker will tr**y to inject Javascript code into the input fields of the** Contact page. To demonstrate this, type valid email address in Email: textbox and type the following JavaScript in the Comment: textbox. Click Save Comment. ***<Script> alert(”You are hacked”); </Script>***
    
    > If the Comment input in the contact page is not properly validated, then the typed JavaScript code is executed successfully in the browser, on click of Save Comment.
    > 

![XSS.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/c00812a2-fbb1-4ce5-bb83-c9097d7f0523/c0c940c2-aa0f-4d40-8d8e-cc8e3ef17f44/XSS.jpg)

1. A pop-up appears on the page displaying a message **You are hacked**. Click **OK**. This indicates that XSS attack is successfully attempted. In this way, the attacker can inject malicious script and perform XSS attack on the target site. Attacker can use this technique to perform session hijacking, phishing, etc.

![xss2.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/c00812a2-fbb1-4ce5-bb83-c9097d7f0523/9140cbf5-d536-4d49-abe2-e85967bb70f4/xss2.jpg)

**Results**:

> Any occurrence of such XSS attack attempts in the web server logs can be treated as IoC of XSS attack. In the labs of modules 3 and 4, you will see how such XSS attack strings are logged in web server logs and how to filter logs comprising such IoCs and monitor such IoCs to detect XSS attack attempts.
> 

**Insights & Learning Points**: 

In this exercise you have learnt how an attacker can initiate XSS attack on target website.

**Assessment 2.2.1:**

Identify the field (textbox) in the Contact page of [Luxurytreats.com](http://luxurytreats.com/), which is vulnerable to an XSS attack.

Answer: Leave Comment
