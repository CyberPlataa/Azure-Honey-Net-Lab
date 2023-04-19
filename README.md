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

## Simulated Attacks

I also took the opportunity to simulate specific attacks via PowerShell scripts or by manually triggering events. The results were observed in Log Analytics Workspace and Sentinel Incident Creation.  

- Linux Brute Force Attempt 
- AAD Brute Force Success 
- Windows Brute Force Success
- Malware Detection (EICAR Test File) 
- Privilege Escalation  

<a href="https://imgur.com/PMCkLMs"><img src="https://i.imgur.com/PMCkLMs.png" title="source: imgur.com" /></a>




## Architecture Before Hardening / Security Controls
<a href="https://imgur.com/IPvNyOu"><img src="https://i.imgur.com/IPvNyOu.png" title="source: imgur.com" /></a>

## Architecture After Hardening / Security Controls
<a href="https://imgur.com/SHXf8h7"><img src="https://i.imgur.com/SHXf8h7.png" title="source: imgur.com" /></a>

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux) 
- Log Analytics Workspace with KQL Queries
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel
- Microsoft Defender for the Cloud
- Windows Remote Desktop
- Command Line Interface
- PowerShell
- NIST SP 800-53 r4
- NIST SP 800-61 r2

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

## Utilizing NIST 800.61r2 Computer Incident Handling Guide

For each simulated attack I then practiced incident responses following NIST SP 800-61 r2.

![NIST 800.61](https://i.imgur.com/6PTG7c0l.png)

Each organization will have policies related to an incident response that should be followed. This event is just a walkthrough for possible actions to take in the detection of malware on a workstation.  

#### Preparation

- The Azure lab was set up to ingest all of the logs into Log Analytics Workspace, Sentinel and Defender were configured, and alert rules were put in place. The reason our environment was flooded with alerts and brute force attempts were due to our NSG's not having properly configured firewalls that filtered outside traffic on the internet. 

#### Detection & Analysis
- Created an [*Incident Response*](https://github.com/CyberPlataa/Incident-Response) Section for this.
- Malware had been detected on a workstation with the potential to compromise the confidentiality, integrity, or availability of the system and data.
- Assigned alert to an owner, set the severity to "High", and the status to "Active"
- Identified the primary user account of the system and all systems affected.
- A full scan of the system was conducted using up-to-date antivirus software to identify the malware.
- Verified the authenticity of the alert as a "True Positive".
- Sent notifications to appropriate personnel as required by the organization's communication policies.

#### Containment, Eradication & Recovery
- The infected system and any additional systems infected by the malware were quarantined.
- If the malware was unable to be removed or the system sustained damage, the system would have been shut down and disconnected from the network.

#### Post-Incident Activity

- In this simulated case, an employee had been testing and creating EICAR files. 
- All information was gathered and analyzed to determine the root cause, extent of damage, and effectiveness of the response. 
- Report disseminated to all stakeholders.
- Actions were taken to correct mistakes. [*Corrected our firewall settings*](https://github.com/CyberPlataa/Secure-Cloud-Configuration)
- Lessons-learned review of the incident was conducted.

<br />




## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It should be noted that the number of security events and incidents were drastially reduced after the security controls/security best practices were applied, demonstrating their effectiveness.

If the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
