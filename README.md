
# 🔒Azure Cloud SOC Project: Honeynet and SIEM using Live Traffic 🔒
![Cloud SOC](https://github.com/cmsuhre/Azure-SOC/assets/25305998/3531ba98-4260-4367-ba5a-13047b40a479)



## Introduction

In this project, I built a compact honeynet within Azure, forwarding log data from various sources into a Log Analytics Workspace. This infrastructure is utilized by Microsoft Sentinel to generate attack maps, initiate alerts, and formulate incidents. I conducted a security assessment by monitoring metrics in the initially unsecured environment for 24 hours, implemented security enhancements to fortify the setup, and conducted another 24-hour metric evaluation. The outcomes are presented below. The metrics we will discuss include:

🔒 SecurityEvent (Windows Event Logs)<br>
🐧 Syslog (Linux Event Logs)<br>
🚨 SecurityAlert (Log Analytics Alerts Triggered)<br>
🛡️ SecurityIncident (Incidents created by Sentinel)<br>
🌐 AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)<br>

## Configured Honeynet Features

The honeynet in Azure consisted of the following features to track, simulate, and monitor real world cyber threats:

💻 Virtual Machines: I set up two Windows VMs and one Linux VM. One of the Windows VMs was designed to simulate attacks, testing our defenses and the honeynet setup across other VMs.<br>

📊 Log Analytics Workspace: All logs from the components were funneled here, allowing for the collection and analysis of data on malicious activities using Kusto Query Language (KQL).<br>

🔒 Blob Storage, Key Vault, and Activity Log: These components were targeted by a simulated internal threat to test their security measures.<br>

🆔 Microsoft Entra ID (formerly Microsoft Active Directory): This was another key component targeted by simulated threats within Azure.<br>

🚨 Microsoft Sentinel (SIEM): Configured to create custom alerts based on the malicious activity data collected from the Log Analytics Workspace, aiding with threat detection and incident response.<br>

🌍 Attack Maps, Incidents, & Alerts: I imported geolocation IP data into a custom watchlist to track the origins of attacks, enhancing our monitoring capabilities through the correlation features in Log Analytics Workspace.
📝 <b>NOTE</b>
<br> 
🔓 For the "BEFORE" metrics, all resources were initially set up with exposure to the internet. The Virtual Machines had their Network Security Groups and built-in firewalls completely open, and all other resources were deployed with public endpoints accessible from the Internet.

🔒 For the "AFTER" metrics, the Network Security Groups were tightened by blocking all traffic except for connections from my admin workstation, and all other resources were secured using their built-in firewalls.

## Attack Maps Before Hardening / Security Controls
![(before) NSG Allowed Malicious Inbound Flows](https://github.com/cmsuhre/Azure-SOC/assets/25305998/077b0ce6-7b08-4cd7-8541-2adb639b9e1e)<br>
![(before) Linux SSH Authentication Failures](https://github.com/cmsuhre/Azure-SOC/assets/25305998/68c034b5-be70-4947-b258-a57bd9fd34bd)<br>
![(before) Windows RDP-SMB Authentication Failures](https://github.com/cmsuhre/Azure-SOC/assets/25305998/a31b6713-1c31-4ce7-9725-5c9b74eb4198)<br>
![(before) MS SQL Server Authentication Failures](https://github.com/cmsuhre/Azure-SOC/assets/25305998/38136886-a20b-4ef8-a90d-b0201fc9cc0c)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:<br>
Start Time 2024-03-28 18:02:50 <br>
Stop Time 2024-03-29 18:02:50

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 47609
| Syslog                   | 3441
| SecurityAlert            | 4
| SecurityIncident         | 276
| AzureNetworkAnalytics_CL | 2300

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:<br>
Start Time 2024-03-30 15:44:43<br>
Stop Time	2024-03-31 15:44:43

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 12336
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

After applying the security controls, there was a significant drop in the number of security events and incidents, clearly showing how effective these measures were. It's also important to point out that if the network resources had been in heavy use by regular users, we might have seen even more security events and alerts during the 24 hours after implementing the controls.
