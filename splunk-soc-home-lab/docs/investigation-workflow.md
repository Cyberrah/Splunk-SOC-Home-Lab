# Investigation Workflow

Use this repeatable sequence when a detection search returns results. Keep conclusions proportional to the evidence.

## 1. Triage

- Record alert ID, time window, hostname, username, source address, and event count.
- Verify the raw events and confirm the fields are parsed correctly.
- Check whether the activity belongs to your own controlled test.

## 2. Scope

- Search nearby time windows for related 4624/4625 events.
- Look for additional affected users or hosts.
- Review logon type and source address when present.
- Check event 4740 for lockout context and 4672 for special privileges.
- Treat 1102 as a high-priority audit-integrity event and determine whether it was expected maintenance.

## 3. Decide and document

Assign one disposition: **Benign / Expected**, **Suspicious / Needs follow-up**, or **Confirmed malicious**. In a home lab, do not claim “confirmed malicious” based only on a threshold match. Record uncertainty and data gaps.

## 4. Contain and improve (lab only)

For the lab, stop the test, preserve screenshots or exported event details, and reset the disposable account/VM if needed. Document the detection tuning change. Do not perform remediation against real systems from this exercise.

Use `incident-report-template.md` to record the investigation.
