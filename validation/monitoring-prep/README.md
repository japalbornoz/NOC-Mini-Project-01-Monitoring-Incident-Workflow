# Monitoring Preparation Validation

## Objective
Verify that the monitoring host has a stable Layer 3 path to the GNS3 lab before full PRTG onboarding, sensor validation, and incident simulation.

## Why This Validation Matters
Baseline validation confirmed that the internal lab topology was operational.

This validation phase confirms that the external monitoring host (Windows/PRTG side) can:

- reach the lab router
- reach monitored lab endpoints through the router
- receive return traffic from the lab
- support initial PRTG device onboarding
- support basic SNMP-based polling after device-side SNMP configuration

Without this step, sensor failures could be misread as monitoring-platform issues when the real problem is host-to-lab connectivity or return-path behavior.

## Scope
The following monitoring-prep checks were validated:

- Windows host reachability to R1 on the host-facing subnet
- Windows host reachability to internal lab subnet `10.10.10.0/24`
- persistent Windows route toward the lab subnet via R1
- R1 return-path validation back to the Windows host
- initial PRTG onboarding tests for R1 and DSW1
- successful Ping, Ping v2, and SNMP System Uptime sensor validation for both monitored devices

## Monitoring Path Summary
Working path:

`Windows Host / PRTG -> R1 host-facing interface -> R1 internal interface -> DSW1 / PC1 / SRV1`

## Key Validation Points

### Windows Host Routing
A persistent route was added on the Windows host so that traffic for the internal lab subnet uses R1 as the next hop.

### R1 Host-Facing Reachability
R1 was validated as reachable from the Windows host on the host-facing subnet.

### End-to-End Reachability
The Windows host successfully reached:

- R1 host-facing interface
- R1 internal gateway IP
- DSW1 management SVI
- PC1
- SRV1

### Return Path Validation
R1 successfully reached the Windows host, confirming bidirectional connectivity for monitoring traffic.

### PRTG Onboarding Validation
PRTG successfully onboarded both monitored devices:

- R1
- DSW1

Initial monitoring validation confirmed successful operation of:

- Ping
- Ping v2
- SNMP System Uptime

### SNMP Validation
SNMPv2c was configured on R1 and DSW1 using a read-only community string and a standard ACL permitting only the Windows/PRTG host.

This confirmed that the environment was ready for basic SNMP-based monitoring beyond ICMP reachability testing.

## Evidence Files

### Connectivity Evidence
- [Windows Host Connectivity](./windows-host-connectivity.txt)
- [R1 Host Link Validation](./r1-host-link.txt)

### Monitoring Tool Preparation
- [PRTG Initial Tests](./prtg-initial-tests.md)

## Result Summary
- Host-to-lab monitoring path: ✅ Verified
- Windows route to lab subnet: ✅ Working
- R1 return path to host: ✅ Working
- DSW1 management reachability: ✅ Working
- PRTG onboarding for R1 and DSW1: ✅ Working
- Basic SNMP polling validation: ✅ Working
- Environment ready for monitoring buildout: ✅ Yes

## Next Step
Proceed to:

- additional PRTG sensor deployment
- monitored interface and uptime checks
- alert validation
- incident simulation
- ticket documentation
