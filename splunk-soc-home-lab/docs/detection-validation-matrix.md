# Detection validation matrix

**Status: test plan — execution pending.** These cases are derived from the six searches in this repository. They are not observed Splunk results. The separate Kestrel Windows test does not validate these exact searches.

## Before testing

Use a dedicated lab index or isolated test dataset. Record the repository commit, Splunk version, timezone, index, sourcetype, search time range and field mappings. Inspect raw events to establish which field identifies the target account and source address. The searches use `coalesce`; a populated but incorrect field can take precedence over the correct one.

Do not generate password attacks or clear a live audit log to exercise a query. Representative synthetic events or existing authorised lab records can test the search logic. Such tests validate logic only; end-to-end collection requires separate endpoint evidence.

## Cases and expected behaviour

| Search | Positive case | Negative / boundary case | Important limitation |
|---|---|---|---|
| [01 Failed-logon burst](../detections/01_failed_logon_burst.spl) | Five 4625 events for one account/source in the ten-minute lookback should return a row | Four matching failures should return no row; events outside the lookback should not count | Groups across hosts; inspect the targets field. Missing account/source values can merge into unknown |
| [02 Password spray](../detections/02_password_spray.spl) | Ten failures across five distinct accounts from one source should return a row | Nine failures across five accounts, or ten across four accounts, should return no row | A shared source address or field extraction error can distort account counts |
| [03 Failure and success](../detections/03_failure_then_success.spl) | One failure and one success for the same account/source in the same ten-minute bucket should return a row | Different accounts/sources, or different buckets, should not combine | Does not establish event order; success before failure also matches. Different hosts can combine |
| [04 Privileged logon](../detections/04_privileged_logon.spl) | A 4672 event within the 24-hour search window should appear | An unrelated event code should not appear | Review context; this search does not distinguish expected administration from abuse |
| [05 Account lockout](../detections/05_account_lockout.spl) | A 4740 event within the 24-hour search window should appear | An unrelated event code should not appear | Confirm locked account and caller fields against the raw event |
| [06 Security log cleared](../detections/06_security_log_cleared.spl) | An existing or synthetic 1102 event within seven days should appear | An unrelated event code should not appear | The search provides a review lead; it does not establish motive or authorisation |

For search 03, test events just before and after a bucket boundary as well as reversed event order. Record those limitations rather than interpreting every match as a successful compromise.

## Execution record

Copy this table for each positive and negative case. Leave results pending until tested.

| Field | Record |
|---|---|
| Case ID / detection | Pending |
| Analyst / date / timezone | Pending |
| Repository commit / Splunk version | Pending |
| Dataset origin: synthetic or endpoint-generated | Pending |
| Index / sourcetype / field mapping | Pending |
| Earliest / latest search bounds | Pending |
| Input events and expected result | Pending |
| Actual result / returned rows | Pending |
| Pass, fail or blocked / explanation | Not run |
| Evidence path / search export | Pending |
| False-positive observation / tuning | Pending |

A useful completion record includes both a matching and non-matching case, raw-event checks and reproducible search bounds. Screenshots alone should not replace the exact query and test inputs.

## Separate acceptance checks

1. **Logic:** Does the query return the expected rows for known inputs?
2. **Ingestion:** Do genuine lab endpoint events reach the intended index with correct timestamps?
3. **Scheduling:** Does a saved alert execute with the intended lookback and result threshold?
4. **Noise:** Do overlapping windows repeat the same events, and does proposed suppression preserve distinct cases?
5. **Documentation:** Can another reviewer reproduce the result from the recorded settings?

[Return to lab guide](../README.md)
