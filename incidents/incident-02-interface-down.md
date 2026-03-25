<img width="1291" height="560" alt="image" src="https://github.com/user-attachments/assets/d4fb00ba-cc48-49bd-b7ff-045fd2e41ecb" /># Incident 02 - Interface Down

## 1. Incident Summary

This incident documents a controlled interface or link outage in the lab environment to validate monitoring visibility, incident detection, and initial triage of a partial network failure.

Unlike the router-down scenario, this incident focuses on a link-level fault affecting connectivity between monitored devices while the router itself remains operational.

---

## 2. Incident Details

- **Incident ID:** INC-02
- **Title:** R1 to DSW1 Interface / Link Down
- **Severity:** Medium
- **Status:** Resolved
- **Detection Method:** PRTG sensor alert and connectivity loss
- **Affected Device(s):** R1 / DSW1 link path
- **Affected Services:** Routed or switched connectivity across the affected path
- **Environment:** Lab / simulated NOC workflow

---

## 3. Detection

The incident was detected when connectivity across the R1-to-DSW1 path was disrupted while the router itself remained reachable.

Observed indicators:
- R1 (192.168.61.10) remained reachable via ICMP from PRTG
- DSW1 (10.10.10.2) became unreachable
- PC1 (10.10.10.10) and SRV1 (10.10.10.20) became unreachable from the monitoring host
- PRTG showed partial failure (not full device outage)

This helped distinguish the event from Incident 01, where all R1 sensors failed due to full device unavailability.

### Detection Evidence
PRTG Dashboard
- [PRTG Healthy Dashboard View](../screenshots/prtg-dashboard-healthy.png)
- [PRTG Dashboard DSW1 Down](../screenshots/prtg-dashboard-dsw1-down.png)
- [PRTG DSW1 Sensors List Down](../screenshots/dsw1-down-alert.png)

GNS3 Topology
- [GNS3 DSW1 Interface e0/0 Down](../screenshots/gns3-topology-dsw1-interface-e0-down.png)

---

## 4. Impact Assessment

Observed impact in the lab:

- R1 remained operational and reachable
- DSW1 management SVI (10.10.10.2) became unreachable
- End devices (PC1, SRV1) behind DSW1 were not reachable from the monitoring host
- Traffic between R1 and VLAN 10 segment was interrupted

---

## 5. Initial Triage

Initial triage focused on determining whether the problem was:

- a full router outage
- a monitoring-only issue
- or a partial path / interface failure

Checks performed:
- Verified that R1 still responded to monitoring
- Compared sensor behavior to the full router-down scenario
- Checked whether downstream connectivity changed during the event
- Correlated the issue with the controlled interface or link action in the lab

Initial conclusion:
- The device remained operational, but a specific path or interface was unavailable

Commands used during triage:

R1:
- show ip interface brief
```cisco
R1#sh ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            10.10.10.1      YES NVRAM  up                    up
FastEthernet0/1            192.168.61.10   YES NVRAM  up                    up
FastEthernet1/0            unassigned      YES NVRAM  administratively down down
```

- ping 192.168.61.1
```cisco
R1#ping 192.168.61.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.61.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 60/64/68 ms
```

- ping 10.10.10.2
```cisco
R1#ping 10.10.10.2

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.10.10.2, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)
```

DSW1:
- show ip interface brief
```cisco
Interface              IP-Address      OK? Method Status                Protocol
Ethernet0/0            unassigned      YES unset  administratively down down
Ethernet0/1            unassigned      YES unset  up                    up
Ethernet0/2            unassigned      YES unset  up                    up
Ethernet0/3            unassigned      YES unset  up                    up
Ethernet1/0            unassigned      YES unset  up                    up
Ethernet1/1            unassigned      YES unset  up                    up
Ethernet1/2            unassigned      YES unset  up                    up
Ethernet1/3            unassigned      YES unset  up                    up
Ethernet2/0            unassigned      YES unset  up                    up
Ethernet2/1            unassigned      YES unset  up                    up
Ethernet2/2            unassigned      YES unset  up                    up
Ethernet2/3            unassigned      YES unset  up                    up
Ethernet3/0            unassigned      YES unset  up                    up
Ethernet3/1            unassigned      YES unset  up                    up
Ethernet3/2            unassigned      YES unset  up                    up
Ethernet3/3            unassigned      YES unset  up                    up
Vlan1                  unassigned      YES unset  administratively down down
Vlan10                 10.10.10.2      YES NVRAM  up                    up
```

Windows:
- ping 192.168.61.10
```bash
C:\Windows\System32>ping 192.168.61.10

Pinging 192.168.61.10 with 32 bytes of data:
Reply from 192.168.61.10: bytes=32 time=3ms TTL=255
Reply from 192.168.61.10: bytes=32 time=8ms TTL=255
Reply from 192.168.61.10: bytes=32 time=5ms TTL=255
Reply from 192.168.61.10: bytes=32 time=5ms TTL=255

Ping statistics for 192.168.61.10:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 3ms, Maximum = 8ms, Average = 5ms
```

- ping 10.10.10.2
```bash
C:\Windows\System32>ping 10.10.10.2

Pinging 10.10.10.2 with 32 bytes of data:
Request timed out.
Request timed out.
Request timed out.
Request timed out.

Ping statistics for 10.10.10.2:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
```

- ping 10.10.10.10
```bash
C:\Windows\System32>ping 10.10.10.10

Pinging 10.10.10.10 with 32 bytes of data:
Request timed out.
Request timed out.
Request timed out.
Request timed out.

Ping statistics for 10.10.10.10:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
```

---

## 6. Timeline of Events

| Time | Event |
|------|-------|
| T0 | R1 and DSW1 in normal monitored state |
| T1 | Controlled interface or link outage introduced |
| T2 | Monitoring reflected partial connectivity impact |
| T3 | Incident condition confirmed as interface/path-related |
| T4 | Interface or link restored |
| T5 | Monitoring visibility and connectivity began recovery |
| T6 | Normal state fully restored |

---

## 7. Technical Validation

The incident was validated by confirming that the router remained operational while the affected path experienced disruption.

Validation points:

- R1 FastEthernet0/1 (192.168.61.10) remained up/up
- R1 FastEthernet0/0 (10.10.10.1) remained up/up
- No response from DSW1 (10.10.10.2), indicating downstream failure
- R1 maintained upstream connectivity while downstream reachability to DSW1 and end devices failed
- Connectivity restored immediately after interface recovery

### Recovery Evidence
- [PRTG Dashboard DSW1 Down](../screenshots/prtg-dashboard-dsw1-up.png)
- [PRTG DSW1 Sensors List Down](../screenshots/dsw1-up-alert.png)

GNS3 Topology
- [GNS3 DSW1 Interface e0/0 Down](../screenshots/gns3-topology-dsw1-interface-e0-up.png)

---

## 8. Root Cause

**Root cause:** Controlled administrative shutdown or disconnection of the DSW1 uplink interface (`Ethernet0/0`) connected to R1.

This caused loss of Layer 2 connectivity between R1 and the downstream switched segment, making DSW1 and attached end devices unreachable from the monitoring host.

The outage was introduced intentionally to validate:
- detection of partial network failures
- differentiation between device failure and path failure
- monitoring and incident workflow for link-level issues

This was a deliberate test scenario, not an unexpected platform failure.

---

## 9. Resolution

The incident was resolved by restoring the affected interface or link in the lab environment.

Resolution steps:

1. Re-enabled the affected interface:
   - DSW1: interface Ethernet0/0 → no shutdown

2. Verified interface status:
   - show ip interface brief

3. Validated connectivity:
   - ping 10.10.10.2
   - ping 10.10.10.10

4. Confirmed PRTG sensors returned to UP state

Final result:
- The affected path returned to normal operation
- Monitoring visibility stabilized
- Incident condition was resolved successfully

---

## 10. Scope Identification

The issue was isolated to the R1-to-DSW1 link because:

- R1 remained reachable from the monitoring host
- Only downstream devices behind DSW1 were affected
- No impact observed on the host-facing subnet (192.168.61.0/24)

Conclusion:
The failure domain was limited to the internal switching segment, not the edge router or monitoring path.

---


## 11. Lessons Learned

This incident confirmed the following:

- A reachable router does not guarantee that all downstream paths are healthy
- Partial failures require different triage from full device outages
- Comparing multiple sensors helps distinguish scope of impact
- Link-level testing is useful for validating monitoring accuracy and operator response

---

## 12. NOC Relevance

This incident reflects a common NOC responsibility:

- identify whether an issue is device-wide or path-specific
- determine the likely scope of impact
- confirm whether the fault is isolated to an interface, segment, or uplink
- validate recovery after restoration

For an L1 NOC role, this demonstrates the ability to:
- recognize partial network failures
- avoid misclassifying the incident as a full outage
- document scope and recovery accurately

---

## 13. Related Files

- [Alert Configuration](../monitoring/alert-configuration.md)
- [Dashboard Evidence](../monitoring/dashboard-evidence.md)
- [Incident 01 - Router Down](incident-01-router-down.md)
- [Incident Summary Table](incident-summary-table.md)
- [Jira Ticket Example 02](../tickets/jira-ticket-example-02.md)
