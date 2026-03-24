# Host Resource Constraints

## Context
The lab was initially built and tested on a personal laptop with 8 GB RAM while running:
- Windows host OS
- VMware Workstation
- GNS3 VM
- GNS3 topology nodes
- PRTG
- browser/admin tools

## Observation
Under this resource condition, the monitoring path between the host and the GNS3 environment was inconsistent during parts of testing.

## Impact
The internal topology itself remained functional, but host-to-lab monitoring validation was less stable than expected.

## Corrective Action
The host system was upgraded from 8 GB RAM to 16 GB RAM.

## Result
After the RAM upgrade, host-to-lab connectivity became stable enough to continue monitoring integration and validation.
