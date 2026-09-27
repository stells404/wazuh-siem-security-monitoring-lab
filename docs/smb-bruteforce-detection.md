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

The authentication test was executed repeatedly to generate a sequence of failed SMB authentication events. The first attempt produced a logon failure, while subsequent attempts resulted in account lockout after repeated authentication failures.

The testing remained within the isolated laboratory environment and was performed against the designated Windows endpoint.
