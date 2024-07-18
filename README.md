# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/user-attachments/assets/a4406ef3-fce2-4f75-b0d0-56ac344b730d)

## Introduction

In this project, I created a mini honeynet on Azure and integrated log data from multiple sources into a Log Analytics workspace. This data was then utilized by Microsoft Sentinel to construct attack maps, generate alerts, and create incidents. I recorded security metrics in the unsecured environment over a 24-hour period, implemented security controls to fortify the environment, measured the metrics again for another 24 hours, and collected the data. The metrics I will present are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources were deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/user-attachments/assets/01efc1f4-1e11-40cf-bb3f-1488cfa391f0)
<br>
![Linux Syslog Auth Failures](https://github.com/user-attachments/assets/d23e2e4f-f27b-46a7-9edb-d644d61c4a8d)<br>
![Windows RDP/SMB Auth Failures](https://github.com/user-attachments/assets/a606e7da-9ffd-415a-aa92-ae908c73c9a1)
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in my insecure environment for 24 hours:
Start Time 2024-06-18 17:04:29
Stop Time 2024-06-19 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 20111
| Syslog                   | 3668
| SecurityAlert            | 10
| SecurityIncident         | 427
| AzureNetworkAnalytics_CL | 843

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics I measured in the environment for another 24 hours, but after applied security controls:

Start Time 2024-06-18 17:04:29
Stop Time 2024-06-19 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 7627
| Syslog                   | 22
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was established on Microsoft Azure, with log sources integrated into a Log Analytics workspace. Microsoft Sentinel was used to generate alerts and create incidents based on the ingested logs. Security metrics were measured in the unsecured environment before and after the implementation of security controls.

The results showed a significant reduction in security events and incidents following the application of security controls, highlighting their effectiveness.

It is also important to note that if the network resources were heavily utilized by regular users, more security events and alerts might have been generated within the 24-hour period after the security controls were implemented.# Cloud-SOC
