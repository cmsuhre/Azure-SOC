# Azure-SOC

# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud SOC](https://github.com/cmsuhre/Azure-SOC/assets/25305998/3531ba98-4260-4367-ba5a-13047b40a479)



## Introduction

In this project, I built a compact honeynet within Azure, forwarding log data from various sources into a Log Analytics workspace. This infrastructure is utilized by Microsoft Sentinel to generate attack maps, initiate alerts, and formulate incidents. I conducted a security assessment by monitoring metrics in the initially unsecured environment for 24 hours, implemented security enhancements to fortify the setup, and conducted another 24-hour metric evaluation. The outcomes are presented below. The metrics we will discuss include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Honeynet Components

The honeynet in Azure consisted of the following components:

- Virtual Machines (2 Windows, 1 Linux) <br>
  - A Windows VM was set up to mimic attacks, testing defenses and honeynet setups across other VMs <br>
- Log Analytics Workspace <br>
  - Logs from every component were routed to the Log Analytics Workspace, enabling the collection and analysis of malicious activity data through Kusto Query Language (KQL) <br>
- Blobs Storage, Key Vault, and Activity Log <br>
  - These components were designed for targeting by a simulated internal threat <br>
- Microsoft Entra ID (formerly Microsoft Active Directory) <br>
  - Another component within Azure that was created for targeting by a simulated internal threat <br>
- Microsoft Sentinel (SIEM) </br>
  - The Azure SIEM tool configured with custom alerts based on the malicious activity data collected and analyzed within the Log Analytics Workspace using KQL<br>
- Attack Maps, Incidents, & Alerts </br>
  - Imported geolocation IP data into a custom watchlist to pinpoint attack origins using Log Analytics Workspace correlation <br>

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls.

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

Upon review, the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness. In addition, it is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
