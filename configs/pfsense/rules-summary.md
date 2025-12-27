# pfSense Rules Summary (Sanitized)

This is a **high-level** firewall policy summary for the lab. It documents intent and control objectives without exporting the full pfSense configuration (which may contain sensitive data).

## Segments
- **ADMIN VLAN**: management + Home Assistant
- **IOT VLAN**: IoT devices + MQTT broker
- **GUEST VLAN**: Internet-only guests

## Policy Principles
- **Default deny** inter-VLAN traffic.
- **Least privilege**: only allow required ports/flows.
- **No lateral movement** from Guest/IoT into Admin.
- Prefer **stateful rules** with clear logging on denied/critical paths.

---

## Baseline Rules (Recommended)

### ADMIN VLAN (higher trust)
**Allow**
- Admin → pfSense management (HTTPS/SSH as used)
- Admin → Home Assistant (8123/tcp if applicable)
- Admin → MQTT broker (1883/tcp or 8883/tcp if TLS) when required
- Admin → DNS/NTP (to designated resolver/time source)
- Admin → Internet (HTTP/HTTPS) as needed

**Block**
- Admin → Guest (unless explicitly required)

### IOT VLAN (untrusted / constrained)
**Allow**
- IoT → MQTT broker (1883/tcp or 8883/tcp)
- IoT → DNS (to pfSense or designated resolver)
- IoT → NTP (to designated time source)
- IoT → Home Assistant **only if required** (typically specific ports, specific destination)

**Block**
- IoT → ADMIN VLAN (any) by default
- IoT → RFC1918 private networks (except explicit destinations above)

### GUEST VLAN (lowest trust)
**Allow**
- Guest → Internet (HTTP/HTTPS)
- Guest → DNS (to pfSense or designated resolver)

**Block**
- Guest → ADMIN VLAN (any)
- Guest → IOT VLAN (any)
- Guest → RFC1918 private networks (any)

---

## Logging Guidance (Blue Team)
- Enable logging on:
  - Default deny rules (inter-VLAN)
  - Any rule that is high-risk (e.g., IoT → HA exceptions)
- Review logs for:
  - Repeated blocked attempts from IoT/Guest to Admin
  - Unusual egress destinations from IoT VLAN
  - Port scanning patterns

## Notes
Replace VLAN IDs, subnets, and IPs in this file with your lab’s values if you want, but avoid publishing identifiers that could be tied to real environments.
