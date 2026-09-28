# SMB Detection Evidence

This directory contains sanitized evidence from the controlled SMB authentication testing scenario performed in the isolated Wazuh laboratory.

## Evidence

- [Nmap SMB discovery](nmap-smb-discovery.png): Documents the identification of SMB on TCP/445.
- [Wazuh authentication dashboard](Wazuh%20authentication%20dashboard.png): Shows the recorded authentication failures and successes.
- [Wazuh detection events](Wazuh%20detection%20events.png): Shows the relevant Wazuh detection and correlation rules.

## Security and Evidence Notes

- Screenshots intended for public release must be reviewed and sanitized to remove environment-specific IP addresses, hostnames, and other unnecessary identifying details.
- Sanitized screenshots preserve the observed evidence; they do not substitute fictional results for original observations.
- Windows Event IDs 4625 and 4740 are documented in the technical report. Original Event Viewer screenshots for these events are not available.
- Raw authentication-testing output and the original presentation are not included in this directory.

## Scope

The evidence supports documentation of the reconnaissance, authentication-testing, telemetry, detection, and investigation stages of the controlled laboratory exercise.
