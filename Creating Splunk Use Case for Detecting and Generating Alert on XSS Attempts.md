IIS logs can act as event data source to detect XSS attempts.

**Lab Scenario**

The presence of specific patterns or signatures in the IIS logs denotes the XSS attempts. You can identify such patterns and create alerts to detect XSS attempt. As a SOC analyst, you should be able to identify the monitoring need, event source, and minimum configuration needed to detect these types of attempts.

**Lab Objectives**

The objective of this lab is to create Splunk SIEM use case for detecting XSS attempts.

**Lab Tasks**

- [ ]  Click [WinServer2012](https://labclient.labondemand.com/Instructions/ea95f72f-9a17-488e-a8f0-3f1b3e3ebbae#) machine
    
    > If WinServer2012 is already launched, skip to step 3
    > 
    
    Click [Ctrl+Alt+Delete](https://labclient.labondemand.com/Instructions/ea95f72f-9a17-488e-a8f0-3f1b3e3ebbae#) link.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/0evlpmpl.jpg)
    
- [ ]  By default **Administrator** account is selected, click Pa$$w0rd and press **Enter** to login.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/jym20uyi.jpg)
    
- [ ]  Click [SIEM1](https://labclient.labondemand.com/Instructions/ea95f72f-9a17-488e-a8f0-3f1b3e3ebbae#) machine
    
    > If SIEM1 is already launched, skip to step 5
    > 
    
    Click [Ctrl+Alt+Delete](https://labclient.labondemand.com/Instructions/ea95f72f-9a17-488e-a8f0-3f1b3e3ebbae#) link.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/545lvrbx.jpg)
    
- [ ]  By default **Administrator** account is selected, click Pa$$w0rd and press **Enter** to login.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/4hnagned.jpg)
    
- [ ]  Double click **Google chrome** on desktop and click http://localhost:8000 in the address bar and press **Enter**. Splunk sign in page appears, enter credentials (Username: **admin**, Password:**admin@123**) and click **Sign In**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/xhhy2ktj.jpg)
    
- [ ]  Click **SplunkForwarder** in the left pane.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/llee3e1q.jpg)
    
- [ ]  The Search console appears. Click host=WinServer2012 sourcetype=iis "%3CSCRIPT" OR “Javascript” OR "Alert" OR "3C%2Fscript" in the Search textbox to detect attempt of XSS attack on LuxuryTreats website on WinServer2012. Change the time to **5 minute window** and click **Search** icon.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/mczlu5yg.jpg)
    
- [ ]  Now to create an alert, click **Save As** and select **Alert** from the dropdown.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/gnkmintk.jpg)
    
- [ ]  Enter following details for the alert:
    - Title: **XSS Attack Alert**
    - Description: **XSS attempt is detected**
    - Permissions : **Private**
    - Select Alert type as **Real-time**
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/4wglaquu.jpg)
    
- [ ]  Tick the **Throttle** Check box, type **c_ip** in **Suppress results containing field value** textbox. Type **30** in **Suppress triggering for** textbox and Select **minute(s)** from right.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/bvngy4ue.jpg)
    
- [ ]  Click **Add Actions** button. Select **Add to Triggered Alerts** from the dropdown menu.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/evpbouou.jpg)
    
- [ ]  Select **Severity** as **Medium** and click **Save**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/bziiyomm.jpg)
    
- [ ]  **Alert has been saved** popup will appear. Close the pop-up window.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/tfzccauk.jpg)
    
- [ ]  Now, click [Kali Linux](https://labclient.labondemand.com/Instructions/ea95f72f-9a17-488e-a8f0-3f1b3e3ebbae#), then drag the blue screen upwards.
    
    > If you are already logged in as root, skip to step 15.
    > 
    
    Type **root** in the **Username** field and press **Enter**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/w1hlhjmt.jpg)
    
- [ ]  Type **toor** in the password field and press **Enter**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/1hiotp0u.jpg)
    
- [ ]  Click on the **Firefox** icon from the favourites bar to open the browser, type **http://www.luxurytreats.com** in the address field and press **Enter**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/s0k3qlcc.jpg)
    
- [ ]  Luxurytreats home page appears. Click on **Contact** menu.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/rn52toxl.jpg)
    
- [ ]  You will be redirected to contact page of luxurytreats. Type valid email address in **Email:** textbox and type the following script in the Comment textbox. Click **Save Comment**.
    
    `<Script>alert(“You are hacked”);</Script>`
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/10x1ar1r.jpg)
    
- [ ]  A pop-up appears on the page displaying a message **You are hacked**. Click **OK**. This demonstrates successful execution of **XSS attack**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/hbstao1x.jpg)
    
- [ ]  Close the browser.
- [ ]  Switch to SIEM1, click [SIEM1](https://labclient.labondemand.com/Instructions/ea95f72f-9a17-488e-a8f0-3f1b3e3ebbae#) and go to Splunk home page.
- [ ]  Navigate to **Activity** and select **Triggered Alerts** option from the dropdown.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/tp2cqvf1.jpg)
    
- [ ]  **XSS Attack alert** will be fired and can be seen in the **Triggered Alerts** window.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/0ntkerx3.jpg)
    
- [ ]  Click **View results** to view the alert detail.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/al0gtucf.jpg)
    
- [ ]  In this way, you can create use case and generate alerts for XSS attack attempts. After receiving alert, you should analyze and perform initial investigation to confirm and escalate the incident.
- [ ]  To disable **XSS Attack alert**. Click **Settings** and then click to **Searches, reports, and alerts**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/euezldfh.jpg)
    
- [ ]  In the **Searches, reports, and alerts** find XSS Attack alert and click the **Edit** dropdown menu. Select Disable to disable the alert.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/fv4pqhes.jpg)
    
- [ ]  **Alert disable** confirmation screen appears click **Disable** to disable the alert
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/xkr2oic0.jpg)
    
- [ ]  In XSS Attack alert, under **status** section it shows **Disabled**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/oopxh5cb.jpg)
    
- [ ]  Navigate to **Activity**, then **Triggered Alerts**, if there are any alerts then delete all the generated alerts.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/50bf05gs.jpg)
    
- [ ]  Click **Administrator -> logout** menu and close the browser.

**End of the Exercise**

In this exercise you have learnt how to create Splunk SIEM use case for detecting XSS attempts.

**Assessment 4.3.1**

Enter the encoded value of the special characters < and > in <SCRIPT> in the answer field below. These values are generally entered into Splunk to search for logs related to cross-site scripting. Enter the values separated by a comma.
