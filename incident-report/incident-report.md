# Security Incident Report

## Incident Information

**Incident Type:** Phishing / Suspicious PowerShell Execution  
**Severity:** High  
**Status:** Contained  
**Affected Endpoints:** 3  
**Phishing Recipients Identified:** 5  

## Executive Summary

A simulated phishing campaign targeted multiple employees using malicious email attachments disguised as 
business-related documents. Investigation identified suspicious PowerShell activity on three endpoints 
that communicated with the same external infrastructure.

The affected endpoints were isolated to contain the incident. Further script analysis identified 
activity targeting Chrome login-related data, staging the data into an archive, and attempting to 
transfer the archive to external infrastructure.

## Initial Alert

The investigation began after Microsoft Defender generated a high-severity alert for suspicious 
PowerShell activity on `FIN-WS-07`.

Initial activity included:

- `ExecutionPolicy Bypass`
- `EncodedCommand`
- PowerShell execution after `Invoice_September.docm` was opened
- Outbound communication with suspicious external infrastructure

Rather than determining the activity was malicious based on PowerShell alone, additional 
endpoint and network telemetry was reviewed to determine the context and scope of the activity.


## Investigation

The investigation began after receiving an alert for suspicious PowerShell activity. I began 
triage by reviewing the activity and identified `ExecutionPolicy Bypass`, an `EncodedCommand`, 
and an unknown outbound connection, which provided enough correlated evidence to investigate 
further. During the investigation, I discovered that `Invoice_September.docm` was opened in
Microsoft Word and Word then launched PowerShell. I investigated the external IP address and 
expanded the scope across the network to determine whether other devices showed similar activity.
Further investigation identified five users who received suspicious phishing attachments, 
with three endpoints showing related PowerShell and network activity. The three affected 
endpoints were isolated, and the malicious sender and network IOC were blocked. Analysis of 
`benefits.ps1` also showed that the script targeted Chrome login-related data, compressed the
data into an archive, and attempted to transfer it to the external infrastructure.

## Evidence & Findings

1. Microsoft Defender generated an alert identifying suspicious PowerShell activity on `FIN-WS-07`.

2. PowerShell executed using `ExecutionPolicy Bypass` and an `EncodedCommand`, followed
   by an outbound connection to suspicious external infrastructure.

3. Email investigation identified five users who received suspicious phishing attachments.
   Three endpoints later demonstrated related PowerShell and network activity.

4. Analysis of `benefits.ps1` showed that the script targeted Chrome login-related data
   and compressed the collected data into `browser_data.zip`.

5. The script contained an HTTP `POST` request designed to send `browser_data.zip` to
   the external infrastructure, providing evidence of attempted data exfiltration.

## Containment Actions

- Isolated the three affected endpoints to prevent additional malicious activity and network communication.
- Blocked the identified phishing senders to prevent additional messages from reaching users.
- Removed the identified malicious phishing emails from user inboxes to reduce the risk of
  additional users interacting with the attachments.
- Blocked the identified malicious network IOC across the environment to prevent additional
  endpoints from communicating with the external infrastructure.

  ## Remediation & Recovery

- Reset credentials for affected users due to the potential exposure of Chrome login-related data and
  revoke active authentication sessions.
- Perform additional endpoint investigation and malware scanning to identify malicious files, scripts,
   persistence mechanisms, or other indicators of compromise.
- Remove identified malicious artifacts and reimage affected endpoints if the integrity of the systems
  cannot be confidently verified.
- Enforce MFA for affected accounts and review authentication activity for signs of unauthorized access.
- Verify affected endpoints are clean and no additional suspicious activity is present before reconnecting
   them to the network.

## Recommendations

- Conduct regular phishing and security awareness training so employees can better recognize suspicious
  emails, attachments, and links. Establish a clear process for users to report suspicious messages to
  the IT or security team before interacting with them.
- Enforce MFA for user accounts to provide an additional authentication control if credentials are compromised.
- Strengthen PowerShell monitoring and application-control policies to detect or restrict suspicious script
  execution while allowing legitimate administrative activity.
- Develop or tune EDR detection rules for suspicious PowerShell behavior, including encoded commands,
  execution-policy bypasses, PowerShell launching unexpectedly from applications such as Microsoft Word,
  and unexpected external network connections

  ## MITRE ATT&CK Mapping

| Tactic | Technique | Technique ID | Observed Activity |
|---|---|---|---|
| Initial Access | Spearphishing Attachment | T1566.001 | Malicious Word and ZIP attachments were delivered to users through phishing emails. |
| Execution | Command and Scripting Interpreter: PowerShell | T1059.001 | PowerShell was used to execute encoded commands and malicious scripts. |
| Credential Access | Credentials from Password Stores: Credentials from Web Browsers | T1555.003 | The PowerShell script targeted Chrome login-related data. |
| Collection | Archive Collected Data: Archive via Utility | T1560.001 | PowerShell `Compress-Archive` was used to package the targeted browser data into `browser_data.zip`. |
  
   
