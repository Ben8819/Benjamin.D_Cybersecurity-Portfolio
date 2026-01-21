# Lab Hardening Strategy

This document describes the hardening measures applied to the SOC home lab environment.  
The goal was not to create an “unbreakable” system, but to apply **realistic defensive controls** aligned with entry-level SOC and blue team practices.

---

## 1. Network Segmentation

The lab is segmented into distinct security zones to reduce lateral movement and isolate critical components:

- **WORKSTATION_LAN**
  - Admin workstation
- **KALI_LAN**
  - Attack simulation host (Kali)
- **SERVER_LAN**
  - Application servers
  - Log-producing systems
- **IDS_MONITORING**
  - Dedicated traffic monitoring path
  - No routing or outbound access

Each segment has explicitly defined trust boundaries enforced by pfSense.

**Security Benefit:**  
Limits blast radius and allows targeted monitoring and alerting.

---

## 2. Firewall Hardening (pfSense)

Firewall rules follow a **default-deny** philosophy:

- Explicit allow rules per interface
- No `any → any` rules in final state
- Inter-LAN traffic restricted to required services only
- Management access limited to admin workstation subnet

Additional measures:
- ICMP restricted between zones
- Logging enabled on sensitive rules
- Stateful filtering used to prevent spoofed traffic

**Security Benefit:**  
Reduces unnecessary exposure and provides visibility into policy violations.

---

## 3. IDS Placement and Hardening

Suricata was deployed on a **dedicated IDS VM** rather than inline on pfSense.

Key design decisions:
- Passive inspection only (no packet modification)
- Traffic duplicated via pfSense bridge
- Separate monitoring and management interfaces on the IDS host

Suricata configuration hardening:
- Precise `HOME_NET` definition
- Rule sets limited to relevant protocols
- Excessive noise (retransmissions, stream warnings) filtered post-ingestion

**Security Benefit:**  
Prevents IDS instability from impacting network availability while preserving detection fidelity.

---

## 4. Host Hardening

### Linux Servers
- SSH key-based authentication
- Password authentication disabled where possible
- Services limited to required ports only
- UFW disabled in favor of centralized firewall enforcement

### IDS Host
- No exposed services on monitoring interface
- Management access restricted to SERVER_LAN
- Minimal installed packages

**Security Benefit:**  
Reduces attack surface and centralizes security control.

---

## 5. Identity & Access Management

- Unique user accounts per system
- No shared credentials
- Privileged access limited to administrative tasks
- `sudo` usage logged by default

Authentication activity is monitored centrally via SIEM dashboards.

**Security Benefit:**  
Improves traceability and supports incident investigation.

---

## 6. Logging & Monitoring Hardening

Logs forwarded to Splunk include:
- Authentication events
- Firewall activity
- IDS alerts
- System logs from critical hosts

Hardening at the SIEM level:
- Dedicated indexes per log source
- Dashboards focused on signal, not volume
- Noise reduction applied before alert interpretation

**Security Benefit:**  
Prevents analyst fatigue and improves detection quality.

---

## 7. Attack Simulation Controls

Offensive tools (Nmap, Hydra, test traffic) were:
- Executed from controlled hosts only
- Time-bounded
- Correlated with detection dashboards

No uncontrolled or persistent attack tooling was left running.

**Security Benefit:**  
Keeps the lab realistic while avoiding self-inflicted instability.

---

## 8. Known Limitations & Trade-offs

This lab intentionally accepts some constraints:

- No hardware-based SPAN switch
- Limited scale compared to enterprise SOCs
- No SOAR or automated response

These limitations were mitigated through architectural choices rather than ignored.

**Security Benefit:**  
Demonstrates awareness of real-world constraints and compensating controls.

---

## Conclusion

The hardening strategy prioritizes:
- Visibility over complexity
- Separation of duties
- Operational safety
- SOC-relevant decision-making

This lab was designed not just to generate alerts, but to **support realistic detection, investigation, and learning workflows**.
