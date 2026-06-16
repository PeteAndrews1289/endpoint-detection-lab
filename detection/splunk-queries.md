# Endpoint Detection Splunk Queries

These searches are written as reusable detection templates for a Windows endpoint lab using Sysmon and Splunk. Field names may need minor adjustment depending on the sourcetypes and field extractions used in the local Splunk environment.

## Base Sysmon Process Search

```spl
index=* (sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" OR source="*Sysmon*")
EventCode=1
| table _time host User ParentImage Image CommandLine ProcessId ParentProcessId
| sort -_time
```

Purpose: review process creation events with parent-child process context.

## Suspicious PowerShell Execution

```spl
index=* (sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" OR source="*Sysmon*")
EventCode=1 Image="*\\powershell.exe"
| search CommandLine="*-enc*" OR CommandLine="*EncodedCommand*" OR CommandLine="*IEX*" OR CommandLine="*DownloadString*" OR CommandLine="*Invoke-WebRequest*"
| table _time host User ParentImage Image CommandLine
| sort -_time
```

Purpose: identify PowerShell commands commonly associated with suspicious script execution, encoded payloads, or remote content retrieval.

## LSASS Access Indicator

```spl
index=* (sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" OR source="*Sysmon*")
EventCode=10 TargetImage="*\\lsass.exe"
| table _time host SourceImage TargetImage GrantedAccess CallTrace User
| sort -_time
```

Purpose: find process access events targeting LSASS. This is useful for credential access triage, especially when the source process is unusual.

## New Local Administrator Account

```spl
index=* (sourcetype="WinEventLog:Security" OR source="*Security*")
(EventCode=4720 OR EventCode=4732)
| table _time host EventCode Account_Name Target_Account_Name Group_Name SubjectUserName
| sort -_time
```

Purpose: identify new account creation and group membership changes that may indicate privilege escalation or persistence.

## Scheduled Task Persistence

```spl
index=* (sourcetype="WinEventLog:Security" OR source="*Security*" OR source="*Sysmon*")
(EventCode=4698 OR CommandLine="*schtasks*" OR Image="*\\schtasks.exe")
| table _time host User Image CommandLine TaskName ParentImage
| sort -_time
```

Purpose: detect scheduled task creation or command-line use of `schtasks.exe`.

## Reconnaissance Through Network Scanning

```spl
index=* (sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" OR source="*Sysmon*")
EventCode=3
| stats count dc(DestinationIp) as unique_destinations values(DestinationPort) as destination_ports by host SourceIp Image
| where unique_destinations > 10 OR count > 50
| sort -count
```

Purpose: identify processes with unusually broad outbound connection behavior.

## Suspicious Command Shell Activity

```spl
index=* (sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" OR source="*Sysmon*")
EventCode=1 (Image="*\\cmd.exe" OR Image="*\\powershell.exe" OR Image="*\\wscript.exe" OR Image="*\\cscript.exe")
| table _time host User ParentImage Image CommandLine
| sort -_time
```

Purpose: review interactive shells and scripting hosts with parent process context.

## Triage Fields to Preserve

- `_time`
- `host`
- `User`
- `Image`
- `ParentImage`
- `CommandLine`
- `ProcessId`
- `ParentProcessId`
- `TargetImage`
- `GrantedAccess`
- `DestinationIp`
- `DestinationPort`

## Notes

These are lab detection templates, not production-ready correlation searches. A production environment would need allowlisting, baseline tuning, asset context, user context, and severity mapping before alerting.
