<h1>Azure VM Honeypot | Microsoft Sentinel 'SIEM'</h1>

<h2>Description</h2>
This project involves configuring a honeypot on a Microsoft Azure virtual machine to generate failed Remote Desktop Protocol 'RDP' login event logs with the help of a running PowerSHell script. Utilizing Microsoft Sentinel, the failed login attempts are visualized and mapped geogprahically. 
<br />

<h2>Technologies & Utilities Used</h2>

- <b>IP Gealocation API</b>
- <b>Microsoft Azure Log Analytics workspaces</b>
- <b>Microsoft Defender for Cloud</b>
- <b>Microsoft Sentinel</b>
- <b>Windows Event Viewer</b>
- <b>Windows PowerShell ISE</b>

<h2>Operating Systems Used</h2>

- <b>Windows 10 Professional</b> (21H2)

<h2>Prerequisites</h2>

- <b>Microsoft Azure Free Trial Account</b>
- <b>Windows 10 Professional Microsoft Azure VM 'Default Configuration'</b>

<h3 align="center">Create Microsoft Azure Windows 10 Professional Honeypot VM</h3>
<p align="center">
Azure Windows 10 Professional VM gets created with  the name 'HoneyPot'. <br/>
<img src="https://i.imgur.com/fRxerOk.png" height="80%" width="80%" alt="Create VM"/>
<br />
Choose 'Advanced' under 'NIC network security group' to create a custom inbound security rule. <br/>
<img src="https://i.imgur.com/7a3uB7A.png" height="80%" width="80%" alt="Create VM NIC Security Group Advanced"/>
<br />
Configure an inbound security rule to allow connections to and from any IP address via all ports and any protocol. This is what turns the VM into a honeypot, as it will attract adversaries on the internet to attempt to gain access. <br/>
<img src="https://i.imgur.com/jz7k0kr.png" height="100%" width="100%" alt="Create VM Newtwork Security Group"/>
<br />
</p>

<h3 align="center">Create & Configure Log Analytics workspace</h3>
<p align="center">
Naming and creating the Log Analytics workspace instance. The Log Analytics workspace will be utilized to ingest the failed RDP logins log data from the honeypot VM.<br/>
<img src="https://i.imgur.com/eTTEBP5.png" height="80%" width="80%" alt="Create Log Analytics workspace"/>
<br />
Enable Micorsoft Defender for Cloud with the the Log Analytics workspace in order to gather logs from the honeypot VM. <br/>
<img src="https://i.imgur.com/7s5dRFM.png" height="80%" width="80%" alt="Microsoft Defender for Cloud Enable Plan"/>
<br />
Turn on 'All Events' data collection for Windows security events generated on the honeypot VM. <br/>
<img src="https://i.imgur.com/pnjp5FU.png" height="80%" width="80%" alt="Microsoft Denfender for Cloud Enable Data Collection"/>
<br />
Connecting the honeyot VM to Log Analytics workspace. <br/>
<img src="https://i.imgur.com/88EPqL7.png" height="80%" width="80%" alt="Connect VM to Log Analytics workspace"/>
<br />
Lastly, Microsoft Sentinel is connected to the Log Analytics workspace to visualize the RDP failed login data collected within it. <br/>
<img src="https://i.imgur.com/oJt930C.png" height="100%" width="100%" alt="Add Microsoft Sentinel to Log Analytics workspace"/>
<br />
</p>

<h3 align="center">RDP to Honeypot & Create Failed Login Attempts Events</h3>
<p align="center">
Acessing honeypot VM via its public IP address '172.210.40.214' with RDP. <br/>
<img src="https://i.imgur.com/S56OAQK.png" height="80%" width="80%" alt="RDP to VM"/>
<br />
<img src="https://i.imgur.com/FqhkXTN.png" height="80%" width="80%" alt="RDP to VM Login"/>
<br />
The Windows Event Viewer shows security events, including one of the purposely failed RDP login attempts highligthed in the image below. The PowerShell script will retrieve these 'Event ID: 4625' security logs from the Windows Event Viewer to gather information for the failed RDP login logs that it will generate. <br/>
<img src="https://i.imgur.com/MStq5tk.png" height="100%" width="100%" alt="Event Viewer Failed RDP Login Test"/>
<br />
</p>

<h3 align="center">PowerShell Script</h3>
<p align="center">
The PowerShell script is now loaded into the Windows PowerShell ISE. Its primary function is to generate a log containing details of each failed RDP login attempt, including the username, source IP address, and timestamp. Additionally, it utilizes an IP Geolocation API to enrich the log with information such as latitude, longitude, state, and country corresponding to each IP address. <br/>
<img src="https://i.imgur.com/dnKbXkU.png" height="100%" width="100%" alt="Load Event Viewer PowerShell Script"/>
<br />
Output of the PowerShell script after being started. It shows the purposely failed login RDP attempts from earlier and the assoacted information.
<img src="https://i.imgur.com/MM3wumO.png" height="100%" width="100%" alt="Event Viewer PowerShell Script Output"/>
<br />
Inspecting the 'failed.rdp' log generated by the PowerShell script, found in the ProgramData folder of the honeypot VM. The script includes extra sample data, resulting in more information than just the three intentionally failed RDP login attempts. <br/>
<img src="https://i.imgur.com/a0te0DB.png" height="100%" width="100%" alt="Event Viewer PowerShell Script RDP Log"/>
<br />
</p>

<h3 align="center">Upload Failed RDP Login Log Data to Log Analytics workspace</h3>
<p align="center">
To enable querying of the raw data in the 'failed.rdp' log, it needs to be uploaded from the local machine (not the VM) into the Log Analytics workspace. <br/>
<img src="https://i.imgur.com/UOm5bP5.png" height="80%" width="80%" alt="Create Custom Log Query in Log Analytics workspace"/>
<br />
<img src="https://i.imgur.com/XG0KJ8r.png" height="80%" width="80%" alt="Create Custom Log Query in Log Analytics workspace Name"/>
<br />
<img src="https://i.imgur.com/npu8uX7.png" height="80%" width="80%" alt="Create Custom Log Query in Log Analytics workspace Details"/>
<br />
</p>

<h3 align="center">Transform Raw Failed RDP Login Log Data in to Custom Fields</h3>
<p align="center">
The failed.rdp' log showing the inegested data in the Log Analytics workspace.
<img src="https://i.imgur.com/jGLHYrh.png" height="80%" width="80%" alt="Testing Custom Log Query in Log Analytics workspace"/>
<br />
Query transforming the raw data into custom fields which will be visualized in the Microsoft Sentinel SIEM. <br/>
<img src="https://i.imgur.com/yo33gbA.png" height="80%" width="80%" alt="Transform Raw Query Data to Custom Fields in Log Analytics workspace"/>
<br />
</p>

<h3 align="center">Upload Failed RDP Login Log Data to Log Analytics workspace</h3>
<p align="center">
To enable querying of the raw data in the 'failed.rdp' log, it needs to be uploaded from the local machine (not the VM) into the Log Analytics workspace. <br/>
<img src="https://i.imgur.com/UOm5bP5.png" height="80%" width="80%" alt="Create Custom Log Query in Log Analytics workspace"/>
<br />
<img src="https://i.imgur.com/XG0KJ8r.png" height="80%" width="80%" alt="Create Custom Log Query in Log Analytics workspace Name"/>
<br />
<img src="https://i.imgur.com/npu8uX7.png" height="80%" width="80%" alt="Create Workbook in Microsoft Sentine to make Failed RDP Login Map"/>
<br />
</p>

<h3 align="center">Configure Microsoft Sentinel Workspace & Workbook</h3>
<p align="center">
The Log Analytics workspace gets added into Microsoft Sentinel first. <br/>
<img src="https://i.imgur.com/vYn6u9K.png" height="80%" width="80%" alt="Add Microsoft Sentinel to Log Analytics workpsace"/>
<br />
The Log Analytics workspace failed.rdp log query is added to a Microsoft Sentinel workbook. The custom field 'state' from the query will be used to count the failed RDP login attempts from adversaries, and the 'Map' workbook visualization option is selected to create a global geographical representation of the source IP address locations. <br/>
<img src="https://i.imgur.com/dSOpTgL.png" height="100%" width="100%" alt="Create Workbook in Microsoft Sentine to make Failed RDP Login Map"/>
<br />
</p>

<h3 align="center">Observe & Results</h3>
<p align="center">
Turn off the Windows Firewall on the Windows 10 Professional VM to make the honeypot more enticing for adversaries utilizing port scans on the internet. <br/>
<img src="https://i.imgur.com/6MDQ5iw.png" height="80%" width="80%" alt="Turn off Windows Firewall On VM"/>
<br />
With the PowerShell script running on the honeypot for a few hours, generating failed RDP login logs, there has been a significant increase in the number of failed RDP login attempts from various locations across the globe. Specifically, there are over 8,000 attempts from Seoul, South Korea, followed by 145 attempts from the state of Alabama. <br/>
<img src="https://i.imgur.com/m5ZFCH9.png" height="100%" width="100%" alt="Failed RDP Login Attempts on Map"/>
<br />
When evaluating the script output in Windows PowerShell ISE and the failed.rdp logs, it appears that the username 'ADMINISTRATOR' is commonly used by adversaries from South Korea in their brute-force attempts. <br/>
<img src="https://i.imgur.com/LmUhyjI.png" height="100%" width="100%" alt="Failed RDP Login Attempts Powershell Script"/>
 <img src="https://i.imgur.com/jgGbZDl.png" height="100%" width="100%" alt="Failed RDP Login Attempts Logs"/>
<br />
</p>
