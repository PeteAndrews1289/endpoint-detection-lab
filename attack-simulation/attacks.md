# Attack Simulation Plan

This file documents the endpoint behaviors the lab is designed to generate and detect. The goal is detection engineering practice in an isolated lab, not unauthorized activity.

## Lab Scope

- Attacker system: Kali Linux VM
- Target system: Windows 10 VM
- Telemetry: Sysmon and Windows Event Logs
- SIEM: Splunk Enterprise

## Simulated Behaviors

### 1. Network Reconnaissance

Objective: generate endpoint network telemetry that shows scanning or broad connection behavior.

Detection value:

- Sysmon network connection events
- High destination count from one source process
- Repeated connection attempts to multiple ports or hosts

### 2. Suspicious PowerShell

Objective: generate suspicious command-line telemetry for script execution and investigation practice.

Detection value:

- Encoded command flags
- Web request patterns
- Unusual parent-child process chains
- PowerShell launched by unexpected parent processes

### 3. Credential Access Indicator

Objective: generate telemetry associated with process access to sensitive Windows processes such as LSASS.

Detection value:

- Sysmon Event ID 10
- Source process touching `lsass.exe`
- Suspicious granted access values
- Unusual process lineage

### 4. Privilege Escalation / Account Change

Objective: generate Windows Security events for local account creation or group membership changes.

Detection value:

- New user creation events
- Local administrator group changes
- Account changes occurring near suspicious process activity

### 5. Persistence Through Scheduled Tasks

Objective: generate telemetry for scheduled task creation.

Detection value:

- Windows scheduled task event IDs
- `schtasks.exe` process creation
- Suspicious command lines or task names

## Evidence to Collect

- Splunk query results for each simulated behavior
- Sysmon process creation and network connection records
- Windows Security account-change records
- Timeline showing behavior order
- Short analyst notes explaining why each event matters

## Safety Notes

- Run simulations only in an isolated lab environment.
- Do not use real credentials, production systems, or public targets.
- Keep payloads and artifacts benign when the goal is telemetry generation.
- Focus documentation on detection logic and investigation workflow.
