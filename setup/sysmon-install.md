# Sysmon Setup Notes

## Purpose

Sysmon provides the endpoint telemetry needed for this lab, including process creation, network connections, process access, and persistence-related activity.

## Recommended Telemetry

- Process creation with command line
- Network connections
- Process access events
- File creation events
- Registry changes, if using a richer configuration
- Driver and image load events, if needed for deeper analysis

## Configuration Guidance

Use a community-reviewed Sysmon configuration as a starting point, then tune it for lab objectives. The configuration should preserve enough detail for detection engineering without creating excessive noise.

Recommended event types for this lab:

- Event ID 1: Process creation
- Event ID 3: Network connection
- Event ID 10: Process access
- Event ID 11: File creation
- Event ID 12, 13, 14: Registry activity

## Validation Checklist

- Confirm Sysmon service is running.
- Confirm Sysmon events appear in Windows Event Viewer.
- Confirm process command lines are captured.
- Confirm network connection events are visible.
- Confirm Splunk receives Sysmon events from the endpoint.

## Evidence to Save

- Sysmon configuration file
- Event Viewer export or sanitized sample events
- Splunk search results showing Sysmon Event ID 1, 3, and 10
