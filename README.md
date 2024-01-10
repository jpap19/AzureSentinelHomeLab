<h1>Azure Sentinel SIEM Home Lab</h1>

<h2>Description</h2>
In this Lab, we setup Azure Sentinel (SIEM) and connect it to a live virtual machine acting as a honey pot. We will observe live attacks (RDP Brute Force) from all around the world. We will use a custom PowerShell script to look up the attackers Geolocation information and plot it on the Azure Sentinel Map!
<br />

<h2>Resources And Links To Required Tools:</h2>

- <b>PowerShell Script for the Lab</b>   (https://github.com/joshmadakor1/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1)
- <b>Azure Trial</b>    (https://azure.microsoft.com/en-us/free/)
- <b>Sentinel Map Query</b> : FAILED_RDP_WITH_GEO_CL | summarize event_count=count() by sourcehost_CF, latitude_CF, longitude_CF, country_CF, label_CF, destinationhost_CF
| where destinationhost_CF != "samplehost"
| where sourcehost_CF != ""


<h2>Program walk-through:</h2>

<p align="center">
 
 The links provided here above can be used to download required softwares. The implementation will be in done in 4 Steps:

STEP 1: Install VMware SANDBOX, CREATE A VIRTUAL MACHINE FOR VULNERABILITY ASSESSMENT AND INSTALL WINDOWS 10 ON IT.

STEP 2: INSTALL and CONFIGURE NESSUS SCANNER

STEP 3: IDENTIFY VULNERABILITY ON WINDOWS HOST

STEP 4: PATCH AND REMEDIATE VULNERABILITIES


STEP 1: Install VMware SANDBOX, CREATE A VIRTUAL MACHINE FOR VULNERABILITY ASSESSMENT AND INSTALL WINDOWS 10 ON IT:
Go to the link provided here above, download and install VMware SANDBOX, then download windows 10 ISO and save in a folder on the computer. VMware  will be used to setup and run virtual machine, Windows 10 ISO  will be installed on the virtual machine.
 
1.1 VMware SANDBOX installed: <br/>

<img src="https://github.com/jpap19/NessusHomeLab/blob/main/Images/VmwareInstalled.png" height="100%" width="100%" alt="Azure Sentinel SIEM Home Lab"/>
<br />
<br />
1.2 VIRTUAL MACHINE CREATED FOR VULNERABILITY ASSESSMENT AND WINDOWS 10 INSTALLED ON IT: <br/>

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
