# Jira Ticket Example 02 - DSW1 Interface / Path Failure Alert

## 1. Objective

This file documents a real Jira Service Management request automatically created from PRTG during the controlled interface / path failure validated in Incident 02.

The purpose of this example is to show that a partial downstream failure can generate a valid ticket even when the upstream router remains reachable.

---

## 2. Scenario Summary

This ticket example is based on the controlled failure between **R1** and **DSW1** in the lab.

In this scenario:

- **R1 remained up and reachable**
- **DSW1 lost reachability from the monitoring system**
- **PRTG DSW1 Ping v2** was used as the primary ticket trigger
- Jira created a real request representing the downstream failure

This was a **partial path / interface failure**, not a full router outage.

### Fault Introduced

The failure was intentionally created by shutting down the DSW1 uplink toward R1:

```cisco
DSW1(config)#interface e0/0
DSW1(config-if)#shutdown
DSW1(config-if)#
*Mar 28 12:19:15.568: %LINK-5-CHANGED: Interface Ethernet0/0, changed state to administratively down
*Mar 28 12:19:16.573: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/0, changed state to down
```

---

## 3. Ticket Identification

### Jira Request Details

- **Ticket Key:** `NOC01-11`
- **Summary:** `[PRTG] DSW1 Ping v2 (Ping v2) - Down (Error caused by lookup value 'Unreachable' in channel 'Status' (The reply timed out.))`
- **Request Type:** Emailed request
- **Status:** Waiting for support
- **Priority:** Medium
- **Reporter:** Jap Albornoz
- **Queue:** PRTG Network Monitoring

This request was created automatically from the PRTG email notification pipeline validated in the lab.

---

## 4. Trigger Source

### Primary Alerting Sensor

The primary sensor used for this ticket example was:

- **Device:** DSW1
- **Sensor:** Ping v2

This sensor was chosen because it provided the cleanest primary indication that the downstream switch had become unreachable from PRTG during the interface failure.

### Correlated Supporting Sensors

Other DSW1 sensors also showed impact during the fault, including:

- DSW1 Ping
- DSW1 SNMP System Uptime

These were treated as **supporting correlated indicators**, not separate ticket sources.

This is important because one outage can affect multiple sensors, but a cleaner incident workflow uses **one primary trigger** and treats the remaining sensors as supporting evidence.

---

## 5. Actual Alert-to-Ticket Workflow

The validated workflow was:

1. **Healthy baseline confirmed**
   - R1 and DSW1 sensors were verified in normal state before testing

2. **Notification trigger already configured**
   - DSW1 Ping v2 was configured to send the `Jira - PRTG Alert Intake` notification when Down for at least `60 seconds`

3. **Controlled fault introduced**
   - DSW1 `Ethernet0/0` was administratively shut down
   - This broke the path between R1 and DSW1 while keeping both nodes powered on

4. **PRTG detected the downstream failure**
   - R1 remained reachable
   - DSW1 Ping v2 transitioned to Down
   - supporting DSW1 sensors also showed impact

5. **PRTG sent the notification**
   - The SMTP relay configuration successfully sent the alert email
   - PRTG logs confirmed successful notification delivery

6. **Jira created the request**
   - Jira Service Management received the email
   - a real request was created automatically as **NOC01-11**

7. **Ticket became available for triage**
   - The new request appeared in the Jira queue for review and response

This validated the actual monitoring-to-ticket workflow for a partial path failure.

---

## 6. Subject and Body Mapping

### PRTG Email Subject

The generated email subject created the Jira ticket summary.

Example:

```text
[PRTG] DSW1 Ping v2 (Ping v2) - Down (Error caused by lookup value 'Unreachable' in channel 'Status' (The reply timed out.))
```

### PRTG Custom Body

The email body was mapped into the Jira ticket description.

Validated fields included:

```text
Source: PRTG
Site: PRTG Network Monitor (JAPNYTE)
Device: DSW1
Sensor: Ping v2 (Ping v2)
Status: Down
Down:
Message: Error caused by lookup value 'Unreachable' in channel 'Status' (The reply timed out.)
Environment: Lab
```

This confirms that the subject/body mapping from PRTG to Jira worked as intended for the DSW1 path failure scenario.

---

## 7. Evidence

### GNS3 Evidence
- [GNS3 Topology - DSW1 e0/0 Administratively Down](../screenshots/gns3-dsw1-down.png)

### PRTG Evidence
- [PRTG Healthy Baseline - R1 and DSW1 Reachable](../screenshots/prtg-overview-dsw1-up.png)
- [PRTG Overview - R1 Up While DSW1 Sensors Show Failure](../screenshots/prtg-overview-dsw1-sensor-ping-v2-down.png)
- [PRTG DSW1 Ping v2 Sensor - Down / Unreachable](../screenshots/prtg-dsw1-sensor-ping-v2-down.png)

### Jira Evidence
- [Jira Queue - DSW1 Alert Ticket Created](../screenshots/jira-queue-showing-dsw1-notification.png)
- [Jira Ticket Detail - NOC01-11](../screenshots/jira-dsw1-ticket-detail-noc01-11.png)

---

## 8. Operational Value

This ticket example demonstrates that:

- a downstream failure can be ticketed even when the upstream router remains reachable
- alert-to-ticket workflows are still useful for partial path outages
- a single primary sensor can be used to represent the incident cleanly
- correlated sensor impact can support triage without creating unnecessary duplicate ticket examples

For a NOC workflow, this is valuable because not all incidents are full device outages. Operators must determine the actual scope of impact and avoid misclassifying partial failures.

---

## 9. Troubleshooting Lessons Learned

This validation reinforced the following lessons:

- using **one primary trigger sensor** results in cleaner incident representation
- `DSW1 Ping v2` was the best primary trigger for this scenario
- correlated sensors should support the incident, not create duplicate examples
- shutting down the interface was a cleaner and more accurate test than stopping the switch node
- keeping **R1 reachable** made it easier to distinguish a path failure from a full router outage
- SMTP relay validation should be confirmed separately before relying on automated ticket creation

---

## 10. Related Files

- [Incident 02 - Interface Down](../incidents/incident-02-interface-down.md)
- [Alert Configuration](../monitoring/alert-configuration.md)
- [Dashboard Evidence](../monitoring/dashboard-evidence.md)
- [Jira Ticket Example 01](jira-ticket-example-01.md)
- [Escalation Workflow](escalation-workflow.md)
