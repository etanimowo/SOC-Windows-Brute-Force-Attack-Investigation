# Detecting Windows Brute Force Attacks Using Windows Security Logs

A SOC investigation project demonstrating how to detect and analyze a Windows RDP brute force attack using Event Viewer, Windows Security Logs, and PowerShell.

---

## Project Overview

Brute force attacks are a common technique used by attackers to gain unauthorized access to Windows systems. In many enterprise environments, attackers target Remote Desktop Protocol (RDP) services and repeatedly attempt different passwords until they successfully authenticate.

This project simulates a controlled brute force attack in a private lab and investigates the resulting authentication logs on a Windows 10 target system.

The investigation focuses on identifying:

- repeated failed logon attempts
- attack source visibility
- targeted user account
- RDP logon indicators
- brute force patterns in Windows Security logs

This project demonstrates practical Tier 1 SOC analyst skills in log analysis, incident investigation, and detection logic development.

---

## Skills Demonstrated

- Security log analysis
- Windows authentication investigation
- Brute force attack detection
- Event correlation
- PowerShell-based log review
- Incident documentation
- MITRE ATT&CK mapping

---

## Tools Used

| Tool | Purpose |
|---|---|
| Windows Event Viewer | Security log investigation |
| Windows Security Log | Authentication event source |
| PowerShell | Log querying and evidence extraction |
| Windows 10 Host | Attack simulation source |
| Windows 10-Test VM | Victim system |

---

## Lab Environment

This project was completed in a private Windows lab environment.

### Architecture

```text
Windows 10 Host
(Attack simulation)
        │
        │ RDP login attempts
        ▼
Windows 10-Test VM
(Victim system)
        │
        │ Windows Security Logs
        ▼
Event Viewer + PowerShell Analysis
```

### Systems Used

- Host machine: Windows 10
- Victim machine: Windows 10 Pro VM
- Attack path: RDP authentication attempts
- Log source: Windows Security Log

---

## Lab Objective

The goal of this lab was to simulate multiple failed RDP login attempts against a Windows 10 VM and investigate the evidence generated in the Windows Security log.

The project aimed to answer the following questions:

- How can a SOC analyst identify brute force activity in Windows logs?
- Which event IDs are relevant to failed and successful logons?
- How can RDP-based brute force behavior be distinguished from other login activity?
- What evidence supports an investigation's conclusion?

---

## Attack Simulation

The Windows 10 host machine was used to initiate repeated failed Remote Desktop login attempts against the Windows 10-Test VM.

The following process was used:

1. Enable Remote Desktop on the victim VM
2. Create a test user account
3. Clear the Security log to start with a clean dataset
4. Initiate RDP connections from the host to the VM
5. Enter incorrect passwords multiple times
6. Review resulting authentication events in Event Viewer

This generated a visible pattern of failed authentication attempts consistent with brute force activity.

---

## Relevant Windows Event IDs

| Event ID | Meaning |
|---|---|
| 4625 | Failed logon attempt |
| 4624 | Successful logon |
| 4648 | Logon attempt using explicit credentials |
| 4672 | Special privileges assigned to new logon |

The most important event in this investigation was **Event ID 4625**.

---

## Key Investigation Findings

The investigation identified the following indicators:

- multiple failed logon events in a short time period
- repeated attempts against the same user account
- same source system involved in repeated attempts
- RDP logon type observed in the event details

### Important Evidence Fields

- Account Name
- Source Network Address
- Logon Type
- Failure Reason
- Time Created

### Logon Type

A critical field in the investigation was:

```text
Logon Type: 10
```

Logon Type 10 indicates **RemoteInteractive**, which is commonly associated with **RDP activity**.

This helped confirm that the failed attempts were related to a remote login attack rather than a local console sign-in.

---

## Example Investigation Timeline

| Time | Observation |
|---|---|
| 10:01 | First failed RDP login |
| 10:01 | Additional failed authentication attempts |
| 10:02 | Repeated Event ID 4625 entries observed |
| 10:03 | Pattern confirmed as brute force behavior |

---

## Indicators of Compromise (IOCs)

| Type | Indicator |
|---|---|
| Event ID | 4625 |
| Target Account | testuser |
| Logon Type | 10 |
| Behavior | repeated failed authentication |
| Access Method | RDP |

---

## PowerShell Log Analysis

PowerShell was used to quickly extract failed logon evidence.

Example query:

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625} -MaxEvents 10 |
Select-Object TimeCreated, Id, Message
```

This made it easier to review relevant events and capture investigation evidence efficiently.

---

## Detection Logic

A simple SOC detection rule for this scenario:

```text
Trigger alert when:
- Event ID 4625 occurs 10 or more times
- within 5 minutes
- against the same account
- from the same source IP
- with Logon Type 10
```

This logic helps identify likely RDP brute force attempts.

---

## MITRE ATT&CK Mapping

| Technique | ID |
|---|---|
| Brute Force | T1110 |
| Remote Services | T1021 |

This project aligns with common attacker behavior involving repeated authentication attempts over remote access services.

---

## Screenshots

This repository includes screenshots of the key investigation stages:

- Security log cleared before testing
- RDP connection window
- failed login attempt
- multiple Event ID 4625 entries
- detailed event evidence
- PowerShell query output

See the `screenshots/` directory.

---

## Key Takeaways

This project demonstrates how Windows Security logs can be used to detect and investigate RDP brute force attacks in a controlled SOC lab environment.

It highlights the importance of:

- monitoring authentication failures
- understanding logon types
- correlating repeated failed attempts
- documenting incident evidence clearly

This is a practical example of entry-level SOC analyst work involving Windows log analysis and incident investigation.

---

## Repository Contents

- `README.md` — project overview
- `lab-setup.md` — step-by-step lab procedure
- `detection-rules.md` — detection logic and alert ideas
- `mitre-mapping.md` — ATT&CK alignment
- `queries/powershell-queries.md` — useful log queries
- `screenshots/` — evidence images
- `investigation-report.pdf` — final report
- `investigation-report.docx` — editable report version

---

## Author

Sunday Tanimowo  
Aspiring SOC Analyst  
Cybersecurity Portfolio Project – 2026
