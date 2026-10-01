# Investigation Timeline

## Incident Overview

This timeline documents a simulated SOC investigation involving a phishing campaign that resulted in suspicious PowerShell execution, communication with external infrastructure, and potential credential-data exfiltration.

> **Lab Note:** This is a simulated cybersecurity investigation created for educational and portfolio purposes. All users, devices, domains, IP addresses, and events are fictional.

---

## Timeline

| Time | Device / User | Event | Analyst Assessment |
|------|---------------|-------|-------------------|
| 09:13 | j.miller | Received `Invoice_September.docm` via email | Suspicious phishing attachment identified |
| 09:18 | r.thomas | Received `Invoice_September.docm` via email | Recipient identified during campaign scoping |
| 09:27 | a.wilson | Received `Benefits_Update.zip` via email | Suspicious phishing attachment identified |
| 09:31 | s.jackson | Received `Benefits_Update.zip` via email | Recipient identified during campaign scoping |
| 09:44 | m.rodriguez | Received `Benefits_Update.zip` via email | Suspicious phishing attachment identified |
| 09:52 | HR-WS-09 / m.rodriguez | `benefits.ps1` executed with PowerShell using `ExecutionPolicy Bypass` | Endpoint identified as likely compromised |
| 09:52 | HR-WS-09 | PowerShell connected to suspicious external infrastructure | Endpoint added to incident scope |
| 10:42 | FIN-WS-07 / j.miller | `WINWORD.EXE` launched PowerShell after `Invoice_September.docm` was opened | Suspicious parent-child process relationship |
| 10:42 | FIN-WS-07 | PowerShell executed an encoded command with `ExecutionPolicy Bypass` | High-confidence suspicious execution |
| 10:42 | FIN-WS-07 | PowerShell established an outbound connection to suspicious infrastructure | Network IOC identified |
| 10:49 | HR-WS-12 / a.wilson | `Benefits_Update.zip` opened/extracted | Beginning of suspicious execution chain |
| 10:50 | HR-WS-12 | `benefits.ps1` executed using hidden PowerShell with `ExecutionPolicy Bypass` | Likely malicious script execution |
| 10:51 | HR-WS-12 | PowerShell connected to the same suspicious infrastructure | Correlated endpoint with existing IOC |

---

## Affected Endpoints

### FIN-WS-07
**User:** j.miller

Observed execution chain:

`Invoice_September.docm` → `WINWORD.EXE` → `powershell.exe` → Encoded Command → External Connection

**Status:** Isolated

### HR-WS-12
**User:** a.wilson

Observed execution chain:

`Benefits_Update.zip` → `benefits.ps1` → `powershell.exe` → External Connection

**Status:** Isolated

### HR-WS-09
**User:** m.rodriguez

Observed execution chain:

`Benefits_Update.zip` → `benefits.ps1` → `powershell.exe` → External Connection

**Status:** Isolated

---

## Current Scope

- 5 users received suspicious phishing attachments.
- 3 endpoints demonstrated correlated suspicious activity.
- 3 affected endpoints were contained.
- Multiple endpoints communicated with the same suspicious external infrastructure.
- Script analysis identified collection and staging of Chrome login-related data.
- The script attempted to transmit the staged archive to external infrastructure.

- ## Analyst Assessment

The investigation began after an alert identified suspicious PowerShell activity on FIN-WS-07. 
PowerShell activity alone was not enough to determine that the device was compromised, but the
combination of `ExecutionPolicy Bypass`, an `EncodedCommand`, PowerShell launching after a 
suspicious Word document was opened, and an outbound connection provided enough evidence to
investigate further. Environment-wide scoping identified five users who received suspicious 
phishing attachments, with three endpoints showing related PowerShell and network activity.
Further script analysis identified the collection and staging of Chrome login-related data 
and an attempted transfer to external infrastructure. The three affected endpoints were 
isolated, and the identified malicious sender and network IOC were blocked as part of containment.
