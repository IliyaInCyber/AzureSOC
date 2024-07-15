# Building a Live SOC + Honeynet in Azure
![Port 1 F](https://github.com/user-attachments/assets/d903598b-0843-493e-b7d3-db567e372b8a)


## Introduction

In this project, I created a mini honeynet in Azure, collecting log sources from various resources into a Log Analytics workspace. This workspace is utilized by Microsoft Sentinel to construct attack maps, trigger alerts, and generate incidents. Initially, I measured security metrics in the insecure environment over a 24-hour period. After implementing security controls to harden the environment, I measured the metrics again for another 24 hours. The results, highlighting the changes in the following metrics, are shown below:

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
![Screenshot 2024-07-11 144702](https://github.com/user-attachments/assets/8f04ffc3-4ff0-4540-97c1-7bb9303ee2d4)

![Screenshot 2024-07-11 144756](https://github.com/user-attachments/assets/dfaa10de-dfdb-4774-b49c-b953d8577274)

![Screenshot 2024-07-11 144923](https://github.com/user-attachments/assets/4486bd2e-21bc-4353-811e-9908ac1afd55)

![Screenshot 2024-07-11 145004](https://github.com/user-attachments/assets/0c781490-9387-48d1-9833-413c2043f0cc)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-07-10T18:36:38.9336907Z
Stop Time 2024-07-11T18:36:38.9336907Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 43370
| Syslog                   | 1808
| SecurityAlert            | 1
| SecurityIncident         | 231
| AzureNetworkAnalytics_CL | 103

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-07-12T01:32:19.9685208Z
Stop Time	2024-07-13T01:32:19.9685208Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 20516
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
