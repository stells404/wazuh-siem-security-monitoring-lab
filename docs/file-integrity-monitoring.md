# File Integrity Monitoring (FIM)

## Objective
The objective of this exercise was to verify that Wazuh could detect and report file-integrity changes on a monitored Windows endpoint.

The test focused on three controlled file operations:

- File creation
- File modification
- File deletion

The exercise used Wazuh's `syscheck` component to monitor a designated directory and generate file-integrity events when changes occurred.

## FIM Configuration
Wazuh's `syscheck` component was configured on the Windows endpoint to monitor a designated directory for file-integrity changes.

The configuration used the following settings:

- `check_all="yes"` — monitor the full set of supported file attributes.
- `realtime="yes"` — enable real-time monitoring so changes can be detected as they occur.

The monitored directory was:


```text
C:\Users\Public\monitored
```

## Controlled File Activity

### File Creation
A test file was created in the monitored directory to generate a file-integrity event.

The demonstration created a file named `secret.txt` using PowerShell:

```powershell
echo "test" > C:\Users\Public\monitored\secret.txt
```

### File Modification
The contents of `secret.txt` were then modified by appending additional text to the existing file.

The demonstration used PowerShell:

```powershell
echo "changed" >> C:\Users\Public\monitored\secret.txt
```

### File Deletion
The test file was then deleted from the monitored directory to generate a file-deletion event.

The demonstration used PowerShell:

```powershell
del C:\Users\Public\monitored\secret.txt
```

## Detection
Wazuh detected the file-integrity changes through its `syscheck` monitoring and detection pipeline.

The documented detection chain was:

```text
File change on Windows endpoint
        ↓
Wazuh syscheck
        ↓
File integrity comparison
        ↓
Event forwarded to Wazuh server
        ↓
FIM detection rules 550–554
        ↓
Dashboard alert
```

## Evidence
The FIM results were reviewed in the Wazuh dashboard for the monitored Windows agent.

The available evidence shows three file-integrity actions:

| Activity | Observed Result |
|---|---|
| File creation | `secret.txt` was created |
| File modification | Content was appended to `secret.txt` |
| File deletion | `secret.txt` was removed |

The Wazuh dashboard displayed the resulting FIM activity under the `syscheck` rule group.

The documented detection results show that all three controlled file operations were detected.


## Analysis
The FIM results demonstrate Wazuh's host-based detection capability by monitoring file activity directly on the Windows endpoint.

The controlled creation, modification, and deletion of `secret.txt` each generated file-integrity activity that was detected through the Wazuh `syscheck` pipeline.

The detection process shows how an endpoint-level change can be transformed into a security event and ultimately presented as an alert in the Wazuh dashboard.

This demonstrates the following security monitoring workflow:

```text
Endpoint File Change
        ↓
Syscheck Monitoring
        ↓
File Integrity Event
        ↓
Wazuh Detection Rules
        ↓
Dashboard Alert
```

## Finding
The FIM exercise confirmed that Wazuh detected controlled file-integrity changes on the Windows endpoint.

All three tested file operations were detected:

- File creation
- File modification
- File deletion

The results demonstrate Wazuh's ability to provide host-level security monitoring through `syscheck` and report file-integrity changes through the centralised Wazuh dashboard.

The exercise therefore demonstrated Wazuh functioning as a Host-Based Intrusion Detection System  `(HIDS)` capability within the lab.

## Security Significance
File integrity monitoring provides visibility into changes occurring directly on a monitored endpoint.

In a security monitoring environment, unexpected or unauthorized changes to files can be an indicator of compromise or other suspicious activity. FIM allows these changes to be detected and reported so that they can be investigated.

In this lab, the file changes were intentional and controlled. However, the same monitoring capability can provide useful security telemetry when investigating unexpected changes on an endpoint.

The exercise demonstrates how Wazuh can provide:

- Host-level visibility into file changes
- Detection of file creation, modification, and deletion
- Centralized alerting through the Wazuh dashboard
- Security telemetry that can support further investigation
