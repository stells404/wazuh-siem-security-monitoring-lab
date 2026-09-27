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
