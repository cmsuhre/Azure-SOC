# Azure-SOC

# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud SOC](https://github.com/cmsuhre/Azure-SOC/assets/25305998/ebfc65b2-9f5c-4262-9ede-c12dc421ad87)


## Introduction

In this project, I constructed a compact honeynet within Azure, channeling log data from diverse sources into a Log Analytics workspace. This infrastructure is utilized by Microsoft Sentinel to generate attack maps, initiate alerts, and formulate incidents. I conducted a security assessment by monitoring metrics in the initially unsecured environment for 24 hours, implemented security enhancements to fortify the setup, and conducted another 24-hour metric evaluation. The outcomes are presented below. The metrics we will discuss include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Honeynet Architecture

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Machines (2 Windows, 1 Linux) </br>
![image](https://github.com/cmsuhre/Azure-SOC/assets/25305998/4ca49a26-a046-4e68-8555-9d24e0916e4a)
- Log Analytics Workspace </br>
![image](https://github.com/cmsuhre/Azure-SOC/assets/25305998/ed1ad557-21ea-4787-8ab7-c0a578d59748)
- Blobs Storage, Key Vault, and Activity Log </br>
![image](https://github.com/cmsuhre/Azure-SOC/assets/25305998/6b786fff-776b-4bbf-b371-190b93ca81fb)
- Microsoft Entra ID (formerly Microsoft Active Directory) </br>
![image](https://github.com/cmsuhre/Azure-SOC/assets/25305998/e4d55a2d-1f7b-41dd-8a54-342e5bde0940)
- Microsoft Sentinel </br>
![image](https://github.com/cmsuhre/Azure-SOC/assets/25305998/3d9e6bc9-f78d-45c6-a13e-c1edad41d81f)

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
