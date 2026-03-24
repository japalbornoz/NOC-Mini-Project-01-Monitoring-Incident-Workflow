# PRTG Initial Tests

## Objective
Validate that PRTG can begin onboarding the lab devices after host-to-lab connectivity is established.

## Devices Added
The following devices were added in PRTG during initial monitoring preparation:

- R1
- DSW1

## Initial Sensors Tested
The following initial sensors were used for onboarding checks:

- Ping
- Ping v2
- SNMP System Uptime

## What the Initial Tests Showed
Initial PRTG results were useful for identifying path prerequisites before full monitoring validation.

During early onboarding attempts, sensor failures indicated that the problem was not the monitoring platform itself, but the reachability path between the monitoring host and the GNS3 lab.

## Issues Identified During Initial Tests
The initial PRTG tests helped expose the following prerequisites:

- Windows host route to the internal lab subnet was required
- bidirectional reachability between the Windows host and lab devices had to be confirmed
- DSW1 required correct return-path behavior for off-subnet management reachability

## Corrective Actions
The following actions were completed before moving forward:

- validated host-to-router reachability
- added persistent Windows route toward `10.10.10.0/24`
- validated Windows reachability to R1, DSW1, PC1, and SRV1
- validated R1 return reachability to the Windows host
- corrected DSW1 off-subnet reachability behavior using a default route via R1

## Result
PRTG initial onboarding was confirmed as a useful validation stage for detecting connectivity prerequisites before full sensor deployment.

The environment is now prepared for:
- final sensor validation
- alert testing
- incident simulation
- ticket workflow documentation

## Notes
These initial tests are part of monitoring preparation, not final production-state monitoring evidence.
