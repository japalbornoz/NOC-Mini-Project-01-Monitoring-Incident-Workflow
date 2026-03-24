# Addressing Plan

## Management / Monitoring Subnet

| Device | Interface | IP Address | Subnet Mask | Default Gateway | Purpose |
|---|---|---|---|---|---|
| Windows Host / PRTG | VMnet1 | 192.168.61.1 | 255.255.255.0 | N/A | Monitoring host |
| R1 | FastEthernet0/1 | 192.168.61.10 | 255.255.255.0 | N/A | Host-facing / monitoring path |

## Internal Lab Subnet

| Device | Interface | IP Address | Subnet Mask | Default Gateway | Purpose |
|---|---|---|---|---|---|
| R1 | FastEthernet0/0 | 10.10.10.1 | 255.255.255.0 | N/A | Internal gateway |
| DSW1 | Vlan10 | 10.10.10.2 | 255.255.255.0 | 10.10.10.1 | Switch management SVI |
| PC1 | e0 | 10.10.10.10 | 255.255.255.0 | 10.10.10.1 | Client endpoint |
| SRV1 | e0 | 10.10.10.20 | 255.255.255.0 | 10.10.10.1 | Server endpoint |

## Routing Notes

- The Windows host uses a persistent route for `10.10.10.0/24` via `192.168.61.10`.
- DSW1 uses a default route toward `10.10.10.1` for off-subnet management reachability.
- R1 provides Layer 3 connectivity between the host-facing subnet and the internal lab subnet.
