
**nderstanding Logs, Event, Incident.**

IIS logs can act as event data source to detect SQL injection attempts.

**Lab Scenario**

The presence of specific patterns or signatures in the IIS logs denotes the SQL injection attempts. You can identify such patterns and create alerts to detect SQL injection attempt. As a SOC analyst, you should be able to identify the monitoring need, event source, and minimum configuration needed to detect these types of attempts.

**Lab Objectives**

The objective of this lab is to create Splunk SIEM use case for detecting SQL injection attempts.

**Lab Tasks**

- [ ]  Click [WinServer2012](https://labclient.labondemand.com/Instructions/9545dbbf-3281-4219-9029-6e3464921f7b#) machine.
    
    > If WinServer2012 is already launched, skip to step 3
    >
**Understanding Logs, Event, Incident.**

IIS logs can act as event data source to detect SQL injection attempts.

**Lab Scenario**

The presence of specific patterns or signatures in the IIS logs denotes the SQL injection attempts. You can identify such patterns and create alerts to detect SQL injection attempt. As a SOC analyst, you should be able to identify the monitoring need, event source, and minimum configuration needed to detect these types of attempts.

**Lab Objectives**

The objective of this lab is to create Splunk SIEM use case for detecting SQL injection attempts.

**Lab Tasks**

- [ ]  Click [WinServer2012](https://labclient.labondemand.com/Instructions/9545dbbf-3281-4219-9029-6e3464921f7b#) machine.
    
    > If WinServer2012 is already launched, skip to step 3
    > 
    
    Click [Ctrl+Alt+Delete](https://labclient.labondemand.com/Instructions/9545dbbf-3281-4219-9029-6e3464921f7b#) link.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/0evlpmpl.jpg)
    
- [ ]  By default **Administrator** account is selected, click Pa$$w0rd and press **Enter** to login.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/jym20uyi.jpg)
    
- [ ]  Click [SIEM1](https://labclient.labondemand.com/Instructions/9545dbbf-3281-4219-9029-6e3464921f7b#) machine
    
    > If SIEM1 is already launched, skip to step 5
    > 
    
    Click [Ctrl+Alt+Delete](https://labclient.labondemand.com/Instructions/9545dbbf-3281-4219-9029-6e3464921f7b#) link.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/545lvrbx.jpg)
    
- [ ]  By default **Administrator** account is selected, click Pa$$w0rd and press **Enter** to login.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/4hnagned.jpg)
    
- [ ]  Double click **Google chrome** on desktop and click http://localhost:8000 in the address bar and press **Enter**. Splunk sign in page appears, enter credentials (Username: **admin**, Password:**admin@123**) and click **Sign In**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/xhhy2ktj.jpg)
    
- [ ]  Click **SplunkForwarder** in the left pane.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/llee3e1q.jpg)
    
- [ ]  The Search console appears. Click **host=WinServer2012 sourcetype=iis | eval cs_uri_query = urldecode(cs_uri_query) | regex cs_uri_query ="/(\%27)|(\')|(--)|(\%23)|(#)/ix" | iplocation c_ip | table _time cs_uri_query cs_User_Agent c_ip** in the Search textbox to detect attempt of SQL injection on LuxuryTreats website on WinServer2012. Change the time to **5 minute window** from the drop down menu and click **Search** icon.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/0mzvyaca.jpg)
    
- [ ]  Now to create an alert, click **Save As** and select **Alert** from the dropdown.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/1vyz5z01.jpg)
    
- [ ]  Enter following details for the alert:
    - Title: “**SQL Injection Alert**”
    - Description: “**SQL injection attempt is detected**”
    - Permissions: **Private**
    - Select Alert type as **Real-time**
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/yd5azlvd.jpg)
    
- [ ]  Tick the **Throttle** Check box, type **c_ip** in **Suppress results containing field value** textbox. Type **30** in **Suppress triggering for** textbox and Select **minute(s)** from right.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/sqbmvcwv.jpg)
    
- [ ]  Click **Add Actions** button. Select **Add to Triggered Alerts** from the dropdown menu.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/banea5eu.jpg)
    
- [ ]  Select **Severity** as **High** and click **Save**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/zi5xwydb.jpg)
    
- [ ]  **Alert has been saved** popup will appear. Close the pop-up window.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/prafsvij.jpg)
    
- [ ]  Now, click [Kali Linux](https://labclient.labondemand.com/Instructions/9545dbbf-3281-4219-9029-6e3464921f7b#), then drag the blue screen upwards.
    
    > [!note] If you are already logged in as root, skip to step 15.
    > 
    
    Type **root** in the **Username** field and press **Enter**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/n2jvbw2k.jpg)
    
- [ ]  Type **toor** in the password field and press **Enter**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/t014g0yz.jpg)
    
- [ ]  Open FireFox Browser. Enter the link **http://www.luxurytreats.com** and press **Enter**. **Luxurytreats** home page appears.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/ssn4p1up.jpg)
    
- [ ]  Assuming you are a registered member in luxurytreats website login using following credential:
    
    **USERNAME**: bob **PASSWORD**: Passw0rd
    
    > If save password pop-up appears, close it.
    > 
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/f1ezkrtw.jpg)
    
- [ ]  You will see **Welcome bob** in **Home** Page.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/onjjpfga.jpg)
    
- [ ]  Hover mouse on left corner of the website click on **My Orders**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/zfpkvzeu.jpg)
    
- [ ]  List of orders will be displayed, click on Order Id **ORD-001**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/hbodkqqb.jpg)
    
- [ ]  Order details of the selected order will be appeared.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/e0gv044c.jpg)
    
- [ ]  Now to demonstrate **SQL Injection**, alter the URL in previous step as follows. **http://www.luxurytreats.com/OrderDetail.aspx?Id=ORD-001 ' or 1=1;--** and press **Enter**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/msbxacjs.jpg)
    
- [ ]  This trick will fetch the order details of the other users.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/1ceukukd.jpg)
    
- [ ]  This is possible because website is vulnerable to SQL injection attacks. When attacker passes such type of SQL injection specific query, it will bypass the security mechanism (authentication) imposed by the application and reveals sensitive data.
- [ ]  Click **Logout** from top right corner of the page to logout from luxurytreats website. Close the browser
- [ ]  Switch to SIEM1, click [SIEM1](https://labclient.labondemand.com/Instructions/9545dbbf-3281-4219-9029-6e3464921f7b#) and go to Splunk home page.
- [ ]  Navigate to **Activity** and select **Triggered Alerts** option from the dropdown.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/3bydzoov.jpg)
    
- [ ]  **SQL Injection alert** will be fired and can be seen in the **Triggered Alerts** window.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/nabcqn35.jpg)
    
- [ ]  Click **View results** to view the alert detail.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/vum30w0n.jpg)
    
- [ ]  In this way, you can create use case and generate alerts for SQL injection attempts. After receiving alert, you should analyze and perform initial investigation to confirm and escalate the incident.
- [ ]  To disable **SQL Injection Alert**. Click **Settings** and then click to **Searches, reports, and alerts**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/adpg4wqe.jpg)
    
- [ ]  In the **Searches, reports, and alerts** find SQL Injection alert and click the **Edit** dropdown menu. Select Disable to disable the alert.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/5ousrn1p.jpg)
    
- [ ]  **Alert disable** confirmation screen appears click **Disable** to disable the alert
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/qoaqpfce.jpg)
    
- [ ]  In SQL Injection Alert, under **status** section it shows **Disabled**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161185/Screens/24qcxepe.jpg)
    
- [ ]  Navigate to **Activity**, then **Triggered Alerts**, if there are any alerts then delete all the generated alerts.
- [ ]  Click **Administrator -> logout** menu and close the browser.

**End of the Exercise**

In this exercise you have learnt how to create Splunk SIEM use case for detecting SQL injection attempts.

**Assessment 4.2.1**

What is the encoded value of a single quote that is entered into Splunk to search for SQL injection entries in the logs (e.g., %2A for *)?

**Answer:**
