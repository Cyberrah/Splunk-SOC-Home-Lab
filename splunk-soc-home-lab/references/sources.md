# Product and event references

Configuration examples and event interpretations should be checked against the version of Splunk and Windows used in your lab. These official references informed this repository: 

- [Splunk Enterprise: Monitor Windows event log data](https://help.splunk.com/en/splunk-enterprise/get-started/get-data-in/9.0/get-windows-data/monitor-windows-event-log-data-with-splunk-enterprise) — Windows Event Log inputs and configuration.
- [Splunk Enterprise: inputs.conf reference](https://help.splunk.com/en/splunk-enterprise/administer/admin-manual/10.6/configuration-file-reference/10.6.0-configuration-file-reference/inputs.conf) — receiver and input configuration reference.
- [Microsoft Learn: Event 4625](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4625) — failed logon event details.
- [Microsoft Learn: Event 4624](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4624) — successful logon event details.
- [Microsoft Learn: View the Security event log](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/view-the-security-event-log) — Windows event log access.

Event schemas, field extraction, and interface labels vary across operating system and Splunk versions. Confirm locally before relying on a field or detection result.
