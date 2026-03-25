# Incident 03 - Monitoring Host Resource Constraint

## 1. Incident Summary

This incident documents a monitoring and lab-platform instability event caused by high host resource usage during testing.

Unlike the previous two incidents, this scenario does not represent a network device or link failure. Instead, it focuses on the monitoring and virtualization platform becoming unstable due to resource pressure on the host system.

---

## 2. Incident Details

- **Incident ID:** INC-03
- **Title:** Monitoring Host Resource Constraint / Lab Platform Instability
- **Severity:** Medium
- **Status:** Resolved
- **Detection Method:** Host performance monitoring, VMware instability, GNS3 VM behavior
- **Affected Component(s):** Monitoring host / virtualization platform / GNS3 lab environment
- **Affected Services:** Lab stability, monitoring test reliability, topology availability
- **Environment:** Lab / simulated NOC workflow

---

## 3. Detection

The incident was detected when the host system reached high memory utilization while multiple applications, browser tabs, and virtual machines were running simultaneously.

Observed indicators:
- Host memory usage increased to a critical level
- Multiple VMs were open at the same time
- VMware and/or GNS3 behavior became unstable
- There was increased risk of topology freeze, monitoring inconsistency, or VM crash

This indicated that the issue was not caused by a network outage, but by platform resource saturation affecting the lab environment.

### Detection Evidence

**Host resource usage**
- [Windows Task Manager - High Memory Usage](../screenshots/host-memory-high.png)

**Virtualization load**
- [VMware - Multiple VMs Open](../screenshots/vmware-multiple-vms-open.png)

**Platform instability**
- [VMware Unrecoverable Error](../screenshots/vmware-unrecoverable-error.png)

> Replace screenshot filenames if your actual names are different.

---

## 4. Impact Assessment

Observed impact in the lab:

- GNS3 and VMware stability was reduced under high host load
- Lab testing reliability was affected
- Monitoring results risked becoming misleading due to platform contention
- There was a risk of topology interruption or forced restart

This type of issue does not represent a direct network fault, but it can still affect monitoring accuracy and incident testing quality.

---

## 5. Initial Triage

Initial triage focused on determining whether the issue was:

- a network problem
- a GNS3 node problem
- or a host / virtualization resource problem

Checks performed:
- Reviewed host memory usage in Task Manager
- Confirmed multiple virtual machines and applications were running
- Correlated instability with increased host resource consumption
- Observed VMware and/or GNS3 instability symptoms during lab operation

Initial conclusion:
- The issue was caused by host resource saturation affecting the monitoring and virtualization platform

---

## 6. Timeline of Events

| Time | Event |
|------|-------|
| T0 | Lab operating normally |
| T1 | Additional applications, browser tabs, and virtual machines opened |
| T2 | Host memory usage increased significantly |
| T3 | VMware / GNS3 stability issues observed |
| T4 | Resource pressure identified as the likely cause |
| T5 | Unnecessary workloads were reduced or closed |
| T6 | Lab environment returned to a more stable operating state |

> Replace `T0` to `T6` with actual timestamps if you recorded them.

---

## 7. Technical Validation

The incident was validated by correlating host resource usage with observed platform instability.

Validation points:
- Host memory usage was elevated during the issue window
- Multiple active workloads were present at the same time
- The observed instability affected the virtualization and lab platform, not a specific network device
- Reducing active workloads improved stability

### Supporting Evidence

**Host memory pressure**
- [Windows Task Manager - High Memory Usage](../screenshots/host-memory-high.png)

**VMware platform condition**
- [VMware - Multiple VMs Open](../screenshots/vmware-multiple-vms-open.png)
- [VMware Unrecoverable Error](../screenshots/vmware-unrecoverable-error.png)

---

## 8. Root Cause

**Root cause:** High host resource consumption during lab operation, caused by running multiple browser tabs, applications, and virtual machines alongside GNS3 and the monitoring environment.

This created pressure on system memory and virtualization resources, which reduced lab stability and introduced risk to accurate monitoring and repeatable incident testing.

This was a controlled lab/platform limitation issue, not a production network failure.

---

## 9. Resolution

The incident was resolved by reducing host workload and returning the lab platform to a stable operating state.

Resolution steps:
1. Closed unnecessary browser tabs and applications
2. Reduced the number of active virtual machines
3. Restarted or stabilized affected VMware / GNS3 components as needed
4. Reopened only the required lab components
5. Confirmed the topology and monitoring environment were stable again

Final result:
- Host resource pressure was reduced
- VMware / GNS3 stability improved
- Lab operations became more reliable for continued testing

---

## 10. Scope Identification

The issue was isolated to the monitoring host / virtualization platform because:

- The instability correlated with host memory pressure
- Symptoms appeared at the platform level rather than on one network device
- Reducing host workload improved platform behavior
- The issue affected lab reliability, not just one interface or router

Conclusion:
The failure domain was the host and virtualization environment, not the network topology itself.

---

## 11. Lessons Learned

This incident confirmed the following:

- Monitoring accuracy depends on platform stability as well as network reachability
- High host resource usage can distort lab testing and incident simulation
- A stable baseline environment is important before performing performance-related tests
- Not every incident in a monitoring project is a network fault; some are platform faults

---

## 12. NOC Relevance

This incident reflects an important operational principle:

- monitoring systems are only as reliable as the platforms supporting them
- host or virtualization instability can create misleading observations
- platform health must be considered during troubleshooting

For an L1 NOC role, this demonstrates the ability to:
- distinguish network faults from platform faults
- identify environmental causes of monitoring instability
- document operational risk accurately

---

## 13. Related Files

- [Alert Configuration](../monitoring/alert-configuration.md)
- [Dashboard Evidence](../monitoring/dashboard-evidence.md)
- [Incident 01 - Router Down](incident-01-router-down.md)
- [Incident 02 - Interface Down](incident-02-interface-down.md)
- [Incident Summary Table](incident-summary-table.md)
- [Lessons from Platform Instability](../troubleshooting/lessons-from-platform-instability.md)
