# SMB Brute-Force Attack Detection

## Overview

This case study documents a controlled SMB authentication attack performed within an isolated virtual cybersecurity laboratory.

The objective was to understand how reconnaissance and repeated SMB authentication attempts can generate security telemetry and how Wazuh can be used to monitor and investigate the resulting activity.

---

## Lab Scenario

The test involved:

- Kali Linux as the testing endpoint
- Windows 10 as the target endpoint
- Wazuh as the security monitoring platform
- SMB exposed through TCP/445
- NetExec for controlled authentication testing
- Nmap for network reconnaissance

All testing was performed within an isolated laboratory environment.

---

## 1. Reconnaissance

Nmap was used to identify services exposed by the Windows endpoint.

The reconnaissance identified:

```text
TCP/445 — Microsoft-DS / SMB
```

The reconnaissance confirmed that SMB was exposed on TCP/445 on the Windows target.

This established that SMB was available as the authentication service for the subsequent controlled testing.

**Sanitized Documentation Address**

The Windows endpoint is represented in this public documentation as:

```text
192.0.2.20
```
`192.0.2.20` is a documentation-only address used to represent the Windows endpoint in this public repository. It was not the actual IP address used during the laboratory exercise.


## 2. SMB Authentication Testing

The reconnaissance phase established that SMB was available on the Windows target through TCP/445.

Because the Windows 10 Home endpoint did not provide the RDP service required for the planned remote-access approach, SMB was selected for the controlled authentication test.

An initial Hydra-based approach was attempted but was not successful in this lab environment. The testing was subsequently performed using NetExec against the SMB service.

The authentication attempts were intentionally conducted within the isolated laboratory environment to generate Windows authentication telemetry for Wazuh monitoring.

No real-world systems or credentials were involved in the exercise.

### Controlled Authentication Attempts

The SMB authentication test was performed from the Kali Linux endpoint against the Windows target using NetExec.

The original laboratory command is not reproduced here because it contained environment-specific addressing and a test username. The following sanitized representation illustrates the command structure used during the exercise:

```bash
netexec smb 192.0.2.20 -u lab-user -p [REDACTED]
```
The address and username shown above are sanitized documentation values and were not the original values used during testing.
## 3. Windows Security Telemetry

The repeated SMB authentication attempts generated Windows security telemetry on the target endpoint.

### Event ID 4625 — Failed Logon

Windows Event ID 4625 was generated for failed authentication attempts.

Each unsuccessful authentication attempt contributed to the sequence of Windows security events observed during the controlled testing.

The exercise demonstrated how repeated authentication failures can generate endpoint telemetry that can subsequently be collected and analyzed by Wazuh.

### Event ID 4740 — Account Lockout

After repeated authentication failures, the test account became locked out.

Windows Event ID 4740 was generated when the account lockout occurred.

This provided an additional security event indicating that the repeated authentication attempts had triggered a defensive control on the Windows endpoint.

### Telemetry Sequence

The documented sequence was:

```text
SMB authentication attempt
        ↓
Failed authentication
        ↓
Windows Event ID 4625
        ↓
Repeated authentication failures
        ↓
Account lockout
        ↓
Windows Event ID 4740
        ↓
Wazuh collection and detection
```
The Event ID sequence is documented from the laboratory exercise and presentation evidence. Original Event Viewer screenshots for Events 4625 and 4740 are not included in the public repository.
## 4. Wazuh Detection & Correlation

The Windows authentication events were collected by the Wazuh agent and processed by the Wazuh detection engine.

Wazuh identified individual authentication failures as well as the resulting account lockout and repeated-failure pattern.

### Detection Rules

The documented Wazuh rules associated with the activity were:

| Rule ID | Severity | Detection |
|---|---:|---|
| 60122 | Level 5 | Logon failure — unknown user or bad password |
| 60115 | Level 9 | User account locked out |
| 60204 | Level 10 | Multiple Windows logon failures / correlation |

### Dashboard Results

The Wazuh dashboard documented:

```text
Authentication Failures: 15
Authentication Successes: 0
```
The alert activity included rules 60122, 60115, and 60204.
This demonstrated that Wazuh was able to detect both individual failed authentication events and the broader repeated-failure pattern.

### Detection Sequence
```
Windows Event ID 4625
        ↓
Wazuh event collection
        ↓
Rule 60122 — Logon failure
        ↓
Repeated authentication failures
        ↓
Rule 60204 — Multiple failures / correlation
        ↓
Account lockout
        ↓
Rule 60115 — Account locked out
        ↓
Wazuh security alert
```
## 5. Investigation & Findings

The Wazuh alerts were analyzed as part of the controlled SMB authentication exercise to establish the sequence of activity and its observable security impact.

### Observed Activity

The documented activity followed this sequence:

```text
Kali Linux
    ↓
SMB authentication attempts
    ↓
Windows authentication failures
    ↓
Event ID 4625
    ↓
Repeated failures
    ↓
Account lockout
    ↓
Event ID 4740
    ↓
Wazuh detection and correlation
```
The testing demonstrated that repeated authentication failures generated security telemetry on the Windows endpoint and that Wazuh was able to process the resulting events.

## Investigation Findings
### Finding 1 — SMB was exposed on the Windows endpoint

Nmap reconnaissance confirmed that TCP/445 was open, establishing SMB as the service used for the controlled authentication test.

### Finding 2 — Repeated authentication failures generated Windows security telemetry

The documented Event ID 4625 activity showed that unsuccessful authentication attempts were recorded by Windows.

### Finding 3 — Repeated failures resulted in account lockout

The documented Event ID 4740 event showed that the repeated authentication attempts triggered the Windows account-lockout control.

### Finding 4 — Wazuh correlated the authentication activity

Wazuh detected the authentication failures and correlated the repeated activity using its Windows authentication detection rules.

### Finding 5 — The activity produced measurable security impact

The account lockout demonstrated an observable defensive response on the Windows endpoint. The Wazuh dashboard also documented 15 authentication failures and 0 authentication successes during the detection exercise.

## Security Significance

The exercise demonstrated the complete monitoring path from network reconnaissance and controlled authentication activity to endpoint telemetry, Wazuh detection, correlation, and security alerting.

The results show how individual authentication failures can become a higher-level security finding when the events are analyzed as a sequence rather than as isolated log entries.
