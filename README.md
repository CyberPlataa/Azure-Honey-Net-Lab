# Building a SOC + Honeynet in Azure (Live Traffic)
<a href="https://imgur.com/E3jGjU6"><img src="https://i.imgur.com/E3jGjU6h.png" title="source: imgur.com" /></a>

## Introduction

I built a mini honeynet in Azure and ingested various log sources into a Log Analytics workspace. I used Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I gathered data for a couple of weeks but only measured security metrics in a span of several days. I applied security controls to harden the environment and plot out the data to do a side by side comparison. 


<br />

## Metrics Gathered

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents Created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

<br />

## Architecture Before Hardening / Security Controls
<a href="https://imgur.com/IPvNyOu"><img src="https://i.imgur.com/IPvNyOu.png" title="source: imgur.com" /></a>

## Architecture After Hardening / Security Controls
<a href="https://imgur.com/SHXf8h7"><img src="https://i.imgur.com/SHXf8h7.png" title="source: imgur.com" /></a>

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

<a href="https://imgur.com/UlufDhH"><img src="https://i.imgur.com/UlufDhH.png" title="source: imgur.com" /></a>

<a href="https://imgur.com/5l66egx"><img src="https://i.imgur.com/5l66egx.png" title="source: imgur.com" /></a>

<a href="https://imgur.com/l3edcHr"><img src="https://i.imgur.com/l3edcHr.png" title="source: imgur.com" /></a>

<a href="https://imgur.com/Uma510n"><img src="https://i.imgur.com/Uma510n.png" title="source: imgur.com" /></a>


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 4/8/2023, 10:43:04.656 PM
Stop Time 4/9/2023, 10:43:04.656 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 56005
| Syslog                   | 986
| SecurityAlert            | 6
| SecurityIncident         | 378
| AzureNetworkAnalytics_CL | 557

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 4/12/2023, 8:37:53.280 AM
Stop Time	4/13/2023, 8:37:53.280 AM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 17857
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastially reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
