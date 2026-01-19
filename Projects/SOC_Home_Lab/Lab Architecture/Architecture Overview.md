# Architecture Overview

### Purpose of the Lab

This project is a self-built cybersecurity lab designed to demonstrate core Security Operations Center (SOC) skills, including:

- Network segmentation
- Firewall policy design
- Intrusion detection
- Log centralization
- Alert analysis
- Attack detection and validation

The lab focuses on **defensive security**, realistic trade-offs, and analyst workflows rather than offensive tooling.

The environment was intentionally designed to resemble a small enterprise network, with clear separation between user workstations, servers, security tooling, and attacker infrastructure.

---

### High-Level Architecture

The lab is built around the following core components:

- **pfSense** as the perimeter firewall, router, and segmentation point
- **Suricata** as a Network Intrusion Detection System (NIDS)
- **Splunk Enterprise** as the SIEM platform
- **Splunk Universal Forwarders** on monitored hosts
- Multiple internal network segments with different trust levels
- An isolated attacker network for controlled attack simulation

All internal traffic is routed through pfSense, allowing centralized visibility and control.

A full network topology diagram is provided in this directory for reference.

--> Topology_diagram

---

### Network Segmentation Design

The environment is segmented into multiple security zones, each with a specific role and trust level.

#### 1. Management / User LAN (LAN)

- **Subnet:** `192.168.1.0/24`
- **Primary host:** `admin-ws` (`192.168.1.10`)
- **Role:** Administrative workstation and primary user endpoint

This network represents a standard internal user environment.

The `admin-ws` host is used for:
- Administrative access (SSH / web management)
- Log generation (authentication events)
- Interaction with Splunk dashboards
- Controlled testing of IDS alerts (e.g. testmyids)

Access to management interfaces across the lab is intentionally restricted to this host.

--

#### 2. Server / Security LAN (SERVER_LAN)

- **Subnet:** `10.0.3.0/24`
- **Primary host:** `srv-ids`(`10.0.3.20`)
- **Role:** Security tooling and log aggregation

This network hosts:
- **Suricata** (IDS sensor)
- **Splunk Enterprise** (SIEM)
- Centralized log ingestion and alert analysis

This segment is treated as a protected infrastructure zone.
Only authorized management traffic is allowed from the administrative workstation.

--

#### 3. Attacker LAN (KALI_LAN)

- **Subnet:** `10.0.2.0/24`
- **Primary host:** `kali-attck`
- **Role:** Simulated internal attacker

This network represents an untrusted or compromised internal segment.

It is intentionally **isolated** from the rest of the lab by default, with no unrestricted access to internal resources.

Temporary access can be granted for controlled attack simulations and detection validation.

---

### Firewall and Routing Core

**pfSense** acts as:

- Default gateway for all internal networks
- Central firewall enforcing segmentation policies
- NAT gateway for outbound traffic
- Traffic inspection and logging point

All inter-LAN communication is explicitly controlled through pfSense firewall rules, following a **default-deny** philosophy with minimal required access.

---

### Traffic Monitoring and Visibility

Traffic visibility is achieved through a mirrored / SPAN-like setup, allowing Suricata to inspect network traffic without actively interfering with packet flow.

Suricata operates in **IDS (passive) mode**, generating alerts without blocking traffic.

This design choice reflects real SOC environments where detection and analysis are prioritized over inline prevention.

---

### Logging and SIEM Integration

Logging and monitoring are centralized using Splunk:

- System and authentication logs are forwarded from monitored hosts using **Splunk Universal Forwarders**
- Network-based alerts from Suricata are ingested via structured JSON logs
- Logs are centralized in **Splunk Enterprise**, where:
  - Noise is classified
  - Alerts are correlated
  - Dashboards provide operational visibility

This enables both **network-based** and **host-based** detection capabilities.

---

### Design Philosophy

The lab prioritizes:

- Realistic security trade-offs
- Clear separation of trust zones
- Defensive detection workflows
- Noise awareness and classification
- SOC-relevant analysis rather than perfect signal

Some background IDS noise is expected in a virtualized environment and is treated as a documented limitation rather than a configuration error.

---

### Summary

This architecture provides a solid foundation for demonstrating:

- Network security design
- IDS deployment
- SIEM ingestion and analysis
- Alert triage and classification
- Attack detection and response reasoning

Subsequent sections of this portfolio detail the firewall implementation, detection tooling, dashboards, and simulated attack scenarios built on top of this architecture.
