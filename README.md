# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

For this project, I learn how to set up a small honeynet in Azure and collected log data into a workspace for Log Analytics from several sources. I use Microsoft Sentinel workspace to create attack maps, trigger alerts, and create incidents. I conducted a 24-hour evaluation of specific security metrics in the environment, put security measures in place to strengthen the environment, and re-evaluated metrics after 24 hours, and presented my findings below. The following metrics will be displayed:

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
- Virtual Machines (2 windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic except my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![image](https://github.com/soriesesay1/Azure-SOC/assets/154941302/cf067c0f-8a8c-40dd-b006-607cfb474781)
![image](https://github.com/soriesesay1/Azure-SOC/assets/154941302/8b203cb7-b1a7-45fd-8f82-c35167f52f0a)
![image](https://github.com/soriesesay1/Azure-SOC/assets/154941302/9be64ef0-232a-43ea-bf5b-64ef6a84f55b)
![image](https://github.com/soriesesay1/Azure-SOC/assets/154941302/cc586f42-ac99-4634-8da9-efc9e9d8d6ab)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-12-15T04:15:41.5076376Z
Stop Time 22023-12-16T04:15:41.5076376Z

| Metric                                       | Count
| ------------------------                     | -----
| SecurityEvent (Windows VMs)                  | 22012
| Syslog (Linux VMs)                           | 2706
| SecurityAlert (Microsoft Defender for Cloud) | 6
| SecurityIncident (Sentinel Incidents)        | 172
| NSG Inbound Malicious Flows Allowed          | 1471

## Attack Maps Before Hardening / Security Controls

```All map queries returned no results due to no instances of malicious activity for the 24 hours after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we applied security controls:
Start Time 2023-12-21T21:25:46.0355069Z
Stop Time	2023-12-22T21:25:46.0355069Z

| Metric                                       | Count
| ------------------------                     | -----
| SecurityEvent (Windows VMs)                  | 5824
| Syslog (Linux VMs)                           | 5
| SecurityAlert (Microsoft Defender for Cloud) | 0
| SecurityIncident (Sentinel Incidents)        | 0
| NSG Inbound Malicious Flows Allowed          | 0

## Result shows our security increase
![image](https://github.com/soriesesay1/Azure-SOC/assets/154941302/5ef9282c-f8db-470b-9589-e78f47921656)

## Conclusion

In this project, a mini honeynet was established on Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was utilized to initiate alerts and generate incidents based on the ingested logs. Additionally, metrics were assessed in the insecure environment before the implementation of security controls and again afterward. It's important to highlight that the implementation of security measures led to a significant reduction in the number of security events and incidents, underscoring their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24 hours following the implementation of the security controls.
