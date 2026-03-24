# PRTG Device Setup

## 1. Objective

This section documents the process of onboarding network devices into PRTG for monitoring.

The goal is to ensure all devices are reachable and properly configured before sensors are applied.

---

## 2. Prerequisites

Before adding devices to PRTG, the following requirements must be met:

### Network Connectivity
- PRTG server must be able to reach all devices via IP
- Routing must be correctly configured between networks
- Devices must have correct default gateways

### ICMP Reachability
- Devices must respond to ping from the PRTG server

Verification:
Connectivity from the PRTG server was validated using ICMP:

#### R1 (10.10.10.1)
```bash
C:\Windows\System32>ping 10.10.10.1

Pinging 10.10.10.1 with 32 bytes of data:
Reply from 10.10.10.1: bytes=32 time=6ms TTL=255
Reply from 10.10.10.1: bytes=32 time=5ms TTL=255
Reply from 10.10.10.1: bytes=32 time=9ms TTL=255
Reply from 10.10.10.1: bytes=32 time=8ms TTL=255

Ping statistics for 10.10.10.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 5ms, Maximum = 9ms, Average = 7ms
```

#### VLAN10 (10.10.10.2)
```bash
C:\Windows\System32>ping 10.10.10.2

Pinging 10.10.10.2 with 32 bytes of data:
Reply from 10.10.10.2: bytes=32 time=20ms TTL=254
Reply from 10.10.10.2: bytes=32 time=15ms TTL=254
Reply from 10.10.10.2: bytes=32 time=25ms TTL=254
Reply from 10.10.10.2: bytes=32 time=23ms TTL=254

Ping statistics for 10.10.10.2:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 15ms, Maximum = 25ms, Average = 20ms
```

#### PC1 (10.10.10.10)
```bash
C:\Windows\System32>ping 10.10.10.10

Pinging 10.10.10.10 with 32 bytes of data:
Reply from 10.10.10.10: bytes=32 time=34ms TTL=63
Reply from 10.10.10.10: bytes=32 time=13ms TTL=63
Reply from 10.10.10.10: bytes=32 time=34ms TTL=63
Reply from 10.10.10.10: bytes=32 time=16ms TTL=63

Ping statistics for 10.10.10.10:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 13ms, Maximum = 34ms, Average = 24ms
```

#### SRV1 (10.10.10.20)
```
C:\Windows\System32>ping 10.10.10.20

Pinging 10.10.10.20 with 32 bytes of data:
Reply from 10.10.10.20: bytes=32 time=28ms TTL=63
Reply from 10.10.10.20: bytes=32 time=23ms TTL=63
Reply from 10.10.10.20: bytes=32 time=18ms TTL=63
Reply from 10.10.10.20: bytes=32 time=19ms TTL=63

Ping statistics for 10.10.10.20:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 18ms, Maximum = 28ms, Average = 22ms
```

```md
All devices responded with 0% packet loss, confirming full Layer 3 reachability from the PRTG server.
```

### SNMP Configuration

All network devices must have SNMP configured before onboarding.

Standard configuration used:

- SNMP version: v2c
- Community string: NOCMONRO
- Access restricted via ACL

### Example: Cisco Device SNMP Configuration

Configuration applied on R1 and DSW1:
```cisco
snmp-server community NOCMONRO RO
snmp-server location LAB
snmp-server contact NOC

access-list 10 permit 192.168.61.1
snmp-server community NOCMONRO RO 10
```

---

## 3. Device Onboarding in PRTG

Devices were added manually into PRTG using the web interface.

### Steps Performed:

1. Log in to PRTG Web Console
2. Navigate to "Devices"
3. Click "Add Device"
4. Configure:
   - Device Name (R1, DSW1, SRV1, PC1)
   - IP Address
5. Assign to appropriate group (e.g., "Lab Network")

---

## 4. Credential Configuration

SNMP credentials were configured in PRTG:

- SNMP Version: v2c
- Community String: NOCMONRO

Credentials were applied at:
- Device level or inherited from parent group

---

## 5. Validation

After adding devices, connectivity was verified.

### ICMP Test
- Devices responded successfully to ping

### SNMP Test
- SNMP sensors were able to retrieve data

Successful onboarding indicators:
- Device shows "Up" status in PRTG
- No SNMP authentication errors

---

## 6. Troubleshooting

### Issue: Device not reachable

Possible causes:
- Incorrect IP address
- Missing route
- Wrong default gateway

Verification:
- Ping test from PRTG server
- Check routing table on R1

---

### Issue: SNMP not working

Possible causes:
- Incorrect community string
- ACL blocking PRTG server
- SNMP not enabled on device

Verification:
- Check SNMP configuration on device
- Confirm ACL permits PRTG IP
- Use packet capture (optional)

---

### Issue: Device shows Down in PRTG

Possible causes:
- ICMP blocked
- Device powered off
- Network misconfiguration

---

## 7. NOC Relevance

In a real NOC environment, onboarding devices involves:

- Verifying reachability
- Ensuring monitoring protocols (ICMP/SNMP) are working
- Validating data collection before relying on alerts

Failure in onboarding leads to:
- False alerts
- Missing incidents
- Reduced visibility
