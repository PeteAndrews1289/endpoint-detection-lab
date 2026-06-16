# Splunk Forwarder Setup Notes

## Purpose

The Splunk Universal Forwarder sends Windows endpoint logs to Splunk Enterprise for detection and investigation.

## Logs to Forward

- Sysmon Operational log
- Windows Security log
- Windows PowerShell log, if enabled
- Task Scheduler Operational log, if enabled

## Validation Checklist

- Confirm the forwarder service is running on the Windows endpoint.
- Confirm the endpoint is visible from the Splunk server.
- Confirm Sysmon logs are indexed.
- Confirm Windows Security events are indexed.
- Confirm field extraction supports at least `host`, `EventCode`, `Image`, `ParentImage`, `CommandLine`, and `User`.

## Suggested Indexing Notes

For a lab, a generic index is acceptable. For a stronger portfolio repo, document the index and sourcetypes used so detection queries can be reproduced.

Useful fields:

- `EventCode`
- `Image`
- `ParentImage`
- `CommandLine`
- `User`
- `TargetImage`
- `DestinationIp`
- `DestinationPort`

## Evidence to Save

- Splunk host list showing the endpoint
- Search showing recent Sysmon events
- Search showing recent Windows Security events
- One sanitized event sample for each major detection type
