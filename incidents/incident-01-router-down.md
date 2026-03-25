# Incident 01 - Router Down

## 1. Incident Summary

This incident documents a controlled router outage in the lab environment to validate monitoring, alert visibility, and initial incident response workflow.

The affected device was **R1**, which acts as the edge router and gateway between the monitoring host network and the internal lab network.

---

## 2. Incident Details

- **Incident ID:** INC-01
- **Title:** R1 Router Down
- **Severity:** High
- **Status:** Resolved
- **Detection Method:** PRTG sensor alert
- **Affected Device:** R1
- **Affected Services:** Device reachability, routed access to internal monitored network
- **Environment:** Lab / simulated NOC workflow

---

## 3. Detection

The incident was detected through PRTG when multiple R1-related sensors changed from **Up** to **Down**.

Affected sensors included:
- Ping
- Ping v2
- SNMP System Uptime
- SNMP Traffic (TO_DSW1)

This indicated loss of reachability to the router and confirmed that the event was not isolated to a single metric.

### Detection Evidence
- [PRTG R1 Down Alert](../screenshots/r1-down-alert.png)
- [GNS3 R1 Stop State](../screenshots/gns3-topology-r1-down.png)

---

## 4. Impact Assessment

When R1 became unavailable, the following operational impact was observed:

- PRTG lost visibility to R1 sensors
- The routed path between the monitoring host and the internal lab network was affected
- Monitoring of downstream internal connectivity depended on restoration of the router path

In a production environment, this type of incident would represent a significant service-impacting event because the router is a critical forwarding device.

---

## 5. Initial Triage

Initial triage focused on determining whether the issue was:

- a sensor-only problem
- an SNMP-only failure
- or a full device/path outage

Checks performed:
- Reviewed PRTG sensor state for R1
- Confirmed multiple sensors failed at the same time
- Correlated the event with the lab topology state in GNS3
- Verified that the outage was caused by a controlled stop of R1

Initial conclusion:
- This was a full device unavailability event, not an isolated monitoring protocol failure

---

## 6. Timeline of Events

| Time | Event |
|------|-------|
| T0 | R1 in normal Up state |
| T1 | Controlled outage introduced by stopping R1 in GNS3 |
| T2 | PRTG sensors for R1 changed to Down |
| T3 | Incident condition confirmed in monitoring console |
| T4 | R1 started again in GNS3 |
| T5 | PRTG sensors began recovery |
| T6 | All monitored R1 sensors returned to Up |

---

## 7. Technical Validation

The incident was validated using both the monitoring platform and the lab topology state.

Validation points:
- R1 sensor failures appeared in PRTG
- Multiple sensors failed simultaneously, confirming broad loss of reachability
- Recovery was confirmed after R1 was restored
- Sensor status returned from Down to Up after the next scan cycle

### Recovery Evidence
- [R1 Recovery In Progress](../screenshots/r1-recovery-green-part1.png)
- [R1 Recovery Partial Green](../screenshots/r1-recovery-green-part2.png)
- [R1 Recovery Fully Restored](../screenshots/r1-recovery-green-part3.png)

---

## 8. Root Cause

**Root cause:** Controlled lab simulation of router unavailability.

R1 was intentionally stopped in GNS3 to validate:
- PRTG alert generation
- operator visibility of the failure
- incident detection workflow
- recovery confirmation process

This was not caused by misconfiguration, packet loss, or unstable routing. It was a deliberate outage introduced for testing.

---

## 9. Resolution

The incident was resolved by restoring R1 in the lab environment.

Resolution steps:
1. Start R1 in GNS3
2. Wait for device boot and protocol readiness
3. Allow PRTG to complete the next scan cycle
4. Confirm affected sensors return to Up
5. Verify monitoring visibility is restored

Final result:
- R1 returned to normal monitored state
- Related sensors cleared automatically in PRTG
- Incident condition was resolved successfully

---

## 10. Lessons Learned

This incident confirmed the following:

- Multiple sensor failures on the same device usually indicate a broader outage
- PRTG provides fast visibility into device unreachability
- Controlled outage testing is useful for validating the monitoring workflow
- Recovery validation is just as important as failure detection

---

## 11. NOC Relevance

This incident reflects a common NOC responsibility:

- detect device outage
- identify affected monitoring points
- confirm whether the issue is isolated or broad
- track restoration and recovery state

For an L1 NOC role, this demonstrates the ability to:
- recognize a critical monitoring event
- perform basic triage
- document operational impact
- confirm service recovery

---

## 12. Related Files

- [Alert Configuration](../monitoring/alert-configuration.md)
- [Dashboard Evidence](../monitoring/dashboard-evidence.md)
- [Incident Summary Table](incident-summary-table.md)
- [Jira Ticket Example 01](../tickets/jira-ticket-example-01.md)
