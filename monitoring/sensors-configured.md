# Sensors Configured

## 1. Objective

This section documents the sensors configured in PRTG to monitor device availability and performance.

Sensors are responsible for collecting specific metrics from each device and triggering alerts when thresholds are exceeded.

---

## 2. Sensor Strategy

The following monitoring approach was used:

- Ping / Ping v2 → for availability and response time monitoring
- SNMP → for system uptime and interface traffic monitoring
- Ping v2 provides additional latency and packet-loss visibility compared to the standard Ping sensor.

Each device was assigned multiple sensors depending on its role.

---

## 3. Configured Sensors per Device

### R1 (Router)

Configured sensors:
- Ping Sensor
- Ping v2 Sensor
- SNMP System Uptime
- SNMP Traffic (TO_DSW1)

Purpose:
- Detect router availability
- Measure response time (latency)
- Monitor uptime (detect reboot)
- Observe traffic levels on the router-to-switch path

### DSW1 (Switch)

Configured sensors:
- Ping Sensor
- Ping v2 Sensor
- SNMP System Uptime

Purpose:
- Ensure switch is reachable
- Measure response time
- Detect device restarts

### PC1 (Client)

Configured sensors:
- Ping Sensor

Purpose:
- Test endpoint connectivity
- Used for reachability validation

### SRV1 (Server)

Configured sensors:
- Ping Sensor

Purpose:
- Validate server availability
- Used as incident simulation target

---

## 4. Key Monitoring Metrics

The following metrics are being tracked:

- Availability (Up/Down)
- Response time (latency)
- System uptime
- Interface traffic utilization

These metrics provide baseline visibility required for Level 1 NOC monitoring.

---

## 5. Threshold Considerations

PRTG uses predefined thresholds to determine sensor status:

- Up (green): normal operation
- Warning (yellow): threshold exceeded
- Down (red): device unreachable or critical issue

Example conditions:
- High response time may indicate network congestion
- Device down triggers immediate alert

---

## 6. Sensor Validation

After configuration, sensors were validated to ensure proper operation.

Validation steps:
- Confirm sensors show "Up" status
- Verify data is being collected (e.g., uptime increasing)
- Check response time values are consistent with ping tests

Successful validation confirms monitoring is functioning correctly.

All configured sensors are in "Up" state, confirming successful data collection.

---

## 7. NOC Relevance

In a real NOC environment:

- Sensors are the primary source of alerts
- Each sensor represents a specific monitored metric
- Incidents are triggered when sensor thresholds are breached

Proper sensor selection ensures:
- Accurate alerting
- Reduced false positives
- Faster troubleshooting

See evidence: [R1 sensors](https://github.com/japalbornoz/NOC-Mini-Project-01-Monitoring-Incident-Workflow/blob/main/screenshots/prtg-sensors-r1.png)

This setup reflects typical L1 monitoring responsibilities.
