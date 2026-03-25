# Incident 02 - Interface Down

## 1. Incident Summary

This incident documents a controlled interface or link outage in the lab environment to validate monitoring visibility, incident detection, and initial triage of a partial network failure.

Unlike the router-down scenario, this incident focuses on a link-level fault affecting connectivity between monitored devices while the router itself remains operational.

---

## 2. Incident Details

- **Incident ID:** INC-02
- **Title:** R1 to DSW1 Interface / Link Down
- **Severity:** Medium
- **Status:** Resolved
- **Detection Method:** PRTG sensor alert and connectivity loss
- **Affected Device(s):** R1 / DSW1 link path
- **Affected Services:** Routed or switched connectivity across the affected path
- **Environment:** Lab / simulated NOC workflow

---

## 3. Detection

The incident was detected when connectivity across the R1-to-DSW1 path was disrupted while the router itself remained reachable.

Observed indicators:
- R1 remained reachable from PRTG
- Some downstream connectivity was affected
- Interface-related monitoring behavior changed during the test
- The issue appeared narrower than a full router outage

This helped distinguish the event from Incident 01, where all R1 sensors failed due to full device unavailability.

### Detection Evidence
PRTG Dashboard
- [PRTG Healthy Dashboard View](../screenshots/prtg-dashboard-healthy.png)
- [PRTG Dashboard DSW1 Down](../screenshots/prtg-dashboard-dsw1-down.png)
- [PRTG Dashboard DSW1 Down](../screenshots/dsw1-down-alert.png)


GNS3 Topology
- [GNS3 DSW1 Interface e0/0 Down](../screenshots/gns3-topology-dsw1-interface-e0-down.png)

---

## 4. Impact Assessment

When the R1-to-DSW1 path was disrupted, the following operational impact was observed:

- R1 itself remained up and reachable
- Connectivity to devices behind the affected path may have been impacted
- Traffic across the affected interface was interrupted
- The incident appeared limited to a specific network segment or forwarding path

In a production environment, this type of issue would likely affect a subset of services or endpoints rather than the entire device.

---

## 5. Initial Triage

Initial triage focused on determining whether the problem was:

- a full router outage
- a monitoring-only issue
- or a partial path / interface failure

Checks performed:
- Verified that R1 still responded to monitoring
- Compared sensor behavior to the full router-down scenario
- Checked whether downstream connectivity changed during the event
- Correlated the issue with the controlled interface or link action in the lab

Initial conclusion:
- The device remained operational, but a specific path or interface was unavailable

---

## 6. Timeline of Events

| Time | Event |
|------|-------|
| T0 | R1 and DSW1 in normal monitored state |
| T1 | Controlled interface or link outage introduced |
| T2 | Monitoring reflected partial connectivity impact |
| T3 | Incident condition confirmed as interface/path-related |
| T4 | Interface or link restored |
| T5 | Monitoring visibility and connectivity began recovery |
| T6 | Normal state fully restored |

> Replace `T0` to `T6` with actual timestamps if recorded during testing.

---

## 7. Technical Validation

The incident was validated by confirming that the router remained operational while the affected path experienced disruption.

Validation points:
- R1 continued responding to monitoring
- The outage did not match full device-loss behavior
- Interface-related traffic or downstream reachability changed during the event
- Recovery was confirmed after the link or interface was restored

### Recovery Evidence
- [Interface Recovery Evidence](../screenshots/interface-recovery-green.png)

> Replace with your actual recovery screenshot filename.

---

## 8. Root Cause

**Root cause:** Controlled lab simulation of an interface or link outage between R1 and DSW1.

The outage was introduced intentionally to validate:
- detection of partial network failures
- differentiation between device failure and path failure
- monitoring and incident workflow for link-level issues

This was a deliberate test scenario, not an unexpected platform failure.

---

## 9. Resolution

The incident was resolved by restoring the affected interface or link in the lab environment.

Resolution steps:
1. Re-enable or reconnect the affected interface or path
2. Wait for connectivity to return
3. Allow PRTG to complete the next scan cycle
4. Confirm monitoring values return to normal
5. Verify affected connectivity is restored

Final result:
- The affected path returned to normal operation
- Monitoring visibility stabilized
- Incident condition was resolved successfully

---

## 10. Lessons Learned

This incident confirmed the following:

- A reachable router does not guarantee that all downstream paths are healthy
- Partial failures require different triage from full device outages
- Comparing multiple sensors helps distinguish scope of impact
- Link-level testing is useful for validating monitoring accuracy and operator response

---

## 11. NOC Relevance

This incident reflects a common NOC responsibility:

- identify whether an issue is device-wide or path-specific
- determine the likely scope of impact
- confirm whether the fault is isolated to an interface, segment, or uplink
- validate recovery after restoration

For an L1 NOC role, this demonstrates the ability to:
- recognize partial network failures
- avoid misclassifying the incident as a full outage
- document scope and recovery accurately

---

## 12. Related Files

- [Alert Configuration](../monitoring/alert-configuration.md)
- [Dashboard Evidence](../monitoring/dashboard-evidence.md)
- [Incident 01 - Router Down](incident-01-router-down.md)
- [Incident Summary Table](incident-summary-table.md)
- [Jira Ticket Example 02](../tickets/jira-ticket-example-02.md)
