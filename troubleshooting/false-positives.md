# False Positives

## 1. Purpose

This document explains how false positives, duplicate alerts, and noisy incident signals were identified and controlled in this lab.

The goal is to show that not every affected sensor should automatically be treated as a separate incident or ticket.

---

## 2. Objective

The objective of this note is to document:

- what false positives and alert noise looked like in this environment
- why one outage could affect multiple sensors at the same time
- how primary alerting sensors were selected
- how supporting sensors were used for correlation instead of duplicate ticket creation

---

## 3. What False Positives Mean in This Lab

In this lab, a false positive did not necessarily mean that PRTG was wrong.  
Instead, it often meant that the monitoring system was showing multiple symptoms of the same root issue.

Examples include:
- multiple sensors on the same device turning red during one outage
- downstream devices appearing failed because the upstream path was broken
- multiple affected sensors creating the risk of duplicate Jira tickets for one incident

This means that raw alert count alone could be misleading if the operator did not first determine the actual fault domain.

---

## 4. Sources of Alert Noise

The main sources of alert noise in this lab were:

- multiple sensors attached to the same monitored device
- one upstream outage affecting several downstream checks
- device-wide outages causing Ping, Ping v2, SNMP, and traffic-related sensors to fail together
- path or interface failures causing one device to remain reachable while another became unreachable

These conditions did not represent multiple separate incidents.  
They represented multiple monitoring symptoms of a single network event.

---

## 5. Primary Sensor Strategy

To reduce duplicate incident handling, one primary sensor was selected for each validated scenario.

The primary sensor was the one used to represent the incident in the alert-to-ticket workflow.

Other affected sensors were treated as supporting evidence only.

### Primary Sensor Selection Used in This Lab

- **Incident 01 – R1 Router Down**
  - primary sensor: `R1 Ping v2`

- **Incident 02 – R1 to DSW1 Interface / Link Down**
  - primary sensor: `DSW1 Ping v2`

This approach reduced alert noise and kept the Jira workflow cleaner.

---

## 6. Incident 01 Example - Router Outage

During the controlled R1 outage, multiple R1 sensors were affected at the same time, including:

- Ping
- Ping v2
- SNMP System Uptime
- TO_DSW1 Traffic

Although several sensors showed failure, they all pointed to the same root event:
- **R1 became unavailable**

For ticketing purposes, only **R1 Ping v2** was used as the primary trigger.

The other affected sensors were used to support triage and confirm that the outage was device-wide rather than limited to one protocol or metric.

---

## 7. Incident 02 Example - Path / Interface Failure

During the controlled interface failure between R1 and DSW1:

- **R1 remained reachable**
- **DSW1 became unreachable**
- DSW1-related sensors showed impact

Affected DSW1 sensors included:
- Ping
- Ping v2
- SNMP System Uptime

For ticketing purposes, only **DSW1 Ping v2** was used as the primary trigger.

This prevented the same interface/path failure from being treated as multiple separate incidents.

---

## 5. Primary Sensor Strategy

To reduce duplicate incident handling, one primary sensor was selected for each validated scenario.

The primary sensor was the one used to represent the incident in the alert-to-ticket workflow.

Other affected sensors were treated as supporting evidence only.

### Primary Sensor Selection Used in This Lab

- **Incident 01 – R1 Router Down**
  - primary sensor: `R1 Ping v2`

- **Incident 02 – R1 to DSW1 Interface / Link Down**
  - primary sensor: `DSW1 Ping v2`

This approach reduced alert noise and kept the Jira workflow cleaner.

---

## 6. Incident 01 Example - Router Outage

During the controlled R1 outage, multiple R1 sensors were affected at the same time, including:

- Ping
- Ping v2
- SNMP System Uptime
- TO_DSW1 Traffic

Although several sensors showed failure, they all pointed to the same root event:
- **R1 became unavailable**

For ticketing purposes, only **R1 Ping v2** was used as the primary trigger.

The other affected sensors were used to support triage and confirm that the outage was device-wide rather than limited to one protocol or metric.

---

## 7. Incident 02 Example - Path / Interface Failure

During the controlled interface failure between R1 and DSW1:

- **R1 remained reachable**
- **DSW1 became unreachable**
- DSW1-related sensors showed impact

Affected DSW1 sensors included:
- Ping
- Ping v2
- SNMP System Uptime

For ticketing purposes, only **DSW1 Ping v2** was used as the primary trigger.

This prevented the same interface/path failure from being treated as multiple separate incidents.

---

## 8. Why Duplicate Tickets Were Avoided

If every affected sensor had been allowed to generate its own Jira request, a single outage could have created multiple tickets for the same root issue.

Examples of this risk in the lab included:

- one router outage affecting several R1 sensors at the same time
- one downstream path failure affecting multiple DSW1 checks
- supporting sensors adding confirmation value, but not representing separate incidents

To avoid this, only one primary sensor per scenario was configured for ticket creation.

This made the workflow:

- easier to understand
- easier to validate
- closer to how real NOC teams reduce duplicate incident noise

---

## 9. Operational Value

Controlling false positives and duplicate tickets is important because:

- raw sensor count does not always equal incident count
- one fault can create multiple monitoring symptoms
- incident handling becomes inefficient if the same event is opened several times
- operators need correlation, not just alert volume

This lab demonstrated that a cleaner workflow is achieved when:
- one sensor is used as the primary trigger
- other affected sensors are treated as supporting evidence
- the operator classifies the fault domain before escalating or documenting the incident

---

## 10. Related Files

- [Monitoring Runbook](monitoring-runbook.md)
- [Connectivity Host to Lab](connectivity-host-to-lab.md)
- [Platform Stability](platform-stability.md)
- [Incident 01 - Router Down](../incidents/incident-01-router-down.md)
- [Incident 02 - Interface Down](../incidents/incident-02-interface-down.md)
- [Jira Ticket Example 01](../tickets/jira-ticket-example-01.md)
- [Jira Ticket Example 02](../tickets/jira-ticket-example-02.md)
