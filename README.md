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

## Validation plan

[Use the six-detection validation matrix](splunk-soc-home-lab/docs/detection-validation-matrix.md) for positive, negative and boundary cases, known query limitations and an execution-record template. New test results remain pending.

## Detection coverage

Failed-logon bursts, password-spray patterns, failures and success within the same time bucket, privileged logons, account lockouts, and security-log clearing.

The searches are starting examples. Field mappings, time windows, thresholds, and exclusions require validation against the actual lab data. A matching event is a lead for investigation, not proof of compromise.

## Evidence status

This repository provides reusable Windows-focused configuration, detection, dashboard and documentation assets. Its incident report remains a template.

The related **[Splunk and Suricata Telemetry Pipeline](https://github.com/Cyberrah/Log-Analysis-Pipeline-Ingesting-and-Analysing-Security-Telemetry-with-Splunk-Enterprise-and-Suricata)** now contains the completed Kestrel lab investigation:

- [Final SOC report](https://github.com/Cyberrah/Log-Analysis-Pipeline-Ingesting-and-Analysing-Security-Telemetry-with-Splunk-Enterprise-and-Suricata/blob/main/reports/Kestrel_SOC_Investigation_Report.pdf)
- [Windows scheduled-alert evidence](https://github.com/Cyberrah/Log-Analysis-Pipeline-Ingesting-and-Analysing-Security-Telemetry-with-Splunk-Enterprise-and-Suricata/blob/main/screenshots/windows-triggered-alerts.png)
- [Dashboard screenshot](https://github.com/Cyberrah/Log-Analysis-Pipeline-Ingesting-and-Analysing-Security-Telemetry-with-Splunk-Enterprise-and-Suricata/blob/main/screenshots/soc-dashboard.png)
- [SSH host/network correlation timeline](https://github.com/Cyberrah/Log-Analysis-Pipeline-Ingesting-and-Analysing-Security-Telemetry-with-Splunk-Enterprise-and-Suricata/blob/main/docs/ssh-investigation.md)
- [Log source inventory](https://github.com/Cyberrah/Log-Analysis-Pipeline-Ingesting-and-Analysing-Security-Telemetry-with-Splunk-Enterprise-and-Suricata/blob/main/inventory/Kestrel_Log_Source_Inventory.xlsx)

That project uses `windows`, `linux` and `suricata` indexes; this starter lab uses `wineventlog`. Its Windows detection also uses explicit XML field extraction. Use the settings and searches appropriate to each project. The Kestrel evidence does not establish that all six examples in this repository have been tested.

Next steps for these starter assets:

- Validate each detection against local events and record expected versus observed results.
- Record field mappings, thresholds, exclusions and false positives.
- Keep the dashboard XML here distinct from the Kestrel dashboard, whose export is not yet published.

[Read the full lab guide](splunk-soc-home-lab/README.md) · [Return to portfolio](https://github.com/Cyberrah/Welcome-to-my-repository)
