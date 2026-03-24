# PRTG Monitoring Overview

## Objective
Provide visibility into network device health, availability, and performance using a centralized monitoring platform.

This monitoring setup simulates a basic NOC environment where devices are continuously checked and alerts are generated when issues occur.

---

## Monitoring Platform
Tool used:
- PRTG Network Monitor

Role in the lab:
- Poll devices using ICMP and SNMP
- Track uptime and availability
- Serve as the trigger source for incidents and tickets

---

## Monitored Devices

| Device | IP Address | Role |
|-------|------------|------|
| R1 | 192.168.61.10 / 10.10.10.1 | Edge router / gateway |
| DSW1 | 10.10.10.2 | Distribution switch (management SVI) |
| PC1 | 10.10.10.10 | Client endpoint |
| SRV1 | 10.10.10.20 | Server endpoint |

---

## Monitoring Architecture

Monitoring path:

Windows Host (PRTG)
→ R1 (192.168.61.10)
→ Internal Network (10.10.10.0/24)
→ DSW1 / PC1 / SRV1

Key requirement:
- Bidirectional Layer 3 connectivity between PRTG and all monitored devices

---

## Monitoring Methods

### ICMP (Ping)
Used for:
- basic reachability checks
- uptime validation
- quick failure detection

### SNMP (Read-Only)
Used for:
- system information (uptime, device identity)
- interface and performance metrics (future expansion)

Configuration approach:
- community string: `NOCMONRO`
- access restricted using ACL
- monitoring host permitted explicitly

---

## What is Being Monitored

Initial scope includes:
- device availability (up/down)
- response time (latency)
- system uptime via SNMP

This reflects typical NOC Level 1 visibility requirements.

---

## Why This Matters (NOC Context)

In a real NOC environment:
- monitoring tools detect issues before users report them
- alerts trigger incident workflows
- engineers use monitoring data for troubleshooting

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

## Next Steps

Proceed to:
- device onboarding in PRTG
- sensor configuration
- alert setup
- incident simulation
