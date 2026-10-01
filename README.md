# SOC Incident Investigation & Detection Engineering

## Overview

This project documents a simulated SOC investigation involving a phishing campaign that resulted in suspicious PowerShell execution across multiple endpoints.

Using Microsoft Defender XDR and KQL, I investigated endpoint activity, analyzed PowerShell execution, identified network indicators of compromise (IOCs), scoped additional affected systems, investigated phishing email activity, and documented containment and remediation actions.

> **Lab Disclaimer:** This is a simulated cybersecurity investigation created for educational and portfolio purposes. All users, devices, domains, IP addresses, alerts, and events are fictional.

## Scenario

A high-severity Microsoft Defender alert identified suspicious PowerShell activity on `FIN-WS-07`.

Initial investigation identified:

- `ExecutionPolicy Bypass`
- An `EncodedCommand`
- PowerShell launching after a suspicious Microsoft Word document was opened
- Communication with suspicious external infrastructure

The investigation was expanded across the environment to determine whether additional endpoints or users were affected.

## Investigation Objectives

- Triage the initial Microsoft Defender alert.
- Determine whether the PowerShell activity was malicious.
- Investigate the suspicious external network connection.
- Identify additional endpoints communicating with the network IOC.
- Determine the scope of the phishing campaign.
- Analyze the suspicious PowerShell scripts.
- Contain affected endpoints and malicious infrastructure.
- Document remediation and recovery recommendations.
- Map observed attacker behavior to MITRE ATT&CK.

## Skills Demonstrated

- Microsoft Defender XDR alert triage and endpoint investigation
- KQL security log analysis and threat hunting
- Phishing email investigation
- Endpoint process and network activity analysis
- Indicator of Compromise (IOC) identification and hunting
- Environment-wide incident scoping
- PowerShell activity analysis
- Incident containment and remediation planning
- Security incident documentation
- MITRE ATT&CK technique mapping
- Evidence-based security analysis

  ## Investigation Workflow

Microsoft Defender Alert  
↓  
PowerShell Triage  
↓  
Process & Network Investigation  
↓  
IOC Identification  
↓  
Environment-Wide Scoping  
↓  
Phishing Email Investigation  
↓  
Script Analysis  
↓  
Containment  
↓  
Remediation & Recovery  
↓  
MITRE ATT&CK Mapping




## KQL Queries

### 1. Initial PowerShell Triage
Investigates PowerShell execution on the initially affected endpoint and provides visibility into the command line and the application or process that launched PowerShell.

[View Query](queries/01-initial-triage.kql)

### 2. Network IOC Hunt
Searches network telemetry across the environment for endpoints communicating with the identified suspicious external IP address.

[View Query](queries/02-network-ioc-hunt.kql)

### 3. Endpoint Scoping
Reviews endpoint process activity within a specific time window to reconstruct activity surrounding suspicious PowerShell execution.

[View Query](queries/03-endpoint-scoping.kql)

### 4. Phishing Email Investigation
Searches email attachment telemetry to identify users who received the suspicious attachments and determine the potential scope of the phishing campaign.

[View Query](queries/04-email-investigation.kql)


## Key Findings

- Five users received suspicious phishing attachments during the simulated campaign.
- Three endpoints demonstrated correlated suspicious PowerShell and network activity.
- Multiple affected endpoints communicated with the same suspicious external infrastructure.
- Script analysis identified activity targeting Chrome login-related data.
- The targeted data was staged into a ZIP archive before an attempted HTTP POST transfer to external infrastructure.
- The affected endpoints were isolated and identified malicious infrastructure was blocked as part of containment.

## Project Structure

SOC-Incident-Investigation/
│
├── README.md
│
├── queries/
│   ├── 01-initial-triage.kql
│   ├── 02-network-ioc-hunt.kql
│   ├── 03-endpoint-scoping.kql
│   └── 04-email-investigation.kql
│
├── evidence/
│   └── investigation-timeline.md
│
└── incident-report/
    └── incident-report.md


 ## Incident Report

The complete investigation report includes the incident summary, investigation findings, containment actions, remediation and recovery steps, security recommendations, and MITRE ATT&CK mapping.

[View Full Incident Report](incident-report/incident-report.md)

## Investigation Timeline

A chronological timeline was created to document the progression of the simulated incident across the affected users and endpoints.

[View Investigation Timeline](evidence/investigation-timeline.md)

## Tools & Technologies

- Microsoft Defender XDR
- Kusto Query Language (KQL)
- PowerShell
- Microsoft security telemetry
- MITRE ATT&CK

## Disclaimer

This project is a simulated cybersecurity investigation created for educational and portfolio purposes. No real organization, users, devices, IP addresses, or security incidents are represented in this repository.
