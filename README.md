# Endpoint Detection & Attack Simulation Lab

## Overview

This project simulates real-world attacker behavior on a Windows endpoint and demonstrates how to detect malicious activity using **Sysmon telemetry and Splunk SIEM**.

It extends previous cloud-based labs by introducing **endpoint-level visibility**, enabling detection of attacker techniques such as credential dumping, privilege escalation, persistence, and suspicious process activity.

The goal is to replicate how a SOC analyst or detection engineer would monitor, detect, and investigate threats on an endpoint system.

---

## Architecture

* **Kali Linux VM** – attacker system used to simulate reconnaissance and exploitation
* **Windows 10 VM** – target endpoint configured with Sysmon
* **Splunk Universal Forwarder** – forwards endpoint logs
* **Splunk Enterprise (local)** – centralized SIEM for log ingestion, detection, and analysis

---

## Attack Simulation

This lab simulates realistic attacker behavior to generate detection-worthy telemetry:

* Network scanning (Nmap)
* Credential dumping (LSASS access via Mimikatz)
* Privilege escalation (creation of administrative users)
* Persistence mechanisms (scheduled tasks)
* Suspicious process execution (PowerShell, command-line activity)

These activities generate endpoint logs that mimic real-world adversary techniques.

---

## Detection Engineering

Sysmon logs are ingested into Splunk and used to develop detection logic for:

* Credential dumping via LSASS access
* Unauthorized administrative account creation
* Persistence through scheduled task creation
* Suspicious process execution and command-line activity
* Network connections indicative of reconnaissance

Dashboards are built to visualize attacker behavior and identify anomalies across process, network, and user activity.

---

## Incident Response Simulation

This project also demonstrates basic SOC workflows by simulating how alerts are investigated:

* Identifying suspicious processes and parent-child relationships
* Correlating user activity with system behavior
* Tracing attacker actions across logs
* Performing containment actions (process termination, account disablement)

---

## Key Insights

* Endpoint telemetry is essential for detecting attacker techniques not visible in cloud logs
* Credential dumping and persistence mechanisms can be identified through process-level monitoring
* SIEM correlation enables visibility across process, network, and user activity
* Detection engineering requires both log ingestion and contextual analysis

---

## Skills Demonstrated

* Endpoint Security
* Detection Engineering
* SIEM (Splunk)
* Threat Simulation
* Incident Response
* Log Analysis
* Sysmon Configuration

---


## Screenshots

This project includes:

* Sysmon installation and configuration
* Endpoint logs in Event Viewer
* Splunk ingestion and queries
* Detection dashboards
* Simulated attack activity

---

## Future Improvements

* Add alerting and automated detection rules in Splunk
* Integrate additional endpoint telemetry sources
* Expand attack scenarios to include lateral movement
* Automate environment deployment

---

## Summary

This lab demonstrates how endpoint telemetry can be leveraged to detect real-world attacker behavior and highlights the importance of combining system-level logging with SIEM-based detection engineering.

It complements cloud security monitoring by providing visibility into host-level activity, creating a more complete security posture.
