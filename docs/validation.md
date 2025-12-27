# Validation Plan & Results (Sanitized)

Use this as a Blue-Team style test sheet. Add screenshots in `/docs/capturas/` and reference them below.

> Legend: PASS / FAIL / N/A

## Network Segmentation Tests

| ID | Test | Source → Destination | Expected | Result | Evidence |
|---:|------|----------------------|----------|--------|----------|
| 1 | Guest cannot reach Admin | Guest VLAN → Admin VLAN | Blocked |  |  |
| 2 | Guest cannot reach IoT | Guest VLAN → IoT VLAN | Blocked |  |  |
| 3 | IoT cannot reach Admin (default) | IoT VLAN → Admin VLAN | Blocked |  |  |
| 4 | Admin can reach pfSense mgmt | Admin VLAN → pfSense | Allowed |  |  |
| 5 | Admin can reach Home Assistant UI | Admin VLAN → HA (8123/tcp) | Allowed |  |  |

## Controlled Exceptions (Least Privilege)

| ID | Test | Source → Destination | Expected | Result | Evidence |
|---:|------|----------------------|----------|--------|----------|
| 6 | IoT to MQTT broker | IoT VLAN → MQTT (1883/8883) | Allowed |  |  |
| 7 | IoT to DNS | IoT VLAN → DNS | Allowed |  |  |
| 8 | Guest to DNS | Guest VLAN → DNS | Allowed |  |  |
| 9 | Guest to Internet (HTTP/HTTPS) | Guest VLAN → Internet | Allowed |  |  |
| 10 | Guest blocked to RFC1918 | Guest VLAN → 10.0.0.0/8 etc. | Blocked |  |  |

## MQTT & Automation Validation

| ID | Test | Steps | Expected | Result | Evidence |
|---:|------|-------|----------|--------|----------|
| 11 | MQTT pub/sub | Publish test topic, subscribe from client | Message received |  |  |
| 12 | HA receives MQTT | Publish sensor payload to HA topic | Sensor updates |  |  |
| 13 | HA automation trigger | Trigger rule from MQTT event | Action executed |  |  |

## Security Observations (SOC Notes)
- Record any blocked inter-VLAN attempts observed in pfSense logs.
- Note any unexpected egress from IoT VLAN.
- Document rule changes made during troubleshooting (what/why).

