# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

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

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![(before) nsg-malicious-allowed-in](https://github.com/jedioverlander/Cloud-SOC/assets/143690773/baef3b1c-4fa9-4b83-a8f7-2fd5d82c3a7b)
![(before) syslog-ssh-auth-fail](https://github.com/jedioverlander/Cloud-SOC/assets/143690773/2ad950be-0a67-4665-ba64-facba7f18ec7)
![(before) windows-rdp-smb-auth-fail](https://github.com/jedioverlander/Cloud-SOC/assets/143690773/ad8784ca-8027-4608-bcbe-749f9f64d504)
![(before) mssql-auth-fail](https://github.com/jedioverlander/Cloud-SOC/assets/143690773/e9595d82-5302-4682-b753-37b1f90b795a)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-08-28 5:52:29
Stop Time 2023-08-29 5:52:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 137281
| Syslog                   | 4916
| SecurityAlert            | 5
| SecurityIncident         | 376
| AzureNetworkAnalytics_CL | 2678

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-08-30 6:16:11
Stop Time	2023-08-31 6:16:11

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9681
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 3
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
