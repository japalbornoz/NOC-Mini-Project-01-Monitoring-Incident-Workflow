# Troubleshooting Runbook (NOC Mini-Project 01)

This runbook is used to triage and resolve common monitoring and connectivity issues in the lab environment.

## Scope
- Monitoring platform: **PRTG**
- Transport: **ICMP (Ping)**, **SNMP**
- Lab segments:
  - Monitoring/Lab LAN: `10.10.10.0/24`
  - Gateway: `R1 Fa0/0 = 10.10.10.1`
  - DSW1 mgmt: `10.10.10.2`
  - PC1: `10.10.10.10`
  - SRV1 (dnsmasq): `10.10.10.20`

---

## Standard Triage Workflow (Follow This Order)

### 1) Confirm the alert context (PRTG)
- What sensor is down? (Ping vs SNMP vs Traffic)
- Is it one device or multiple?
- When did it start? (timestamp)
- Is it flapping? (up/down frequently)

### 2) Scope the impact
- **Single host** issue (one device)?
- **Subnet** issue (multiple devices in `10.10.10.0/24`)?
- **Monitoring system** issue (PRTG host can’t reach anything)?

### 3) Validate Layer 3 reachability (fastest checks)
From a test host (PC1 / monitoring host):
- Ping gateway: `ping 10.10.10.1`
- Ping DSW1: `ping 10.10.10.2`
- Ping target device IP

### 4) Validate Layer 2 (if ping fails)
- ARP table on host (or router)
- MAC address table on switch
- Interface status / duplex mismatch logs

### 5) Fix the simplest likely cause
- Interface down / wrong VLAN / cabling
- SNMP ACL / community mismatch
- Device CPU/RAM resource saturation
- Tooling/platform instability

### 6) Verify recovery
- Ping stable
- SNMP returns data
- PRTG sensor returns to Up
- Document evidence (screenshots + command outputs)

---

## Scenario 01 — Ping Sensor Down (Device Unreachable)

### Symptom (PRTG)
- Ping sensor for `R1` or `DSW1` shows **Down**
- Packet loss 100% or timeout

### First checks (60 seconds)
1. Confirm if *only one* device is down or multiple.
2. Ping the gateway:
   - If **gateway reachable**, problem may be host/device-specific.
   - If **gateway not reachable**, problem is likely L2/L3 at the access layer.

### Isolation steps
#### A) If gateway is NOT reachable
- Check if PC1 has correct config:
  - IP in `10.10.10.0/24`
  - gateway `10.10.10.1`
- Check ARP resolution:
  - Does PC1 learn the MAC for `10.10.10.1`?
- Check switchport status to PC1 and to R1.

#### B) If gateway IS reachable, but target is NOT
- Ping another device in same subnet (ex: DSW1).
  - If others respond → target device is likely down.
  - If others fail → subnet/VLAN issue.

### Likely causes
- Target device powered off/down (lab node stopped)
- Wrong interface/VLAN mapping on DSW1
- Link down between DSW1 ↔ R1
- Duplex mismatch causing intermittent loss

### Fix actions
- Confirm device/node is started (GNS3 node state)
- Verify DSW1 access VLAN assignment for relevant ports
- Ensure R1 interface is `up/up` and has correct IP
- If duplex mismatch logs exist, force duplex to full on both ends

### Verification
- Stable ping (repeat 5–10 pings)
- PRTG Ping sensor returns to **Up**
- Attach evidence:
  - PRTG sensor screenshot
  - `show ip int brief` from R1
  - `show int status` from DSW1

### Escalation criteria
Escalate (or move to deeper troubleshooting) if:
- Multiple devices drop simultaneously
- Links flap (Up/Down frequently)
- CPU/RAM is saturated or platform unstable

---

## Scenario 02 — SNMP Sensor Down / “No Data” (Ping Works)

### Symptom (PRTG)
- Device responds to Ping
- SNMP sensors show **Down** or **No response**
- SNMP Traffic/Uptime not updating

### First checks (60 seconds)
1. Confirm Ping works from PRTG host to device IP.
2. Confirm SNMP sensor target IP is correct.
3. Confirm SNMP version/community configured in PRTG.

### Isolation steps (router-side)
On the monitored device (R1/DSW1):
- Confirm SNMP is enabled and community is correct
- Check SNMP ACL (if applied):
  - Ensure PRTG host IP is permitted

### Likely causes
- Wrong community string or SNMP version mismatch
- SNMP ACL blocks PRTG host
- Device lacks required MIB support (common in emulation images)
- Firewall between PRTG and device blocks UDP/161 (less likely in lab)

### Fix actions
- Match PRTG SNMP settings to device config
- Update SNMP ACL to allow PRTG host
- Recreate SNMP sensor if it was created against wrong interface/IP
- If the image does not support some SNMP OIDs, switch to supported sensors (Ping/Uptime/Traffic)

### Verification
- SNMP uptime returns a value
- SNMP traffic shows RX/TX
- PRTG sensor changes to **Up**
- Attach evidence:
  - PRTG SNMP sensor screenshot
  - Relevant device SNMP config snippet
  - ACL snippet if used

### Escalation criteria
Escalate if:
- Ping works but SNMP fails across all devices (PRTG-side issue)
- Only one device fails (device config/image limitation)

---

## Scenario 03 — High Packet Loss / Intermittent Ping (Flapping)

### Symptom (PRTG)
- Ping sensor shows **Warning** (loss/latency)
- “Up/Down” flapping alerts
- User symptoms: slow/unreliable connectivity

### First checks (60 seconds)
- Run 20 pings from PC1 to gateway and to target device.
- Identify if loss is:
  - only to one device
  - or across multiple devices

### Isolation steps
- Check duplex mismatch logs (CDP warnings)
- Check interface counters/errors
- Verify switchport VLAN assignment is consistent
- Confirm no duplicate IP in subnet

### Likely causes
- Duplex mismatch (very common symptom)
- ARP warm-up / emulation startup behavior (initial ping failures)
- Host resource constraints (CPU/RAM pegged)
- Link instability (node restarting)

### Fix actions
- Force duplex full on both ends (R1 + DSW1 port)
- Restart affected node if emulation is unstable
- Reduce running nodes if host RAM is near capacity
- Re-test after stabilization period (1–2 minutes)

### Verification
- Loss drops to 0%
- PRTG returns to normal latency
- Attach evidence:
  - PRTG graphs before/after
  - `show cdp neighbors detail` (if applicable)
  - interface status outputs

---

## Scenario 04 — DNS Resolution Failure (Name fails, IP works)

### Symptom
- `ping 10.10.10.20` works (DNS server reachable)
- `ping www.branchlab.com` fails (cannot resolve)

### First checks
- Confirm client DNS setting points to `10.10.10.20`
- Confirm SRV1 is running and has correct IP/gateway

### Likely causes
- Client DNS not set
- DNS record missing/typo
- SRV1 container not started / wrong IP config

### Fix actions
- Set DNS on client to SRV1 IP
- Add/verify dnsmasq address record for the hostname
- Restart SRV1 after config changes

### Verification
- Name resolves and ping reaches the expected IP
- Attach evidence:
  - SRV1 DNS config snippet / start command
  - client DNS setting screenshot/output

---

## Scenario 05 — “Everything is Down” (Monitoring System Issue)

### Symptom (PRTG)
- Multiple unrelated devices show Down at the same time
- PRTG host itself may be degraded

### First checks
- Confirm PRTG host network connectivity (gateway, local subnet)
- Confirm no host firewall changes
- Confirm CPU/RAM on host system

### Likely causes
- Host resource constraints (RAM high, CPU pegged)
- GNS3 controller/VM not connected
- VM networking misconfigured (NAT/host-only mismatch)
- Security software blocking local connections

### Fix actions
- Reduce load (stop nodes, close heavy apps)
- Ensure GNS3 VM is green/connected
- Confirm local server port (avoid port 80 conflicts; prefer 3080)
- Restart GNS3 VM + GNS3 GUI if controller disconnects

### Verification
- Devices return Up as connectivity is restored
- Attach evidence:
  - GNS3 VM status (green)
  - system resource screenshot (optional)
  - PRTG device tree alarm clearing

---

## Ticket Evidence Checklist (What to attach)
- PRTG screenshot of sensor state (Down/Warning/Up)
- Timestamp of detection + resolution
- Command outputs (if relevant):
  - R1: `show ip int brief`, `show arp`
  - DSW1: `show int status`, `show vlan brief`, `show mac address-table`
- Any config changes made (snippets only)
- Final verification ping results

---

## Notes (Lab Emulation Reality)
- Initial pings may fail due to ARP/neighbor warm-up.
- Some SNMP sensors may not return data depending on image limitations.
- High host RAM usage can cause controller/VM disconnects and false “outages”.
