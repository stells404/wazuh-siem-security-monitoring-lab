# Wazuh SIEM and Security Monitoring Lab

A hands-on portfolio lab using Wazuh to monitor Windows and Kali Linux endpoints in an isolated virtual network. The project demonstrates endpoint enrollment, File Integrity Monitoring (FIM), SMB authentication testing, and centralized investigation of Windows security events.

The original presentation is not included in this public repository. This repository documents the lab work and links to the available evidence.

## Project at a Glance

- **Monitoring platform:** Wazuh server, manager, indexer, and dashboard
- **Endpoints:** Windows 10 Home and Kali GNU/Linux 2026.2
- **Network:** Isolated virtual lab using pfSense for gateway and DHCP
- **Demonstrations:** File creation, modification, and deletion detection; controlled SMB authentication failures and account lockout
- **Purpose:** Show how endpoint activity becomes centralized security telemetry that can be investigated in Wazuh

## Lab Architecture

The lab used a Wazuh server on Ubuntu Server 22.04, a Windows endpoint, Kali Linux, and a pfSense gateway. Addressing details are omitted from this public overview.

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
     │  Ubuntu Server │    │ Endpoint       │    │ Endpoint       │
     │ 22.04          │    │ Wazuh Agent    │    │ Wazuh Agent    │
     │ Manager,       │    │ Test Target    │    │ Test Source    │
     │ Indexer,       │    │                │    │                │
     │ Dashboard      │    │                │    │                │
     └────────────────┘    └────────────────┘    └────────────────┘
```

[View the lab architecture evidence](evidence/architecture/Architecture.png).

## What the Lab Demonstrates

### Endpoint Enrollment

Windows and Kali agents were enrolled in Wazuh. The dashboard overview shows both agents active at the time of capture; the endpoint details screenshot provides additional Kali agent information.

- [Agent deployment notes](evidence/agent-deployment/README.md)
- [Agent overview](evidence/agent-deployment/Wazuh%20agent%20overview.png)
- [Kali endpoint details](evidence/agent-deployment/Endpoint%20agent%20details.png)

### File Integrity Monitoring

Wazuh `syscheck` monitored `C:\Users\Public\monitored`. Controlled file creation, modification, and deletion were detected and shown in the dashboard.

- [FIM technical report](docs/file-integrity-monitoring.md)
- [FIM evidence notes](evidence/fim/README.md)
- [FIM dashboard results](evidence/fim/fim-monitoring-dashboard.png)

### SMB Authentication Monitoring

Nmap confirmed SMB on TCP/445. After an initial Hydra attempt was incompatible with the target’s SMB dialect, NetExec was used to generate controlled authentication failures with the lab-created `passwords.txt` wordlist. The console output shows failed logons followed by account-lockout responses. Separately, Wazuh dashboard evidence documents 15 authentication failures, 0 successes, and related detection rules.

The account lockout was a Windows endpoint response. NetExec’s output is evidence of the authentication test; the Wazuh dashboards are the evidence of centralized monitoring and detection.

- [SMB detection report](docs/smb-bruteforce-detection.md)
- [SMB evidence index](evidence/smb-detection/README.md)
- [Nmap SMB discovery](evidence/smb-detection/nmap-smb-discovery.png)
- [NetExec setup capture](evidence/smb-detection/netexec-setup.png)
- [NetExec SMB test output](evidence/smb-detection/netexec-smb-authentication.png)
- [Wazuh authentication dashboard](evidence/smb-detection/Wazuh%20authentication%20dashboard.png)
- [Wazuh detection events](evidence/smb-detection/Wazuh%20detection%20events.png)

## Key Findings

- Wazuh reported the three controlled FIM actions: file added, modified, and deleted.
- The SMB test output showed failed logons and subsequent account-lockout responses.
- The Wazuh dashboard reported 15 authentication failures and 0 successes, with rules 60122, 60115, and 60204 associated with the activity.
- The lab illustrates the distinction between endpoint response (Windows account lockout) and SIEM detection (Wazuh event collection, rules, and dashboards).

## Scope and Next Steps

This project demonstrates host-based monitoring and SIEM investigation. It does not implement network-based intrusion detection or automated containment.

Evidence not currently included consists of a Wazuh server installation or running-status capture, separate FIM configuration and agent-log screenshots, and original Windows Event Viewer captures for Events 4625 and 4740. The reports describe these parts of the lab and identify where direct screenshots are unavailable.

Future work identified in the presentation:

- Integrate Suricata for network-based IDS coverage.
- Create a custom Wazuh detection rule.
- Configure Wazuh Active Response for an approved automated response.

## Repository Guide

- `docs/file-integrity-monitoring.md` — FIM setup, actions, results, and analysis
- `docs/smb-bruteforce-detection.md` — SMB test method, Windows telemetry, Wazuh detection, and findings
- `evidence/` — Architecture, agent deployment, FIM, and SMB screenshots with evidence notes
