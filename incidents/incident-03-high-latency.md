# Incident 03 - High Latency

## 1. Incident Summary

This incident documents a controlled high-latency condition in the lab environment to validate performance monitoring, alert visibility, and initial triage of a degraded-but-not-down service state.

Unlike the previous incidents, this scenario focuses on a performance issue where monitored devices remained reachable, but response times increased beyond normal baseline values.

---

## 2. Incident Details

- **Incident ID:** INC-03
- **Title:** High Latency Detected on Monitored Path
- **Severity:** Medium
- **Status:** Resolved
- **Detection Method:** PRTG Ping / Ping v2 response-time increase
- **Affected Device(s):** R1 and/or DSW1 monitored path
- **Affected Services:** Management reachability and response-time performance
- **Environment:** Lab / simulated NOC workflow

---

## 3. Detection

The incident was detected when PRTG showed increased response times while monitored devices still remained reachable.

Observed indicators:
- Ping sensor remained Up
- Ping v2 sensor remained Up
- Response time values increased above normal baseline
- No full device outage occurred

This helped classify the event as a performance degradation issue rather than a hard failure.

### Detection Evidence
- [PRTG Healthy Dashboard View](../screenshots/prtg-dashboard-healthy.png)
- [PRTG High Latency Alert View](../screenshots/high-latency-alert.png)
- [PRTG High Latency Sensor Detail](../screenshots/high-latency-sensor-detail.png)

> Replace screenshot names with your actual filenames if different.

---

## 4. Baseline vs Incident Condition

Normal baseline during healthy operation:
- R1 latency: low and stable
- DSW1 latency: low and stable
- No packet loss
- Sensors remained green

Incident condition:
- Devices remained reachable
- Response times increased noticeably above normal baseline
- Monitoring visibility remained intact
- Issue appeared to be performance-related rather than reachability-related

This distinction is important because latency problems often impact user experience before full outages occur.

---

## 5. Impact Assessment

Observed impact in the lab:

- Devices remained reachable from the monitoring host
- Response time increased during the incident window
- Monitoring indicated degraded performance rather than complete failure
- Service path remained available, but with reduced quality

In a production environment, this type of issue may affect application responsiveness, user experience, and early warning signs of congestion or resource contention.

---

## 6. Initial Triage

Initial triage focused on determining whether the issue was:

- a full outage
- packet loss or reachability failure
- or a performance degradation issue

Checks performed:
- Verified that monitored devices still responded to ping
- Compared current latency values to normal baseline
- Checked whether packet loss occurred
- Correlated the latency increase with the controlled lab action or performance condition

Initial conclusion:
- This was a degradation event, not a device-down or interface-down incident

Commands used during triage:

Windows:
- ping 192.168.61.10
```bash
C:\Windows\System32>ping 192.168.61.10

Pinging 192.168.61.10 with 32 bytes of data:
Reply from 192.168.61.10: bytes=32 time=___ms TTL=255
Reply from 192.168.61.10: bytes=32 time=___ms TTL=255
Reply from 192.168.61.10: bytes=32 time=___ms TTL=255
Reply from 192.168.61.10: bytes=32 time=___ms TTL=255

Ping statistics for 192.168.61.10:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = ___ms, Maximum = ___ms, Average = ___ms
```

Optional device-side checks:

R1:
- show ip interface brief
- show processes cpu history (only if supported and actually used)

DSW1:
- show ip interface brief

## 7. Timeline of Events

| Time | Event                                             |
| ---- | ------------------------------------------------- |
| T0   | Monitored devices in normal baseline state        |
| T1   | Controlled high-latency condition introduced      |
| T2   | PRTG showed elevated response times               |
| T3   | Devices confirmed reachable with degraded latency |
| T4   | Controlled condition removed                      |
| T5   | Response times began returning to baseline        |
| T6   | Normal latency baseline restored                  |

---

## 8. Technical Validation

The incident was validated by confirming that monitored devices remained reachable while latency values increased above normal baseline.

Validation points:

Ping success remained 100%
No full device outage occurred
PRTG response-time values increased during the incident
Response times returned to normal after the controlled condition was removed
Recovery Evidence
PRTG High Latency Recovery

Replace with your actual recovery screenshot filename.

---

9. Root Cause

Root cause: Controlled lab simulation of elevated network latency or delayed response on the monitored path.

The condition was introduced intentionally to validate:

detection of degraded performance
differentiation between latency issues and hard outages
monitoring visibility for non-binary service degradation

This was a deliberate test scenario, not an unexpected platform failure.

---

10. Resolution

The incident was resolved by removing the controlled condition causing elevated latency and allowing monitored response times to return to normal.

Resolution steps:

Remove or stop the condition introducing delay
Allow PRTG to complete the next scan cycle
Verify response times begin to normalize
Confirm sensors remain Up and latency returns close to baseline

Final result:

Reachability remained intact throughout the event
Response times normalized after the test condition ended
Incident condition was resolved successfully

---

11. Scope Identification

The issue was classified as a performance problem rather than a reachability problem because:

monitored devices remained reachable
packet loss did not indicate a hard outage
only latency values increased above baseline
recovery returned response times to normal without interface or device restoration steps

Conclusion:
The failure domain was performance-related and did not indicate total device or path loss.

---

12. Lessons Learned

This incident confirmed the following:

Not all incidents are binary up/down failures
High latency can be an early warning sign before full service loss
Baseline response-time data is important for identifying degraded conditions
Monitoring tools are useful for detecting both outages and performance issues

---

13. NOC Relevance

This incident reflects a common NOC responsibility:

identify degraded performance before full outage occurs
compare current behavior to baseline
distinguish latency issues from reachability failures
monitor recovery back to normal operating levels

For an L1 NOC role, this demonstrates the ability to:

recognize performance degradation
avoid misclassifying the issue as a hard outage
document baseline vs incident behavior clearly

---

14. Related Files
Alert Configuration
Dashboard Evidence
Incident 01 - Router Down
Incident 02 - Interface Down
Incident Summary Table
Jira Ticket Example 03


