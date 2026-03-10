# NOC-Mini-Project-01-Monitoring-Incident-Workflow

- Overview
- Monitoring Topology
- Tools Used
- Sensors Configured
- Incident Scenarios
- Incident Workflow
- Lessons Learned


# Addressing Table

| Device              | Interface         | IP Address      | Purpose                        |
|---------------------|------------------|-----------------|--------------------------------|
| PRTG Server         | NIC              | 192.168.10.10   | Monitoring platform            |
| Edge Router         | G0/0             | 192.168.10.1    | Gateway / monitored device     |
| Distribution Switch | VLAN 10          | 192.168.10.2    | Switch management              |
| Server              | NIC              | 192.168.10.20   | Monitored endpoint             |
| PC                  | NIC              | 192.168.10.30   | Test client                    |
