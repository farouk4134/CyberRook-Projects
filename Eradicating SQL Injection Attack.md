# 

Eradication involves removing or eliminating the root cause of the incident and closing all the attack vectors to prevent similar incidents in future.

**Lab Scenario**

If the target website is undergoing SQL and XSS attacks, the ideal eradication strategy would be to identify the root cause of such attacks. These attacks are possible when attacker poises normal web requests with malicious parameters and sends it to the webservers. Web filters, if configured properly, can help you eradicate such SQL and XSS injection vulnerabilities as they filter requests based on allowed or denied characters in the web requests

**Lab Objectives**

The objective of this lab is to explain how the incidents are eradicated in eradication phase of incident response process. In this lab, we are eradicating SQL Injection and XSS incidents.

**Lab Tasks**

- [ ]  Click [WinServer2012](https://labclient.labondemand.com/Instructions/c2ad4095-bf2f-4dfc-a704-a58e4a3b3852#) machine
    
    > If WinServer2012 Machine is already launched skip to step 3.
    > 
    
    Click [Ctrl+Alt+Delete](https://labclient.labondemand.com/Instructions/c2ad4095-bf2f-4dfc-a704-a58e4a3b3852#) link.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/f2tkoy3v.jpg)
    
- [ ]  By default **Administrator** account is selected, click Pa$$w0rd and press **Enter** to login.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/0kfoakgz.jpg)
    
- [ ]  Navigate to **Z:\SOC-Tools\Module 6 Incident Response\UrlScan Tool** folder and double-click **urlscan_v31_x64.msi**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/vfhjubzw.jpg)
    
- [ ]  If the **Open File -Security Warning** pop-up appears, click **Run**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/zixwzooz.jpg)
    
- [ ]  The **Microsoft UrlScan Filter v3.1** Setup wizard appears, click **I accept the terms in the License Agreement** checkbox and click **Install**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/qn0a1jco.jpg)
    
- [ ]  On completing the installation, click **Finish**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/lth0qlzc.jpg)
    
- [ ]  UrlScan 3.1 installer installs **UrlScan.dll** and **UrlScan.ini** files in **C:\Windows\system32\inetsrv\urlscan** directory. The **UrlScan.ini** contains the recommended settings to prevent known attacks against IIS servers.
- [ ]  Navigate to **C:\Windows\system32\inetsrv\** folder, copy **urlscan** folder.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/1cfeh1kj.jpg)
    
- [ ]  Paste it in **C:\inetpub\wwwroot\LuxuryTreats** folder.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/5sgxoyb2.jpg)
    
- [ ]  To add a custom rule to prevent **SQL Injection attack**, Navigate to **c:\inetpub\wwwroot\LuxuryTreats\urlscan\UrlScan.ini** right-click the **UrlScan.ini** and click **Edit with Notepad++** . Add the following rule in the **UrlScan.ini** file. (After **Line No.175**)

```
TypeCopy
[SQL Injection]
AppliesTo=.asp,.aspx
DenyDataSection=SQL Injection Strings
ScanUrl=0
ScanAllRaw=0
ScanQueryString=1
ScanHeaders=0

[SQL Injection Strings]
--
;%3b ; a semicolon
/*
@ ; also catches @@
char ; also catches nchar and varchar
alter
begin
cast
convert
create
cursor
declare
delete
drop
end
exec ; also catches execute
fetch
insert
kill
open
select
sys ; also catches sysobjects and syscolumns
table
update
```

![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/0mqw01wx.jpg)

- [ ]  In line **175**, find **RuleList=** and include the created SQL Injection rule by replacing it with **RuleList="SQL Injection"**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/4fz42r1x.jpg)
    
- [ ]  Click **Save** icon and **close** the file.
- [ ]  Now, click **Start** menu button, click Windows **Administrative Tools**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/htbz33po.jpg)
    
- [ ]  **Administrative Tools** window will appear, double-click on **Internet Information Services (IIS)** Manager.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/44evydnf.jpg)
    
- [ ]  **Internet Information Services (IIS) Manager** main window appears; now in **Connections** pane, expand **Sites** and select **Luxurytreats** website. In the **Luxurytreats Home** window, double-click **ISAPI Filters** under **IIS**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/tnnilsnw.jpg)
    
- [ ]  In the **ISAP Filters** window, select **Urlscan 3.1** and click **Edit** from the **Actions** pane.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/ekcmgmby.jpg)
    
- [ ]  In the **Edit ISAPI Filter**, click the **ellipse** button.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/5xznhluy.jpg)
    
- [ ]  **Open** dialog box appears. Navigate to **C:\inetpub\wwwroot\LuxuryTreats\urlscan** folder and select **urlscan.dll** file from the **Open** dialog box. Click **OK**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/wtfgdjs5.jpg)
    
- [ ]  Select **IIS Server (WINSERVER2012)** from the **Connections** pane and click **Restart** from the **Actions** pane.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/wc2e1sh5.jpg)
    
- [ ]  Now, double click **Google Chrome** to open the browser, place the mouse cursor in the address bar and click the link http://www.luxurytreats.com and press **Enter**. Luxurytreats home page appears.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/a5oo4gb1.jpg)
    
- [ ]  Assuming you are a registered member in luxurytreats website login using following credential:
    
    **USERNAME**: bob **PASSWORD**: Passw0rd
    
    > If save password pop-up appears, close it.
    > 
- [ ]  You will see **Welcome bob** in **Home** Page.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/bzpfbd0n.jpg)
    
- [ ]  Hover mouse on left corner of the website click on **My Orders**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/fvekelbn.jpg)
    
- [ ]  List of orders will be displayed, click on Order Id **ORD-001**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/wsv3qbni.jpg)
    
- [ ]  Order details of the selected order will be appeared.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/dldfy5ra.jpg)
    
- [ ]  Close all open windows.
- [ ]  As per standard security practice, only authorized person should see their personal data. If person can see other’s order details, then it can be considered as security breach. This can be possible using SQL Injection technique. Attacker use this technique to bypass security measures of other user’s data.
- [ ]  Now, click [Kali Linux](https://labclient.labondemand.com/Instructions/c2ad4095-bf2f-4dfc-a704-a58e4a3b3852#), then drag the blue screen upwards. Type **root** in the **Username** field and press **Enter**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/0rcetccs.jpg)
    
- [ ]  Type **toor** in the password field and press **Enter**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/tk1al02f.jpg)
    
- [ ]  Before starting this lab, assume that you are registered user of **www.luxurytreats.com** website. Right click on the **Firefox** icon from the favourites bar to open the browser.
- [ ]  Now, repeat the **steps 20-25** from this lab in **Firefox** browser of kali machine.
- [ ]  Now to demonstrate **SQL Injection**, alter the URL in previous step as follows. **http://www.luxurytreats.com/OrderDetail.aspx?Id=ORD-001 ' or 1=1;--** and press **Enter**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/ppnp53fs.jpg)
    
- [ ]  This time, you will not be able to perform SQL Injection and view all the order details; instead, you will see HTTP Error 404.0 as shown in the following screenshot. Urlscan tool will filter the request based on the query string. It will block contents containing SQL strings defined in **SQL Injection Rule** in the **UrlScan.ini** before it is processed by a web application.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/10f5ko0c.jpg)
    
- [ ]  Now, close the browser and relaunch by clicking on the **Firefox** icon from the favourites bar to open the browser, type **http://www.luxurytreats.com** in the address field and press **Enter**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/40lmmylj.jpg)
    
- [ ]  Luxurytreats home page appears. Click on **Contact** menu.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/l1kb240i.jpg)
    
- [ ]  You will be redirected to contact page of luxurytreats. Type valid email address in **Email:** textbox and type the following script in the Comment textbox. Click **Save Comment**.
    
    `<Script> alert(“You are hacked”);</Script>`
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/pt4ril1p.jpg)
    
- [ ]  This time the **XSS attack** will not be successful, as **UrlScan** will filter the request and identify it to be vulnerable and return an HTTP 404 as shown in the screenshot.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/k0nfjxjg.jpg)
    
- [ ]  Switch to WinServer2012 virtual machine, click [WinServer2012](https://labclient.labondemand.com/Instructions/c2ad4095-bf2f-4dfc-a704-a58e4a3b3852#). Now, click **Start** menu button, click **Windows Administrative Tools**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/zo1ynhg2.jpg)
    
- [ ]  **Administrative Tools** window will appear, double-click on **Internet Information Services (IIS) Manager**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/0zovdddg.jpg)
    
- [ ]  **Internet Information Services (IIS) Manager** main window appears; now in **Connections** pane, expand **Sites** and select **Luxurytreats** website. In the **Luxurytreats Home** window, double-click **ISAPI Filters** under **IIS**.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/dz3w2vv5.jpg)
    
- [ ]  To remove **UrlScan** filter from **luxurytreats** site, select **UrlScan 3.1** from the **Actions** pane of the **ISAPI Filter** window, and click **Remove**, and then click **Yes** in the pop-up.
    
    ![](https://labondemand.blob.core.windows.net/content/lab161184/Screens/ybxoiz30.jpg)
    
- [ ]  Close all open windows.

**End of the Exercise**

In this exercise you have learnt how the incidents are eradicated in eradication phase of in incident response process. Eradicating SQL Injection and XSS incidents.

**Assessment 6.3.1:**

According to the URIScan.ini file, .asp webpages and another type of webpages are protected from SQL injection and cross-site scripting. What is the other type?
