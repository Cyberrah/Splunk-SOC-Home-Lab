# Splunk SOC Home Lab

A VirtualBox lab project for collecting Windows security events with Splunk Universal Forwarder and analysing authentication activity in Splunk Enterprise on Kali Linux.

## Published deliverables

- [Deployment guide](splunk-soc-home-lab/docs/deployment-guide.md)
- [Architecture and scope](splunk-soc-home-lab/docs/architecture-and-scope.md)
- [Forwarder and receiver configuration examples](splunk-soc-home-lab/configs)
- [Six SPL detection searches](splunk-soc-home-lab/detections)
- [Windows authentication dashboard XML](splunk-soc-home-lab/dashboards/windows_auth_overview.xml)
- [Validation and testing guide](splunk-soc-home-lab/docs/validation-and-testing.md)
- [Investigation workflow](splunk-soc-home-lab/docs/investigation-workflow.md)
- [Incident report template](splunk-soc-home-lab/docs/incident-report-template.md)

## Detection coverage

Failed-logon bursts, password-spray patterns, failures and success within the same time bucket, privileged logons, account lockouts, and security-log clearing.

The searches are starting examples. Field mappings, time windows, thresholds, and exclusions require validation against the actual lab data. A matching event is a lead for investigation, not proof of compromise.

## Evidence status

The repository currently publishes configuration, detection, dashboard, and documentation assets. Redacted screenshots, recorded test outcomes, and a completed investigation have not yet been published. The incident report is a template.

Next steps:
- Capture successful Windows event ingestion and record the lab configuration.
- Test each detection and document expected versus observed results.
- Add redacted dashboard and detection screenshots.
- Complete one investigation with a timeline, findings, false-positive considerations, and response recommendations.

[Read the full lab guide](splunk-soc-home-lab/README.md) · [Return to portfolio](https://github.com/Cyberrah/Welcome-to-my-repository)
