# Jira Ticket Example 01 - Real PRTG Alert Intake for R1 Router Down

## 1. Objective

This file documents a real Jira Service Management ticket created automatically from a PRTG alert during a controlled R1 outage in the lab.

Unlike a manually written example, this ticket was generated through the validated monitoring-to-ticketing workflow configured for this project.

---

## 2. Scenario Summary

A controlled outage was introduced by stopping R1 in the GNS3 topology.

PRTG detected the outage through the `Ping v2` sensor assigned to R1.  
After the configured trigger interval was met, PRTG sent an email notification through the configured Gmail SMTP relay.  
Jira Service Management received the email through the project intake address and created a new service request automatically.

This validated the complete alert-to-ticket workflow.

---

## 3. Ticket Identification

- **Jira Project:** NOC Mini Project 01 - Monitoring & Incidents
- **Queue:** From PRTG
- **Ticket Key:** `NOC01-10`
- **Summary:** `[PRTG] R1 Ping v2 (Ping v2) - Down (Error caused by lookup value 'Unreachable' in channel 'Status' (The reply timed out.))`
- **Request Type:** Emailed request
- **Status:** Waiting for support
- **Reporter:** Jap Albornoz
- **Priority:** Highest
- **Environment:** Lab

> Ticket key may vary if the workflow is retested and a newer request is generated.

---

## 4. Trigger Source

### Primary Alerting Sensor
- **Device:** R1
- **Sensor:** Ping v2
- **Condition:** Sensor state = Down for at least 60 seconds
- **Notification Template:** Jira - PRTG Alert Intake

### Why Ping v2 Was Used
`Ping v2` was selected as the primary alerting sensor for this workflow because it provides a clean availability signal for router reachability loss.

Other R1 sensors such as:
- Ping
- SNMP System Uptime
- (002) TO_DSW1 Traffic

were treated as supporting sensors for the same outage rather than separate ticket sources.

This kept the workflow cleaner and avoided duplicate Jira tickets for one root incident.

---

## 5. Actual Alert-to-Ticket Workflow

The validated workflow was:

1. R1 was stopped in GNS3
2. PRTG detected the R1 `Ping v2` sensor state change to **Down**
3. PRTG state trigger waited for the configured threshold
4. PRTG sent an email using the configured Gmail SMTP relay
5. Jira Service Management intake email received the alert
6. Jira created a new request automatically in the **From PRTG** queue
7. Ticket details reflected the outage information sent by PRTG

---

## 6. Subject and Body Mapping

### PRTG Email Subject
The generated email subject created the Jira ticket summary.

Example:

```text
[PRTG] R1 Ping v2 (Ping v2) - Down (Error caused by lookup value 'Unreachable' in channel 'Status' (The reply timed out.))
```

### PRTG Custom Body
The email body was mapped into the Jira ticket description.

Validated fields included:

```text
Source: PRTG
Site: PRTG Network Monitor (JAPNYTE)
Device: R1
Sensor: Ping v2 (Ping v2)
Status: Down
Down:
Message: Error caused by lookup value 'Unreachable' in channel 'Status' (The reply timed out.)
Environment: Lab
```
This confirms that both the alert summary and supporting incident context were transferred successfully into Jira.

---

## 7. Evidence

### PRTG Detection
- [PRTG R1 Ping v2 Sensor Down](../screenshots/prtg-r1-sensor-ping-v2-down.png)
- [PRTG Overview - R1 Down](../screenshots/prtg-overview-r1-down.png)

### Trigger and Delivery Configuration
- [PRTG Notification Trigger - R1 Ping v2](../screenshots/prtg-notification-trigger-r1-ping-v2.png)
- [PRTG SMTP Relay Settings](../screenshots/prtg-notification-delivery-smtp-relay-settings.png)

### Delivery Confirmation
- [PRTG Log - Notification Sent Successfully](../screenshots/prtg-log-entries-r1-notification-sent.png)

### Source Outage in GNS3
- [GNS3 R1 Down State](../screenshots/gns3-r1-down.png)

### Jira Ticket Creation
- [Jira Queue Showing R1 Notification](../screenshots/jira-queue-showing-r1-notification.png)
- [Jira Ticket Detail - NOC01-10](../screenshots/jira-r1-ticket-detail-noc01-10.png)

---

## 8. Operational Value

This validation demonstrates that the monitoring platform was not only able to detect an outage, but also pass the incident context into the ticketing platform automatically.

Operationally, this provides:

- faster incident intake
- reduced manual ticket creation
- consistent alert formatting
- better traceability between monitoring and service management

For a NOC workflow, this is important because it reduces the delay between detection and documented response.  

---

## 9. Troubleshooting Lessons Learned

This workflow did not work immediately during initial testing.

The key issue was outbound email delivery from PRTG.

### Initial Problem
PRTG alert triggers were firing, but Jira did not receive any incident emails.

### Root Cause
PRTG email delivery was initially using direct delivery and later failed authentication with a regular Gmail password.

### Fix Applied
The workflow started working after:

- changing PRTG to use **one SMTP relay server**
- setting Gmail SMTP relay:
  - `smtp.gmail.com`
  - port `587`
- using **standard SMTP authentication**
- using a **Google app password**
- retesting delivery with Jira intake email

After SMTP delivery was corrected, Jira successfully received live outage notifications.

This was an important lab troubleshooting lesson because the monitoring trigger itself was correct; the failure was in the notification delivery path.

---

## 10. Related Files

- [Incident 01 - Router Down](../incidents/incident-01-router-down.md)
- [Alert Configuration](../monitoring/alert-configuration.md)
- [Dashboard Evidence](../monitoring/dashboard-evidence.md)
- [Escalation Workflow](escalation-workflow.md)
