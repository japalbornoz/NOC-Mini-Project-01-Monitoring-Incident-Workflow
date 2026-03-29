# Connectivity - Host to Lab

## 1. Purpose

This document explains how the Windows monitoring host was connected to the GNS3 lab environment and how end-to-end reachability was validated before monitoring was configured in PRTG.

The goal is to show that monitoring depended on a working Layer 3 path between the host system and the lab network.

---

## 2. Objective

The objective of this troubleshooting note is to document:

- the host-to-lab connectivity path
- the IP addressing involved
- the role of R1 as the gateway between networks
- the validation steps used to confirm reachability
- the common failure symptoms when this path is broken

---

## 3. Connectivity Overview

The monitoring host was connected to the lab through a VMware host-only network and the router inside the GNS3 topology.

Traffic path:

Windows Monitoring Host  
→ VMware host-only adapter  
→ R1 host-facing interface  
→ R1 internal interface  
→ DSW1 / PC1 / SRV1 on the lab network

This path allowed the Windows host running PRTG to monitor both the router-facing segment and the downstream internal lab devices.

---

## 4. Networks Involved

### Host-Side Network
- **VMware host-only network:** `192.168.61.0/24`
- **Windows host IP:** `192.168.61.1`
- **R1 host-facing IP:** `192.168.61.10`

### Internal Lab Network
- **Lab network:** `10.10.10.0/24`
- **R1 internal interface:** `10.10.10.1`
- **DSW1 management SVI:** `10.10.10.2`
- **PC1:** `10.10.10.10`
- **SRV1:** `10.10.10.20`

---

## 5. Why This Connectivity Was Required

PRTG was installed on the Windows host, not inside the GNS3 topology.

Because of that, the monitoring host needed working IP reachability to:

- R1 on the host-facing segment
- downstream devices on the internal lab network
- sensors using ICMP and SNMP across the routed path

Without this connectivity, PRTG would not be able to:

- poll devices
- confirm availability
- collect SNMP-based monitoring data
- generate meaningful alerts for lab incidents

---

## 6. Validation Performed

Before configuring monitoring in PRTG, host-to-lab connectivity was validated from the Windows system.

### Windows Host Validation

Typical tests included:

```bash
ping 192.168.61.10
ping 10.10.10.2
ping 10.10.10.10
ping 10.10.10.20
```

These tests confirmed that the monitoring host could reach:

- the upstream router on the host-facing segment
- the switch management IP on the internal network
- downstream lab endpoints behind R1

### Interpretation

- **Successful ping to `192.168.61.10`**  
  confirmed host reachability to R1 on the VMware host-only segment

- **Successful ping to `10.10.10.2`, `10.10.10.10`, and `10.10.10.20`**  
  confirmed that routed access through R1 to the internal lab network was working

- **Failure to reach downstream devices while R1 remained reachable**  
  suggested a downstream path, interface, VLAN, or routing issue

---

## 7. Role of R1 in the Connectivity Path

R1 acted as the routed boundary between the host-facing VMware network and the internal lab network.

### R1 Function

R1 provided:

- host-facing connectivity on `192.168.61.10`
- internal lab routing on `10.10.10.1`
- the Layer 3 path required for the monitoring host to reach devices behind the router

### Operational Meaning

If R1 was unreachable:
- PRTG would lose direct reachability to the router
- downstream devices on `10.10.10.0/24` would also become unreachable from the monitoring host

If R1 remained reachable but downstream devices were unreachable:
- the issue was more likely downstream of the router
- the fault domain would shift toward DSW1 or the R1-to-DSW1 path

---

## 8. Common Failure Symptoms

When host-to-lab connectivity was not working correctly, the following symptoms were expected:

- PRTG sensors showed devices as unreachable
- SNMP-based monitoring did not return usable data
- the Windows host could reach some addresses but not the full lab path
- Jira ticket creation could still work from manual email tests, but real monitoring alerts would not reflect actual device state

### Typical Failure Patterns

- **Windows host cannot reach `192.168.61.10`**  
  suggests a host-only adapter, VMware, or host-to-R1 path issue

- **Windows host can reach `192.168.61.10` but not `10.10.10.0/24` devices**  
  suggests an internal routing, interface, VLAN, or downstream path issue

- **PRTG can monitor R1 but not downstream devices**  
  suggests that the host-facing path is working, but internal lab reachability is incomplete

- **Monitoring appears inconsistent across devices**  
  suggests that connectivity should be validated first before assuming a sensor or alerting problem

---

## 9. Troubleshooting Approach

When connectivity between the Windows host and the lab was in question, the troubleshooting approach was:

1. verify the Windows host IP on the VMware host-only adapter
2. verify reachability to `192.168.61.10`
3. verify reachability to `10.10.10.2`, `10.10.10.10`, and `10.10.10.20`
4. confirm R1 interface status and routed path condition
5. determine whether the issue was upstream, downstream, or monitoring-specific

This sequence helped isolate whether the problem was:

- host-side connectivity
- router reachability
- downstream path failure
- or a monitoring/notification issue rather than a network path issue


---

## 10. Key Takeaway

Host-to-lab reachability was a foundational requirement for the entire monitoring workflow.

Before trusting PRTG sensor results, it was necessary to confirm that the Windows monitoring host could successfully reach both:

- the host-facing router interface
- the downstream internal lab devices

This ensured that monitoring failures could be interpreted correctly as either:

- real device or path issues
- or connectivity problems between the monitoring host and the lab

---

## 11. Related Files

- [Monitoring Runbook](monitoring-runbook.md)
- [False Positives](false-positives.md)
- [Platform Stability](platform-stability.md)
- [PRTG Device Setup](../monitoring/prtg-device-setup.md)
- [Incident 01 - Router Down](../incidents/incident-01-router-down.md)
- [Incident 02 - Interface Down](../incidents/incident-02-interface-down.md)
