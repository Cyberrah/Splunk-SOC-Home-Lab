# Validation and Safe Testing

## Validation sequence

1. Confirm the forwarder service is running on Windows.
2. Confirm Windows can reach the Kali host-only address on TCP 9997.
3. Search Splunk for recent events from the Windows hostname.
4. Confirm the event IDs and parsed fields needed by the detections.
5. Perform only the controlled checks below, then verify the events appeared.

## Controlled event generation

Use a disposable local Windows VM and a test account. Enable Logon auditing for both success and failure. Generate one failed interactive sign-in with an intentionally incorrect password, then stop. If you need to exercise the burst detection threshold, generate a small number of manually controlled failures against the disposable test account, with a threshold chosen to avoid triggering account lockout. Do not use scripts or tools to guess passwords, do not target a real service, and do not test against any system you do not own.

For a successful logon event, sign in normally to the disposable VM with the test account. Event 4624 and 4625 fields depend on logon type and Windows configuration. You can inspect the local Security log using Event Viewer → Windows Logs → Security.

## Suggested validation searches

Failed logons:

```spl
index=wineventlog EventCode=4625 earliest=-30m
| table _time host user Account_Name Source_Network_Address src_ip Logon_Type Failure_Reason
| sort - _time
```

Successful logons:

```spl
index=wineventlog EventCode=4624 earliest=-30m
| table _time host user Account_Name Source_Network_Address src_ip Logon_Type
| sort - _time
```

## What to record

- Test date/time and timezone
- VM name and lab IP (redact before public upload)
- Event ID and observed fields
- Search used and time range
- Whether the detection fired and why
- False positives or missing fields discovered
- Tuning change and reason

Do not describe a detection as validated unless you observed the event in Splunk and confirmed the result.
