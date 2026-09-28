# File Integrity Monitoring Evidence

This directory contains evidence from the File Integrity Monitoring (FIM) scenario conducted in the Wazuh laboratory.

## Lab Scenario

Wazuh was configured to monitor the directory `C:\Users\Public\monitored` on the Windows endpoint.

The test involved controlled file creation, modification, and deletion to examine the resulting file integrity events.

## Evidence

- [FIM monitoring results](fim-monitoring-dashboard.png): Dashboard evidence of file integrity activity recorded by Wazuh.
- Separate file-operation logs and Windows Event Viewer screenshots are not included; the dashboard image is the available FIM evidence.

## Security and Evidence Notes

- Public screenshots must be reviewed and sanitized to remove original lab IP addresses, server URLs, and unnecessary environment-specific identifiers.
- Screenshots must preserve the original observed results.
- Evidence descriptions must not imply that an event was captured in a screenshot unless that screenshot is available.

## Scope

This evidence supports the file monitoring, telemetry collection, and detection stages of the laboratory exercise.
