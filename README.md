# Cloud SOC + Honeynet in Azure (Live Traffic)
<img width="730" alt="SOC" src="https://github.com/audreclemons/Azure-SOC-/assets/171464782/629daaca-f5f1-4b24-86e8-b46273be44e5">


## Introduction

In this project, I set up a mini honeynet in Azure and collected log data from various sources into a Log Analytics workspace. Microsoft Sentinel then utilized this data to create attack maps, trigger alerts, and generate incidents. Initially, I measured security metrics in the unsecured environment over 24 hours, applied security controls to harden the environment, and then measured the metrics again for another 24 hours. The results are presented below. The metrics include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into honeynet)

## Architecture Before Hardening / Security Controls
![Azure_SOC](https://github.com/audreclemons/Azure-SOC-/assets/171464782/4993222e-b81d-4f54-84b7-b80047260b13)


## Architecture After Hardening / Security Controls
![Azure_SOC- After](https://github.com/audreclemons/Azure-SOC-/assets/171464782/a2a6384b-d67a-4b1e-a83b-1e1a93ecc093)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (1 windows with microsoft SQL DB, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were initially deployed and exposed to the internet. The Virtual Machines had their Network Security Groups (NSGs) and built-in firewalls completely open, and all other resources had public endpoints visible to the Internet, without using Private Endpoints. Additionally, NSGs were not applied to any subnets.

For the "AFTER" metrics, I hardened the Network Security Groups by blocking all traffic except from my admin workstation and applied NSGs to the subnets. Additionally, all other resources (Key Vault & Blob Storage) were secured with built-in firewalls and Private Endpoints.


## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-05-15 23:45:12
Stop Time 2024-05-16 23:45:12

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 63283
| Syslog                   | 10737
| SecurityAlert            | 53
| SecurityIncident         | 363
| AzureNetworkAnalytics_CL | 2414

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-05-20 04:02:33
Stop Time	2024-05-21 04:02:33

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 21724
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
