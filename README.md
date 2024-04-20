
# 🔒Azure Cloud SOC Project: Honeynet and SIEM 🔒
![Cloud SOC](https://github.com/cmsuhre/Azure-SOC/assets/25305998/3531ba98-4260-4367-ba5a-13047b40a479)



## Introduction

In this project, I created a small honeynet in Azure. I sent log data from different sources to a Log Analytics Workspace. Microsoft Sentinel uses this infrastructure for attack maps, alerts, and incidents. 

I started by monitoring metrics in an unsecured environment for 24 hours. I then implemented security enhancements to strengthen the setup. Finally, I conducted another 24-hour metric evaluation. The report presents the outcomes below.

📄 <strong><u>Collected Logs included: </u></strong>

- SecurityEvent: Windows Event Logs<br>
- Syslog: Linux Event Logs<br>
- SecurityAlert: Log Analytics Alerts Triggered<br>
- SecurityIncident: Incidents created by Sentinel<br>
- AzureNetworkAnalytics_CL: Malicious Flows allowed into our honeynet<br>

## Configured Honeynet Features

The honeynet in Azure had features to track and simulate real world cyber threats: 

- 💻 <b>Virtual Machines:</b> I set up two Windows VMs and one Linux VM. I designed one of the Windows VMs to simulate attacks. It tested our defenses and the honeynet setup across other VMs

- 📊 <b>Log Analytics Workspace:</b> I funneled all logs from the components here. This simplified collecting and analyzing data on malicious activities with KQL.

- 🔒 <b>Blob Storage, Key Vault, and Activity Log:</b> A simulated threat tested their security.

- 🆔 <b>Microsoft Entra ID, once Microsoft Active Directory:</b> faced simulated threats in Azure.

- 🚨 <b>Microsoft Sentinel (SIEM):</b> Turns malicious activity data into custom alerts. This helps with threat detection and incident response.

- 🌍 <b>Attack Maps, Incidents, & Alerts:</b> I added geolocation IP data to a custom watchlist. This tracked attack origins and improved monitoring in the Log Analytics Workspace.

<br><strong><u> 🔄 BEFORE & AFTER🔄 </u></strong><br>

- 🔓 For the "BEFORE" metrics, I exposed all resources to the internet. The Virtual Machines kept their Network Security Groups and built-in firewalls completely open. I deployed all other resources with public endpoints accessible from the Internet.

- 🔒 For the "AFTER" metrics, I blocked all traffic except from my admin workstation. I also secured all other resources with their built-in firewalls.

## Attack Maps Before Hardening / Security Controls
![(before) NSG Allowed Malicious Inbound Flows](https://github.com/cmsuhre/Azure-SOC/assets/25305998/077b0ce6-7b08-4cd7-8541-2adb639b9e1e)<br>
![(before) Linux SSH Authentication Failures](https://github.com/cmsuhre/Azure-SOC/assets/25305998/68c034b5-be70-4947-b258-a57bd9fd34bd)<br>
![(before) Windows RDP-SMB Authentication Failures](https://github.com/cmsuhre/Azure-SOC/assets/25305998/a31b6713-1c31-4ce7-9725-5c9b74eb4198)<br>
![(before) MS SQL Server Authentication Failures](https://github.com/cmsuhre/Azure-SOC/assets/25305998/38136886-a20b-4ef8-a90d-b0201fc9cc0c)<br>

## Metrics Before Hardening / Security Controls

The table below lists the metrics from our insecure environment over 24 hours:<br>
- Start Time: 2024-03-28 18:02:50
- Stop Time: 2024-03-29 18:02:50

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 47609
| Syslog                   | 3441
| SecurityAlert            | 4
| SecurityIncident         | 276
| AzureNetworkAnalytics_CL | 2300

## Attack Maps Before Hardening / Security Controls

```After hardening, no map queries showed malicious activity for 24 hours.```

## Metrics After Hardening / Security Controls

Below is a table of our metrics 24 hours after adding security controls:
- Start Time: 2024-03-30 15:44:43
- Stop Time: 2024-03-31 15:44:43

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 12336
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

After adding security controls, security events and incidents dropped, showing their effectiveness. More people on the network could mean more events and alerts after adding controls.
