# 

Use this as-is for your documentation.

---

## **1. Install FTP Server Role**

1. Open **Server Manager** (or “Turn Windows features on/off” on Windows 11).
2. Enable:
    - **IIS**
    - **FTP Server**
        - FTP Service
        - FTP Extensibility
3. Apply and restart if needed.

![image.png](screenshots/loggins/Winfeatures.png)

---

## **2. Create the FTP Site**

1. Open **IIS Manager**
2. Right-click **Sites > Add FTP Site**
3. Fill the fields:
    - **Site Name:** `Admin11` (or any name you want)
    - **Physical Path:** `C:\inetpub\`
4. Click **Next**
![image.png](screenshots/loggins/Winfeatures.png)
---

## **3. Bindings**

- **IP Address:** your VM or machine IP (example: `192.168.0.186`)
    
    *(Note: write it as `192.168.0.186 )`*
    
- **Port:** `21`
- **SSL:** No SSL (for lab)

---

## **4. Authentication & Authorization**

- **Authentication:** Enable **Basic**
- **Authorization:**
    - Allow access to: Specified User
    - Permissions: **Read** and **Write**

Finish the wizard.

![image.png](screenshots/loggins/FTPdone.png)

---

---

## **5. Open Windows Firewall Ports**

**These are the corrected commands.**

The reason your commands failed earlier is because of missing quotes and spacing.

### **Run CMD as Administrator:**

```
netsh advfirewall firewall add rule name="FTP" dir=in action=allow protocol=TCP localport=21

```

```
netsh advfirewall firewall add rule name="FTP Passive" dir=in action=allow protocol=TCP localport=1024-65535

```

Both should complete successfully.

If Powershell complains, use `netsh` in **CMD**, not Powershell.

---

## **6. Verify the FTP Service**

In IIS Manager → **FTP site → Manage FTP Site → Start**

Or check command line:

```
netstat -ano | findstr :21

```

You should see the FTP listener.

![image.png](screenshots/loggins/confirm.png)

---

## **7. Test the FTP Login**

From another machine or your Kali box:

```
ftp 192.168.0.186 (or any Ip you used)

```

Enter the Windows username + password.

If it logs in and you see directories → your server is ready.

![image.png](screenshots/loggins/kalistep.png)
