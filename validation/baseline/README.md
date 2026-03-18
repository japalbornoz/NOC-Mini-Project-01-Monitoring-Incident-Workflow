# Baseline Validation

## Objective

Establish and verify a stable Layer 2 and Layer 3 network baseline for the NOC Mini Project topology before introducing monitoring and incident scenarios.

## Topology Scope

The following devices were validated:

* R1 (Layer 3 gateway)
* DSW1 (Layer 2 switch with management SVI)
* PC1 (user endpoint)
* SRV1 (server endpoint via VPCS)

## Network Overview

* VLAN 10 used for user/data network
* Subnet: 10.10.10.0/24
* Default gateway: 10.10.10.1 (R1)
* Switch management IP: 10.10.10.2

## Validation Checklist

### 1. Interface Status

Verified that all required interfaces are:

* administratively up
* line protocol up

Commands used:

* `show ip interface brief`

---

### 2. VLAN Configuration

Confirmed VLAN 10 exists and access ports are assigned correctly.

Commands used:

* `show vlan brief`

---

### 3. Layer 3 Gateway Functionality

Validated that R1 is routing traffic between endpoints.

Commands used:

* `show ip route`
* ICMP testing

---

### 4. End-to-End Connectivity

Tested bidirectional connectivity between:

| Source | Destination        | Result  |
| ------ | ------------------ | ------- |
| PC1    | R1 (10.10.10.1)    | SUCCESS |
| PC1    | DSW1 (10.10.10.2)  | SUCCESS |
| PC1    | SRV1 (10.10.10.20) | SUCCESS |
| SRV1   | PC1                | SUCCESS |
| R1     | PC1                | SUCCESS |
| R1     | SRV1               | SUCCESS |
| R1     | DSW1               | SUCCESS |

---

### 5. ARP Behavior Observation

Initial packet loss (1 ping) observed during first ICMP attempt due to ARP resolution.

This is expected behavior:

* First packet triggers ARP request
* Subsequent packets succeed

---

## Result Summary

* Layer 2 switching: ✅ Operational
* VLAN segmentation: ✅ Verified
* Layer 3 routing: ✅ Functional
* End-to-end connectivity: ✅ Stable

The network is considered **baseline-ready** for monitoring and incident simulation.

---

## Validation Evidence (Quick Access)

### Device Outputs

* [R1 Show Commands](./r1-show-commands.txt)
* [DSW1 Show Commands](./dsw1-show-commands.txt)
* [PC1 IP Info](./pc1-show-ip.txt)
* [SRV1 IP Info](./srv1-show-ip.txt)

### Connectivity Tests

* [Ping Test Results](./ping-tests.txt)

---

## Next Phase

Proceed to:

* Monitoring integration (PRTG)
* Incident simulation (link failure, device failure)
* Alert validation and documentation
