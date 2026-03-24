# Alert Configuration

## 1. Objective

This section documents how alerting was configured in PRTG to notify the operator when monitored devices or sensors enter a warning or down state.

The goal is to simulate a basic NOC alerting workflow where monitoring events generate actionable incidents.

---

## 2. Alerting Approach

Alerts in PRTG are triggered when a sensor changes state, such as:

- Up → normal operation
- Warning → threshold exceeded
- Down → device or service unavailable

For this lab, alerting focused primarily on:
- device unreachability
- sensor down state
- basic incident visibility from the monitoring console

---

## 3. Notification / Alert Configuration

Alerting was configured within PRTG using built-in notification and sensor status logic.

Configured behavior:
- Sensors generate visible alarms when they enter a Down state
- Warning and Down states are visible from the device tree and alarm view
- The monitoring console acts as the primary alert display for this lab

If email, push, or other notification methods were not configured, state that clearly:
- No external notification channel (email/SMS) was configured in this lab
- Alerts were reviewed directly in the PRTG web console

---

## 4. Trigger Conditions

The following conditions were used to generate alerts:

- Ping sensor failure
- Ping v2 sensor failure
- SNMP sensor failure
- Device or interface becoming unreachable

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

Alerting was validated by simulating a device or path failure and observing sensor state changes in PRTG.

Validation steps:
1. Identify a monitored target device
2. Introduce a controlled failure (for example, shut down an interface or disconnect a path)
3. Observe the affected sensor state in PRTG
4. Confirm the sensor changes from Up to Down
5. Restore connectivity and confirm the sensor returns to Up

Expected result:
- PRTG displays an alarm for the affected sensor
- The alarm clears after service is restored

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

The workflow is typically:
1. Monitoring sensor changes state
2. Alert appears in the monitoring platform
3. Operator reviews affected device and metric
4. Initial troubleshooting begins
5. Incident is logged or escalated if needed

This lab reflects the first stage of that process by showing how sensor failures become visible operational events.

See evidence: `monitoring/images/r1-down-alert.png`
See evidence: `monitoring/images/r1-recovery-green.png`
