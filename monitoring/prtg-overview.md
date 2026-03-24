# PRTG Monitoring Overview

## Objective
Provide real-time visibility into network device health, availability, and performance using a centralized monitoring platform.

This setup simulates a Level 1 NOC environment where alerts are generated based on device status, enabling early detection and response to network incidents.

---

## Monitoring Platform

Tool used:
- PRTG Network Monitor

Role in the lab:
- Poll devices using ICMP and SNMP
- Track uptime and availability
- Act as the trigger source for alerts and incidents

---

## Monitored Devices

| Device | IP Address | Role |
|-------|------------|------|
| R1 | 192.168.61.10 (outside) / 10.10.10.1 (inside) | Edge router / gateway |
| DSW1 | 10.10.10.2 | Distribution switch (management SVI) |
| PC1 | 10.10.10.10 | Client endpoint |
| SRV1 | 10.10.10.20 | Server endpoint |

---

## Monitoring Architecture

PRTG is installed on a Windows host and monitors devices across two networks:

- External interface: 192.168.61.0/24 (host side)
- Internal lab network: 10.10.10.0/24

Monitoring flow:

PRTG Server
→ R1 (gateway between networks)
→ Internal network (10.10.10.0/24)
→ DSW1 / PC1 / SRV1

Key requirement:
- Full Layer 3 reachability between PRTG and all monitored devices
- Proper routing and gateway configuration

---

## Monitoring Methods

### ICMP (Ping)
Used for:
- basic reachability checks
- uptime validation
- quick failure detection
- does not provide performance metrics (availability only)

### SNMP (Read-Only)

Used for:
- device identification and uptime
- performance metrics (CPU, memory, interfaces)

Configuration:
- SNMP version: v2c
- community string: `NOCMONRO`
- access control: restricted via ACL
- only PRTG server IP is allowed

Security Note:
- Read-only community is used to prevent configuration changes

---

## What is Being Monitored

Current monitoring coverage:

- Device availability (ICMP ping)
- Network latency (response time)
- System uptime (SNMP)

Planned/optional expansion:
- Interface utilization (bandwidth)
- CPU and memory usage
- Error/discard rates

This aligns with typical L1 NOC monitoring responsibilities.

---

## Why This Matters (NOC Context)

In a real NOC environment:
- monitoring tools detect issues before users report them
- alerts trigger incident workflows
- engineers use monitoring data for troubleshooting
- provides baseline data for identifying anomalies

This lab demonstrates:
- how monitoring depends on proper network reachability
- how SNMP and ICMP are used together
- how monitoring ties into incident and ticket workflows

---

## Limitations of the Lab

- No real production traffic
- Limited SNMP metrics (basic sensors only)
- Single monitoring host (no redundancy)

These limitations are expected in a lab environment but do not affect the core monitoring concepts being demonstrated.

---

## Key Dependencies

Monitoring accuracy depends on:

- Correct IP addressing
- Proper default gateway configuration
- SNMP reachability (UDP 161)
- ICMP allowed between devices

Any failure in these areas will result in false alerts or missing data.

---

## Next Steps

Proceed to:
- device onboarding in PRTG
- sensor configuration
- alert setup
- incident simulation
