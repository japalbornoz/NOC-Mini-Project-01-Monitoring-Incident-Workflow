# PRTG Initial Tests

## Objective
Validate that PRTG can onboard the lab devices after host-to-lab connectivity and device-side monitoring prerequisites are established.

## Devices Added
The following devices were added in PRTG during initial monitoring preparation:

- R1
- DSW1

## Initial Sensors Tested
The following initial sensors were used for onboarding checks:

- Ping
- Ping v2
- SNMP System Uptime

## Current Observed Status
Based on the current PRTG onboarding results:

- Ping sensor for R1: working
- Ping v2 sensor for R1: working
- SNMP System Uptime for R1: working
- Ping sensor for DSW1: working
- Ping v2 sensor for DSW1: working
- SNMP System Uptime for DSW1: working

## What the Initial Tests Showed
The initial PRTG results confirmed that the monitoring host can reach both lab devices at Layer 3 and can successfully poll them using SNMPv2c.

Ping and Ping v2 verified IP reachability from the Windows/PRTG host to both monitored devices.

SNMP System Uptime verified that device-side SNMP configuration was functioning correctly and that PRTG credentials matched the device configuration.

## Issues Identified During Initial Tests
The initial PRTG tests helped validate or expose the following prerequisites:

- Windows host route to the internal lab subnet was required
- bidirectional reachability between the Windows host and lab devices had to be confirmed
- DSW1 required correct off-subnet return-path behavior to respond to the monitoring host
- SNMP-based monitoring required explicit device-side SNMP configuration before SNMP sensors could succeed

## Corrective Actions Completed
The following actions were completed before this validation stage was considered successful:

- validated host-to-router reachability
- added persistent Windows route toward `10.10.10.0/24`
- validated Windows reachability to R1, DSW1, PC1, and SRV1
- validated R1 return reachability to the Windows host
- corrected DSW1 off-subnet reachability behavior
- configured SNMPv2c on R1 and DSW1
- applied a read-only community string for monitoring access
- restricted SNMP access with ACL 10 to permit only the Windows/PRTG host (`192.168.61.1`)
- configured matching SNMP credentials in PRTG

## Validation Notes
The SNMP configuration on both devices used the following model:

- SNMP version: v2c
- community string: `NOCMONRO`
- access mode: read-only
- source restriction: ACL 10 permitting `192.168.61.1`

ACL match counters on both devices confirmed that PRTG polling traffic was reaching and being permitted by the monitored devices.

## Result
PRTG initial onboarding was successful.

The environment is now prepared for:

- additional SNMP sensor validation
- alert testing
- incident simulation
- ticket workflow documentation

## Notes
These initial tests represent the first successful monitoring validation stage for the lab.

At this point, both IP-based monitoring and basic SNMP-based monitoring are operational for R1 and DSW1.
