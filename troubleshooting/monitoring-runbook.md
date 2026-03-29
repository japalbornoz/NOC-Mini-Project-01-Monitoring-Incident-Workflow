# Monitoring Troubleshooting Runbook

## 1. Purpose

This runbook provides a structured troubleshooting workflow for the monitoring and ticketing components used in this lab.

It is intended to help validate whether an issue is caused by:

- a real device outage
- a path or interface failure
- a monitoring configuration problem
- a notification delivery problem
- a host or platform stability problem

This document is written as a quick operational guide for repeated use during lab testing.

---

## 2. Scope

This runbook applies to the following components in the project:

- **PRTG Network Monitor**
- **Jira Service Management**
- **Windows monitoring host**
- **GNS3 topology**
- **R1**
- **DSW1**
- **host-to-lab connectivity path**
- **SMTP notification delivery**

---

## 3. When to Use This Runbook

Use this runbook when any of the following occurs:

- a PRTG sensor changes to Down unexpectedly
- a monitored device becomes unreachable
- Jira does not create a ticket from a PRTG alert
- connectivity from the monitoring host to the lab fails
- monitoring results appear inconsistent
- GNS3 or VMware instability affects testing

---

## 4. Troubleshooting Workflow Summary

| Step | Check | Goal |
|------|-------|------|
| 1 | Confirm sensor state in PRTG | Identify what failed |
| 2 | Identify affected device and sensor | Determine scope |
| 3 | Test basic reachability from monitoring host | Confirm connectivity |
| 4 | Check whether issue is device-wide or path-specific | Narrow fault domain |
| 5 | Validate GNS3 topology/device state | Confirm lab-side cause |
| 6 | Review notification/ticketing flow if no Jira ticket appears | Validate alert delivery |
| 7 | Check host/platform health if behavior is inconsistent | Rule out lab instability |
| 8 | Restore service and validate recovery | Confirm issue resolution |

---

## 5. Standard Troubleshooting Sequence

### Step 1 – Confirm the Alert in PRTG

Check:
- affected device
- affected sensor
- current sensor state
- time of last up/down transition
- sensor message

Questions to answer:
- Is this a single sensor failure?
- Did multiple sensors fail at the same time?
- Is the affected device still visible in the overview?

### Step 2 – Identify the Failure Type

Use the sensor pattern to classify the issue.

#### Possible patterns

**Pattern A – Full device outage**
- multiple sensors on the same device go Down
- device becomes fully unreachable
- likely device failure or node shutdown

**Pattern B – Partial path/interface failure**
- upstream device remains reachable
- downstream device becomes unreachable
- likely interface, path, or segment issue

**Pattern C – Notification failure**
- sensor changes state correctly
- no Jira request is created
- likely SMTP or delivery path issue

**Pattern D – Platform instability**
- results inconsistent or delayed
- GNS3 / VMware becomes unstable
- likely host resource or virtualization issue

---

## 6. Reachability Validation

### From the Windows Monitoring Host

Use ping to test monitored reachability.

Typical commands:

```bash
ping 192.168.61.10
ping 10.10.10.2
ping 10.10.10.10
ping 10.10.10.20
```

### Interpretation

- **R1 reachable, DSW1 unreachable**  
  suggests a downstream path or interface problem

- **R1 unreachable and downstream devices unreachable**  
  suggests an upstream router or device failure

- **All devices reachable but no Jira ticket appears**  
  suggests a notification workflow issue rather than a network failure

---  

## 7. Device and Path Validation

### R1 Checks

Typical commands:

```cisco
show ip interface brief
ping 192.168.61.1
ping 10.10.10.2
```

Use these to determine:
- whether R1 is up
- whether the monitoring-host-facing side is working
- whether downstream reachability is intact

### DSW1 Checks

Typical commands:
```md id="ne73r1"
show ip interface brief
```

Use this to determine:
- whether the uplink interface is up/down
- whether the switch is still operating
- whether the management SVI remains usable

### Path Interpretation

- **R1 up, DSW1 down**  
  likely interface or path failure

- **R1 down, DSW1 unreachable**  
  likely full upstream outage

---

## 8. Notification and Jira Validation

Use this section when PRTG detects an alert but Jira does not create a request.

### Check 1 – Trigger Configuration

Verify:
- the correct sensor is being used
- notification inheritance is disabled where appropriate
- the state trigger is configured correctly
- the trigger action points to the correct Jira notification template

### Check 2 – Notification Template

Verify:
- the recipient email address is correct
- the Jira intake address is correct
- the subject and body format are correct
- the template is active

### Check 3 – SMTP Delivery

Verify:
- SMTP relay is enabled
- Gmail relay settings are correct
- authentication succeeds
- Test SMTP Settings works successfully

### Check 4 – Jira Intake

Verify:
- the Jira email channel is active
- the service project is open for intake
- a manual test email creates a request
- incoming requests appear in the expected queue

---

## 9. False-Positive Control

Not every affected sensor should create its own Jira ticket.

### Rules used in this lab

- use **one primary sensor** per scenario for alert-to-ticket integration
- treat other affected sensors as correlated evidence
- do not create duplicate ticket sources for the same outage

### Examples

**Router outage**
- primary trigger: `R1 Ping v2`
- supporting sensors:
  - Ping
  - SNMP System Uptime
  - TO_DSW1 Traffic

**Interface/path failure**
- primary trigger: `DSW1 Ping v2`
- supporting sensors:
  - DSW1 Ping
  - DSW1 SNMP System Uptime

This keeps the incident workflow cleaner and more realistic.

---

## 10. Platform Health Checks

Use this section when results are inconsistent or the lab behaves unexpectedly.

### Check the monitoring host

Review:
- RAM usage
- number of open browser tabs
- number of active virtual machines
- overall Windows responsiveness

### Check VMware / GNS3

Review:
- GNS3 VM status
- VMware errors
- topology responsiveness
- node boot consistency

### Interpretation

- unstable host or VM behavior can distort monitoring results
- performance issues in the lab platform are not the same as real network faults
- fix platform stability first before trusting new monitoring results

---

## 11. Recovery Validation

After corrective action, confirm that:

- the affected device or interface has been restored
- the PRTG sensor returns to Up/OK state
- the Jira ticket contains enough detail for closure
- connectivity tests pass again
- the issue no longer reproduces unexpectedly

Do not consider the issue resolved until both monitoring and reachability return to normal.

---

## 12. Escalation Guidance

Escalate when:

- the issue cannot be restored through expected corrective action
- the fault domain is unclear
- monitoring and device state do not match
- multiple dependent components are affected
- notification delivery still fails after SMTP and Jira checks

In this lab, escalation is simulated. In a production environment, this would typically move to a higher support tier such as network engineering or infrastructure operations.

---

## 13. Quick Reference Checklist

### Monitoring Alert Investigation
- [ ] Identify the affected sensor
- [ ] Identify the affected device
- [ ] Check whether multiple sensors are impacted
- [ ] Determine if the issue is device-wide or path-specific

### Connectivity Validation
- [ ] Ping R1 from the Windows host
- [ ] Ping DSW1 from the Windows host
- [ ] Check the relevant device interfaces
- [ ] Confirm whether the upstream path is still healthy

### Jira / Notification Validation
- [ ] Check the notification trigger
- [ ] Check the notification template
- [ ] Check the SMTP relay settings
- [ ] Confirm Jira email intake is working

### Recovery Validation
- [ ] Restore the device or interface
- [ ] Confirm the PRTG sensor returns to Up
- [ ] Confirm connectivity is restored
- [ ] Update the ticket or incident notes

---

## 14. Related Files

- [Connectivity Host to Lab](connectivity-host-to-lab.md)
- [False Positives](false-positives.md)
- [Platform Stability](platform-stability.md)
- [Incident 01 - Router Down](../incidents/incident-01-router-down.md)
- [Incident 02 - Interface Down](../incidents/incident-02-interface-down.md)
- [Escalation Workflow](../tickets/escalation-workflow.md)

