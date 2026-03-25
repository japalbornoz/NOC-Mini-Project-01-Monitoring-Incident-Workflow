# Incident Summary Table

This table provides a quick summary of the documented incident scenarios validated in this lab.

| Incident ID | Title | Type | Severity | Detection Method | Status | Summary |
|------------|-------|------|----------|------------------|--------|---------|
| INC-01 | R1 Router Down | Device Outage | High | PRTG sensor alert | Resolved | Controlled full router outage validating detection, alert visibility, and recovery confirmation. |
| INC-02 | R1 to DSW1 Interface / Link Down | Partial Path / Interface Failure | Medium | PRTG sensor alert and connectivity loss | Resolved | Controlled link failure validating distinction between device availability and downstream path failure. |

---

## Notes

- Both incidents were simulated in a lab environment for monitoring and incident workflow validation.
- Incident scenarios were selected to demonstrate both full device failure and partial network path failure.
- Recovery was confirmed through PRTG sensor state restoration and follow-up validation checks.

---

## Related Files

- [Incident 01 - Router Down](incident-01-router-down.md)
- [Incident 02 - Interface Down](incident-02-interface-down.md)
