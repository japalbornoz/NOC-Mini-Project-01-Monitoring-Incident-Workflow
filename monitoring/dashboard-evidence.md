# Dashboard Evidence

## 1. Objective

This section documents the dashboard and monitoring views used in PRTG to provide operational visibility into device health, sensor status, and alert conditions.

The goal is to show how the monitoring platform presents actionable information to the operator during normal conditions and during incidents.

---

## 2. Dashboard Purpose

In a NOC environment, the dashboard is used to quickly identify:

- which devices are healthy
- which sensors are in warning or down state
- where immediate investigation is required

For this lab, the PRTG dashboard served as the primary monitoring view for:

- device availability
- sensor health
- active alarms
- recovery confirmation after an outage

---

## 3. Dashboard Views Used

The following PRTG views were used as primary operational references:

### Device Tree View
Used to identify:
- monitored devices
- sensor status per device
- current health state using color indicators

Operational value:
- allows quick identification of affected devices
- helps determine whether the issue is isolated or widespread

### Sensor Detail View
Used to identify:
- individual sensor health
- response time values
- uptime data
- interface traffic activity

Operational value:
- helps confirm which metric has failed
- provides more detail for initial troubleshooting

### Alarm View
Used to identify:
- sensors currently in warning or down state
- active monitoring issues requiring attention

Operational value:
- provides a focused list of current issues
- reduces time spent searching through healthy devices

---

## 4. Normal Operating State

Under normal conditions, the dashboard showed:

- all monitored devices in Up state
- all validated sensors reporting normally
- no active alarms
- stable visibility across R1, DSW1, PC1, and SRV1

This represents the expected monitoring baseline for the lab.

### Evidence
- [PRTG Healthy Dashboard View](../screenshots/prtg-dashboard-healthy.png)
- [PRTG R1 Sensor Overview](../screenshots/prtg-r1-sensors-healthy.png)

---

## 5. Incident / Alert State Visibility

When a controlled outage was introduced on R1, the dashboard reflected the failure through visible sensor state changes.

Observed behavior:
- affected R1 sensors changed from Up to Down
- alarm visibility appeared in the monitoring console
- the affected device became easy to identify from the dashboard view

This demonstrates how the dashboard supports rapid detection of operational issues.

### Evidence
- [PRTG R1 Down Alert View](../screenshots/r1-down-alert.png)
- [GNS3 R1 Stop State](../screenshots/gns3-topology-r1-down.png)

---

## 6. Recovery Visibility

After R1 was restored, the dashboard reflected the recovery process as sensors returned to normal.

Observed behavior:
- sensors transitioned from Down back to Up
- alarm condition cleared
- normal monitoring visibility resumed

This confirms that the dashboard can be used not only for detection, but also for recovery validation.

### Evidence
- [R1 Recovery In Progress](../screenshots/r1-recovery-green-part1.png)
- [R1 Recovery Partial Green](../screenshots/r1-recovery-green-part2.png)
- [R1 Recovery Fully Restored](../screenshots/r1-recovery-green-part3.png)

---

## 7. Key Operational Value

The dashboard improved operational awareness by providing:

- centralized visibility into monitored devices
- immediate identification of failed sensors
- confirmation of service recovery after restoration
- a simple visual starting point for incident triage

For a Level 1 NOC role, this is important because the dashboard is often the first place an operator checks when an alert occurs.

---

## 8. Limitations of the Dashboard in This Lab

The dashboard in this lab environment has the following limitations:

- no custom map or advanced visualization was created
- no email or SMS alert widgets were integrated
- no ticketing panel or Jira integration was displayed
- visibility is limited to the PRTG console only

These limitations are acceptable for a foundational monitoring and incident workflow lab.

---

## 9. NOC Relevance

In a real NOC environment, dashboards are used to:

- maintain situational awareness
- prioritize active issues
- validate whether recovery actions were successful
- support escalation with visual evidence

This lab demonstrates those same core concepts using PRTG as the monitoring platform.

---

## 10. Planned Improvements

Future dashboard improvements may include:

- custom dashboard or map views
- additional traffic and performance sensors
- integration with ticketing workflows
- richer alert visualization for incident tracking
