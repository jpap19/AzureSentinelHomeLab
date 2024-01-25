<h1>Azure Sentinel SIEM Home Lab</h1>

<h2>Description</h2>
In this Lab, we setup Azure Sentinel (SIEM) and connect it to a live virtual machine acting as a honey pot. We will observe live attacks (RDP Brute Force) from all around the world. We will use a custom PowerShell script to look up the attackers Geolocation information and plot it on the Azure Sentinel Map!
<br />

<h2>Resources And Links To Required Tools:</h2>

- <b>PowerShell Script for the Lab</b>   (https://github.com/joshmadakor1/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1)
- <b>Azure Trial</b>    (https://azure.microsoft.com/en-us/free/)

<h2>Program walk-through:</h2>

This is the high level overview of what we are going to setup in this Lab:

<img src="https://github.com/jpap19/AzureSentinelHomeLab/blob/main/ImageSentinelProj/Overview.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />
<br />
<p align="center">
 
 The links provided here above can be used to get required tools we need. The implementation will be in done in 4 Steps:

STEP 1: Create an Azure Subscription, Set up a honeypot Virtual Machine (VM).

STEP 2: Create a Log Analytics Workspace repository  in Azure to ingest logs from the Virtual Machine.

STEP 3: Set up Sentinel (Microsoft cloud Native SIEM) and use it to create a MAP to see Attackers geographical location.

STEP 4: Use a PowerShell Script and a Geolocation API to extract important data for maping attackers positions.
<br />

STEP 1: Create an Azure Subscription, Set up a honeypot Virtual Machine (VM):
Use the link above to create a subscription, then in portal.azure.com, create a Virtual Machine (VM), make it very vulnerable by disabling all firewall. 
This will characterize the vm as a honeypot (resource intentionally set to be very less secure, serve as a bait in order to trap attackers and detect them)
 
1.1 Azure Virtual Machine creation: <br/>

<img src="https://github.com/jpap19/AzureSentinelHomeLab/blob/main/ImageSentinelProj/vm_security%20group%20creation.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />
<br />

1.2 RDP connection to the Virtual Machine from our local computer: <br/>

Copy the public IP address of the VM, open start menu from our local computer and rdp into the VM using credentials set up during VM creation in azure portal.

<img src="https://github.com/jpap19/AzureSentinelHomeLab/blob/main/ImageSentinelProj/vm_rdp1.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />
<br />

1.3  Set up a Honeypot Virtual Machine and run PowerShell Script: <br/>
Disabling Firewall on Azure VM to make it to accept ping, thus vulnerable.

1.3.1 Firewall disable: <br/>
<img src="https://github.com/jpap19/AzureSentinelHomeLab/blob/main/ImageSentinelProj/vm_firewall%20disable.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />
<br />

1.3.2 Ping connectivity passing from local computer to the virtual machine
<img src="https://github.com/jpap19/AzureSentinelHomeLab/blob/main/ImageSentinelProj/vm_pinging%20success.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />
<br />

1.3.3 Windows Logs Security to visualize IPs remote connection to the virtual machine: <br/>
<img src="https://github.com/jpap19/AzureSentinelHomeLab/blob/main/ImageSentinelProj/vm_rdp%20log%20securityEventViwer.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />
<br />

1.4 PowerShell Script Run on the Virtual Machine: <br/>

Use the link provided here above to copy the PowerShell Script, open PowerShell ISE on the virtual machine, paste and save the script. 
Then sign in to the APi website, Get the API key and replace it in the Script. 
This Script is a continous while loop that basically look at events viewer, for the failed logon, get IPs addresses, sent those data to the API, get the country and geographical data (Latitude, Longitude)
and create a customs logs that will be used to train the logs analytics workspaces.

1.4.1 API Website sign in
<img src="https://github.com/jpap19/AzureSentinelHomeLab/blob/main/ImageSentinelProj/APIGeoLocationSignin.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />
<br />
1.4.2 API Key replaced in the PowerShell Script on the VM
<img src="https://github.com/jpap19/AzureSentinelHomeLab/blob/main/ImageSentinelProj/APIKeyReplacedScriptRun.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />
<br />

STEP 2: Create a Log Analytics Workspace repository in Azure to ingest log from the Virtual Machine.
In portal.azure.com, go to Log Analytics Workspaces and create a new Log Analytics Workspace that will be used to ingest logs from the virtual machine. 
customs logs with geographical data will be created to maps where the attacks originate.

2.1 Log Analytics Workspace created: <br/>

<img src="https://github.com/jpap19/AzureSentinelHomeLab/blob/main/ImageSentinelProj/LogAnalytic1.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />
<br />
2.2  Enable ability for Security Center to gather logs from the virtual machine into the Log Analytics Workspace : <br/>

From Azure portal, go to Microsoft Defender for cloud, Environment settings, Settings, defender plans; Then go back to work analytics workspace and connect it to the virtual machine

2.2.1 enabling Security Center ability to collect logs from Logs Analytics Workspace  into the virtual machine
<img src="https://github.com/jpap19/AzureSentinelHomeLab/blob/main/ImageSentinelProj/EnableSecurityCenter1.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />
<br />
2.2.2 Enabling all events Logs collection from Analytics Workspace  into the virtual machine
<img src="https://github.com/jpap19/AzureSentinelHomeLab/blob/main/ImageSentinelProj/SecurityCenterEnableWorkAnalyticLogs2.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />
<br />

2.3 Custom Logs creation in Work Analytics Workspaces: <br/>
Go to Work analytics workspace and click on our workspace (law-honeypot1), Tables > Create> New Custom Log (MMA-Based). Then upload the file with customs logs.
Open RDP on our local computer, and intentionally fail a login to the virtual machine, wait for about 10 minutes and run the work analyltic workspace and observed all customs fields created parsed with data.
2.3.1 Custom Logs created in Work Analytics Workspaces ( raw data can be observed): <br/>
<img src="https://github.com/jpap19/AzureSentinelHomeLab/blob/main/ImageSentinelProj/RawDataIngested.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />
<br />
STEP 3: Set up Sentinel (Microsoft cloud Native SIEM) and use it to create a MAP to see Attackers geographical location. This will be use to visualize the attacks from all over the world

Go to Azure Sentinel and click on create.

3.1 Azure Sentinel creation: <br/>

<img src="https://github.com/jpap19/NessusHomeLab/blob/main/Images/VmwareInstalled.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />
<br />
3.2 Azure Sentinel Workbook creation: <br/>

<img src="https://github.com/jpap19/NessusHomeLab/blob/main/Images/WindowsInstalledOnVM.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />
<br />

3.4 Geo visualization on a MAP of attackers (Failed login) from all over the world : <br/>

3.4.1 Azure Sentinel Workbook query results: <br/>
 Run the query below in 
- <b>Sentinel Map Query</b> : FAILED_RDP_WITH_GEO_CL 
| extend username = extract(@"username:([^,]+)", 1, RawData),
         timestamp = extract(@"timestamp:([^,]+)", 1, RawData),
         latitude = extract(@"latitude:([^,]+)", 1, RawData),
         longitude = extract(@"longitude:([^,]+)", 1, RawData),
         sourcehost = extract(@"sourcehost:([^,]+)", 1, RawData),
         state = extract(@"state:([^,]+)", 1, RawData),
         label = extract(@"label:([^,]+)", 1, RawData),
         destination = extract(@"destinationhost:([^,]+)", 1, RawData),
         country = extract(@"country:([^,]+)", 1, RawData)
| where destination != "samplehost"
| where sourcehost != ""
| summarize event_count=count() by latitude, longitude, sourcehost, label, destination, country

 Customs fields longitude. latitude, sourcehost, label, destination, contry, and event_count can be observed
 
<img src="https://github.com/jpap19/AzureSentinelHomeLab/blob/main/ImageSentinelProj/WorkBookQuery2.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />

<br />
3.4.1 Live Attacks can be visualized on World Map: <br/>
<img src="https://github.com/jpap19/NessusHomeLab/blob/main/Images/WindowsInstalledOnVM.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />
<br />


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
