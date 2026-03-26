# Jira Ticket Example 01 - R1 Router Down

## 1. Ticket Purpose

This file shows an example Jira Service Management incident ticket based on **Incident 01 - Router Down** from the lab.

This is a manually documented ticket example used to demonstrate how a monitored alert can be translated into an incident record for operational tracking.

---

## 2. Ticket Summary

- **Ticket Type:** Incident
- **Reference Incident:** INC-01
- **Title:** R1 Router Down
- **Priority:** High
- **Status:** Resolved
- **Environment:** Lab / simulated NOC workflow
- **Detection Source:** PRTG sensor alert

---

## 3. Affected Service / Component

- **Affected Device:** R1
- **Affected Service:** Routed connectivity between monitoring host and internal lab network
- **Impact Summary:** Loss of monitoring visibility and routed access through the edge router path

---

## 4. Description

PRTG generated an alert indicating that R1 became unreachable.

Multiple R1-related sensors changed from **Up** to **Down**, including:

- Ping
- Ping v2
- SNMP System Uptime
- SNMP Traffic (TO_DSW1)

The alert suggested full device unavailability rather than a single-sensor failure.

---

## 5. Symptoms Observed

- R1 unreachable in PRTG
- Multiple R1 sensors in Down state
- Monitoring visibility to the routed internal path affected
- Event matched a full router outage condition

---

## 6. Initial Triage Notes

Initial checks performed:

- Reviewed affected R1 sensors in PRTG
- Confirmed multiple monitoring points failed simultaneously
- Compared behavior against normal healthy-state monitoring
- Correlated the event with the controlled lab outage condition

Initial assessment:
- Likely full device outage
- Not limited to a single protocol or individual sensor failure

---

## 7. Actions Taken

- Confirmed outage visibility in PRTG
- Verified that R1 had been intentionally stopped in the lab for testing
- Monitored sensor state changes during the outage window
- Restored R1 in GNS3
- Waited for the next PRTG scan cycle
- Confirmed affected sensors returned to Up state

---

## 8. Resolution Notes

R1 was restored in the lab environment and monitoring visibility returned to normal.

All affected R1 sensors recovered successfully after the router was started again and PRTG completed the next scan cycle.

---

## 9. Closure Summary

- **Root Cause:** Controlled lab simulation of router outage
- **Resolution:** R1 restarted and monitoring restored
- **Final Status:** Resolved
- **Closure Note:** Incident successfully validated alert generation, outage visibility, and recovery confirmation workflow

---

## 10. Suggested Jira Fields Mapping

| Jira Field | Example Value |
|-----------|---------------|
| Issue Type | Incident |
| Summary | R1 Router Down |
| Priority | High |
| Status | Resolved |
| Source | PRTG |
| Affected CI | R1 |
| Environment | Lab |
| Resolution | Router restored |
| Root Cause | Controlled outage simulation |

---

## 11. Related Files

- [Incident 01 - Router Down](../incidents/incident-01-router-down.md)
- [Alert Configuration](../monitoring/alert-configuration.md)
- [Dashboard Evidence](../monitoring/dashboard-evidence.md)
