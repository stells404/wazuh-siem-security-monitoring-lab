# Wazuh SIEM & Security Monitoring Lab

A hands-on cybersecurity lab built to deploy and validate Wazuh for endpoint monitoring, file integrity monitoring, network reconnaissance, controlled SMB authentication attack simulation, security-event analysis, detection, and investigation.

> **Portfolio note:** This repository documents work performed in an isolated virtual lab. IP addresses, usernames, host-specific identifiers, and other environment-specific information shown in the public documentation are sanitized where appropriate.

---

## Project Overview

This project documents the deployment and validation of a Wazuh-based security monitoring environment using virtualized Windows and Linux endpoints.

The lab was used to explore how security activity moves from:

**Reconnaissance → Attack Activity → Security Telemetry → Detection → Alert → Investigation**

Two primary security-monitoring scenarios were investigated:

1. **Controlled SMB brute-force/authentication attack**
2. **File Integrity Monitoring (FIM)**

The objective was not simply to deploy Wazuh, but to understand how endpoint and network activity can generate security telemetry that can be collected, correlated, detected, and investigated through a SIEM platform.

---

## Lab Architecture

The laboratory environment consisted of:

- Wazuh Server
- Wazuh Indexer
- Wazuh Manager
- Wazuh Dashboard
- Windows 10 endpoint with Wazuh Agent
- Kali Linux endpoint with Wazuh Agent
- pfSense network environment
- Isolated virtual network

### Public Documentation Addressing

IP addresses shown in public documentation are sanitized representations of the original lab environment.

| Component | Documentation Address |
|---|---|
| pfSense | `10.10.10.1` |
| Wazuh Server | `10.10.10.10` |
| Windows Endpoint | `10.10.10.20` |
| Kali Linux | `10.10.10.30` |
| Lab Network | `10.10.10.0/24` |

> These addresses are used only for public documentation and do not represent the original lab addressing.

---

## Technologies & Tools

### Security Monitoring

- Wazuh SIEM
- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard
- Wazuh Agent
- File Integrity Monitoring (FIM)
- Windows security-event monitoring

### Operating Systems

- Ubuntu Server
- Windows 10
- Kali Linux
- pfSense

### Security Testing

- Nmap
- NetExec
- SMB
- Windows authentication telemetry

### Virtualization

- Virtualized laboratory environment
- Isolated network segmentation

---

# Security Monitoring Scenario 1 — SMB Brute-Force Attack

## Objective

A controlled SMB authentication attack was simulated against the Windows endpoint to examine how repeated authentication attempts generate security telemetry and how Wazuh can be used to detect and investigate the resulting activity.

The attack was performed only within the isolated laboratory environment.

---

## Phase 1 — Network Reconnaissance

Nmap was used to identify services exposed by the Windows endpoint.

The reconnaissance identified:

```text
TCP/445 — Microsoft-DS / SMB
