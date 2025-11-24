# **Windows Security Logging & Brute Force Detection**

## **Overview**

This lab demonstrates configuring Windows security audit policies, generating brute force login attempts, and analyzing the resulting event logs.

The goal is to showcase the ability to detect unauthorized login attempts using Windows Event Viewer.

---

## **Lab Environment**

- **Target OS:** Windows Server 2012 / 2022
- **Attack Machine:** Kali Linux
- **Tools Used:** Hydra, FTP client, Event Viewer
- **Key Configurations:**
    - Audit policy enabled for **Success** and **Failure** logon events
    - FTP server configured for testing login attempts

---

## **Lab Objectives**

1. Enable auditing for successful and failed logins in Windows.
2. Generate logs using brute force login attempts via Hydra.
3. Analyze Windows Event Viewer logs to detect and interpret attack activity.

---

## **1. Configure Windows Audit Policy**

**Steps Taken:**

1. Open **Local Security Policy** → **Local Policies** → **Audit Policy**.
2. Double-click **Audit logon events**.
3. Enable both **Success** and **Failure** checkboxes.
4. Apply and close the policy window.

**Screenshot:**

![image.png](attachment:f8ea87c6-6f4e-48ee-b2ef-2a87ab54791d:image.png)

---

![image.png](attachment:8cb2258f-7369-4a5a-960c-250a0ea39875:image.png)

![image.png](attachment:8bcaf167-2955-471d-b443-fd00504024fb:image.png)

## **2. Generate Brute Force Attempts**

**Steps Taken:**

1. Launch Kali Linux.
2. Open terminal and run:

```bash
hydra -L /root/Wordlist/userlist.txt -P /root/Wordlist/pass.txt ftp://<target_IP>

```

1. Record login attempts and capture successful credentials.

**Screenshot:**

![image.png](attachment:dfd5021c-8518-474d-9404-9fefa025b266:image.png)

---

![image.png](attachment:20e566ef-19e2-46de-ad80-b7368a1687e8:image.png)

## **3. Analyze Windows Event Logs**

**Steps Taken:**

1. Open **Event Viewer** → **Windows Logs** → **Security**.
2. Filter **Event ID 4625** to view failed login attempts.
3. Check **Target Username** and **Source IP** to correlate with attack.

**Screenshot:**

![image.png](attachment:2869bb7a-7ef4-48e5-993a-7debeab87b28:image.png)

---

![image.png](attachment:4017fb0f-3178-4ab5-9928-2eec4cadecaa:image.png)

## **Results**

- Number of failed login attempts: *(insert number)*
- Successful login found: *(username/password if applicable)*
- Source IP of attacks: *(insert Kali IP)*

---

## **Insights & Learning Points**

- Audit policies effectively record both success and failure login attempts.
- Event Viewer can be used to trace brute force attempts and correlate attack details.
- Understanding log structure is crucial for incident detection and response.
