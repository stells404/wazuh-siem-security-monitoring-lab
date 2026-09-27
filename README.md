# Wazuh SIEM & Security Monitoring Lab

A hands-on security monitoring laboratory demonstrating the deployment and use of Wazuh as a Security Information and Event Management (SIEM) and Host-Based Intrusion Detection System (HIDS).

The project uses isolated virtual machines to generate controlled security events, collect endpoint telemetry, detect suspicious activity, and investigate resulting alerts.

## Project Objectives

The laboratory focuses on two primary detection scenarios:

1. File Integrity Monitoring (FIM)
2. SMB authentication brute-force detection

The project also demonstrates Wazuh agent deployment on both Windows and Linux endpoints and the use of Windows security telemetry to support centralized detection and investigation.

---

## Lab Architecture

The laboratory was built using an isolated virtual network containing a central Wazuh server, monitored Windows and Linux endpoints, and a pfSense network gateway.

### Lab Components

| Component | Role | Platform |
|---|---|---|
| Wazuh Server | Centralized security monitoring, event analysis, and dashboard | Ubuntu Server |
| Windows Endpoint | Wazuh agent and controlled security-testing target | Windows 10 |
| Kali Linux | Wazuh agent and controlled security-testing source | Kali Linux |
| pfSense | Network gateway and DHCP | pfSense |

### Architecture

```text
                         ┌─────────────────────┐
                         │       pfSense       │
                         │   Gateway / DHCP    │
                         └──────────┬──────────┘
                                    │
                         Isolated Lab Network
                                    │
              ┌─────────────────────┼─────────────────────┐
              │                     │                     │
              ▼                     ▼                     ▼
     ┌────────────────┐    ┌────────────────┐    ┌────────────────┐
     │  Wazuh Server  │    │ Windows        │    │ Kali Linux     │
     │                │    │ Endpoint       │    │                │
     │ • Indexer      │    │                │    │ • Wazuh Agent  │
     │ • Manager      │    │ • Wazuh Agent  │    │ • Test Source  │
     │ • Dashboard    │    │ • Test Target  │    │                │
     └────────────────┘    └────────────────┘    └────────────────┘
```
---

## Technologies & Tools

The laboratory used the following technologies and security tools.

### Security Monitoring

- **Wazuh** — Security monitoring and detection platform
- **Wazuh Dashboard** — Security event visualization and investigation
- **Wazuh Indexer** — Storage and indexing of security data
- **Wazuh Server/Manager** — Event processing and detection-rule evaluation
- **Wazuh Agent** — Endpoint telemetry collection
- **File Integrity Monitoring (FIM)** — Detection of monitored file changes

### Operating Systems

- **Ubuntu Server** — Wazuh server platform
- **Windows 10** — Monitored endpoint and controlled testing target
- **Kali Linux** — Security-testing system and monitored Linux endpoint

### Network Infrastructure

- **pfSense** — Lab gateway and DHCP
- **Virtualized isolated network** — Laboratory network environment

### Security Testing

- **Nmap** — Network reconnaissance and service discovery
- **NetExec** — SMB authentication testing
- **PowerShell** — Windows endpoint administration and controlled file operations
- ---

## Project Workflow

The project follows a practical security monitoring workflow:

```text
Reconnaissance
      ↓
Security Activity
      ↓
Endpoint Telemetry
      ↓
Wazuh Collection
      ↓
Detection & Correlation
      ↓
Security Alert
      ↓
Investigation
      ↓
Security Findings
```

---

## Detection Scenarios

The laboratory contains two controlled security-monitoring scenarios designed to demonstrate different Wazuh detection capabilities.

### Scenario 1 — File Integrity Monitoring

The first scenario demonstrates File Integrity Monitoring (FIM) on the Windows endpoint.

A monitored directory was configured and controlled file operations were performed to generate file creation, modification, and deletion events.

The purpose of this exercise was to demonstrate how Wazuh detects changes to monitored files and reports the resulting security events.

Detection flow:

```text
File Change
     ↓
Wazuh Syscheck
     ↓
Integrity Check
     ↓
Security Event
     ↓
Wazuh Detection
     ↓
Dashboard Alert
```

### Scenario 2 — SMB Authentication Brute-Force Detection

The second scenario demonstrates the detection of repeated SMB authentication failures against the Windows endpoint.

Network reconnaissance was first performed from the Kali Linux testing system to identify whether the SMB service was accessible on the target.

Because the Windows endpoint used in the laboratory did not provide an RDP server, SMB was used as the authentication-testing protocol instead.

Controlled authentication attempts were then generated against the SMB service. The resulting failed authentication activity was recorded by Windows security logging and collected by Wazuh.

Repeated authentication failures eventually resulted in an account lockout event, providing additional security telemetry for detection and investigation.

Detection flow:

```text
SMB Reconnaissance
        ↓
SMB Service Identified
        ↓
Controlled Authentication Attempts
        ↓
Windows Event ID 4625
        ↓
Repeated Authentication Failures
        ↓
Account Lockout
        ↓
Windows Event ID 4740
        ↓
Wazuh Detection & Correlation
        ↓
Security Alert
```

---

## Wazuh Agent Deployment

Wazuh agents were deployed to the Windows and Kali Linux endpoints so that security telemetry from each system could be collected and monitored centrally by the Wazuh server.

### Agent Architecture

The deployment followed this model:

```text
                    Wazuh Server
                         │
              Centralized Monitoring
                         │
             ┌───────────┴───────────┐
             │                       │
             ▼                       ▼
      Windows Endpoint         Kali Linux
       Wazuh Agent             Wazuh Agent
             │                       │
             └───────────┬───────────┘
                         │
                  Security Telemetry
```

### Windows Agent

### Kali Linux Agent

### Agent Verification
