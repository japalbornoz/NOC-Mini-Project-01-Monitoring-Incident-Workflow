# Alert Configuration

## 1. Objective

This section documents how alert visibility was configured in PRTG to identify monitored devices or sensors entering a warning or down state.

The goal is to simulate a basic NOC alerting workflow where monitoring events become visible operational issues requiring investigation.

---

## 2. Alerting Approach

PRTG generates alerts based on sensor state changes. In this lab, alert visibility relied on built-in sensor status monitoring within the PRTG web console.

Sensor state interpretation:

- Up (green) → normal operation
- Warning (yellow) → non-critical issue or threshold condition
- Down (red) → device or monitored metric unavailable

For this lab, alerting focused primarily on:
- device unreachability
- sensor down state
- visible alarm conditions in the monitoring console

---

## 3. Notification / Alert Configuration

Alerting was configured using built-in PRTG alarm visibility and sensor state changes in the web console.

Configured behavior:
- Sensors generate visible alarms when they enter a Down state
- Warning and Down states are visible in the device tree and alarm view
- The PRTG web console acts as the primary alert display for this lab

Current lab status:
- No email alerting configured yet
- No SMS alerting configured yet
- No ticketing integration configured yet

---

## 4. Trigger Conditions

The following conditions were used to generate alerts:

- Ping sensor failure
- Ping v2 sensor failure
- SNMP sensor failure
- Device or monitored path becoming unreachable

Typical examples:
- If R1 becomes unreachable, related sensors change to Down state
- If DSW1 stops responding, PRTG marks the affected sensors as Down
- If SNMP polling fails, SNMP-based sensors generate an alert condition

---

## 5. Alert Severity Interpretation

PRTG uses sensor states to indicate the severity of an event:

- Green (Up): normal status
- Yellow (Warning): non-critical issue or threshold condition
- Red (Down): critical issue requiring investigation

For this project, the most important operational state was:
- Red (Down), because it represents a likely incident requiring NOC attention

---

## 6. Alert Validation

Alerting was validated by simulating a controlled outage on R1 and observing sensor state changes in PRTG.

Validation steps:
1. Confirm R1 sensors are in Up state
2. Stop R1 in the lab environment
3. Wait for the next PRTG scan cycle
4. Confirm affected R1 sensors change from Up to Down
5. Start R1 again
6. Wait for the next PRTG scan cycle
7. Confirm affected sensors return to Up

Expected result:
- PRTG displays an alarm for the affected R1 sensors during the outage
- The alarm clears after connectivity is restored

### Evidence

**Failure simulation**
- [GNS3 R1 Stop State](../screenshots/gns3-topology-r1-down.png)
- [PRTG R1 Down Alert](../screenshots/r1-down-alert.png)

**Recovery sequence**
- [R1 Recovery In Progress](../screenshots/r1-recovery-green-part1.png)
- [R1 Recovery Partial Green](../screenshots/r1-recovery-green-part2.png)
- [R1 Recovery Fully Restored](../screenshots/r1-recovery-green-part3.png)

---

## 7. Operational Considerations

Effective alerting depends on accurate monitoring configuration.

Common causes of invalid or misleading alerts include:
- Incorrect IP addressing
- Routing issues
- SNMP misconfiguration
- Excessive or unnecessary sensors
- Monitoring disconnected interfaces

To reduce false positives, only relevant and validated sensors were used in this lab.

---

## 8. NOC Relevance

In a real NOC environment, alerts are the starting point for incident handling.

Typical workflow:
1. Monitoring sensor changes state
2. Alert appears in the monitoring platform
3. Operator reviews the affected device and metric
4. Initial troubleshooting begins
5. Incident is logged or escalated if needed

This lab reflects the first stage of that process by showing how sensor failures become visible operational events.

---

## 9. Planned Enhancements

The following alerting capabilities are planned for future improvement:

- Email-based alert notification
- Integration with Jira Service Management for incident tracking
- SMS or mobile-based notification for critical alerts

These enhancements will extend the lab from basic monitoring visibility to a more complete incident management workflow.
