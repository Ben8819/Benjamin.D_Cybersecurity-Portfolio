# How to Navigate This Project

This project documents a hands-on SOC and network security lab designed to demonstrate practical defensive security skills, not just tool usage.

If you are a recruiter or hiring manager, this short guide explains how to review the project efficiently.

---

### 1. Start Here – Project Context

Begin with the [**README**](README.md) at the root of the project folder.

It provides:
- A high-level overview of the lab goals
- The motivation behind the project
- The overall architecture and security approach

This sets the context for everything that follows.

---

### 2. Understand the Architecture

Next, review the architecture-related documentation:

- [**Architecture Overview**](Lab_Architecture/Architecture_Overview.md)
  - Network segmentation
  - pfSense firewall placement
  - Dedicated IDS and SIEM components
- [**pfSense Design**](Firewall_And_Network_Security/Pfsense_Design_And_Network_Security.md) and [**Firewall Rules**](Firewall_And_Network_Security/Firewall_Rules_Design.md)
  - Firewall role
  - Interface separation
  - Management vs monitored traffic

These sections explain *how traffic flows* and *where visibility is enforced*.

---

### 3. Review the Security Controls

The core defensive components are documented in dedicated sections:

- [**Suricata IDS**](Detection_And_Monitoring/Intrusion_Detection_System_Suricata.md)
  - Deployment evolution of Suricata, [configuration deep dive](Detection_And_Monitoring/Suricata_IDS_Configuration_Deep_Dive.md)
  - [Log Ingestion and Data Flow](Detection_And_Monitoring/Log_Ingestion_And_Data_Flow.md)
  - Rule strategy and noise reduction
- [**Splunk**](Detection_And_Monitoring/Security_Information_And_Event_Management_Splunk.md)
  - Deployment evolution of Splunk
- [**Lab Hardening**](Lab_Hardening/Lab_Hardening_Strategy.md)
  - Network segmentation
  - Firewall restrictions
  - Host-level hardening
  - Operational security choices

This shows how detection was combined with prevention and risk reduction.

---

### 4. Explore the SIEM & Dashboards

The dashboards represent how a SOC analyst would *consume* security data:

- [**Authentication Monitoring Dashboard**](Dashboards/Authentication_Monitoring_Dashboard.md)
  - Login activity
  - Failure patterns
  - Suspicious authentication behavior
- [**SOC Overview Dashboard**](Dashboards/SOC_Overview_Dashboard.md)
  - High-level visibility across all data sources
  - Firewall, IDS, and host log correlation
- [**IDS / Network Threats Dashboard**](Dashboards/IDS_Network_Threat_Dashboard.md)
  - Suricata alerts
  - Noise vs suspicious classification
  - Focus on actionable signals

Screenshots are referenced directly in each dashboard document for visual context.

---

### 5. Incident Investigation & Scenarios

To demonstrate analysis skills beyond dashboards, review:

- [**Attack Simulation and Detection**](Attack_Simulation_And_Detection/Attack_Simulation_And_Detection.md)
  - Controlled scans and attack-like activity
  - How alerts were generated and observed
- [**SOC Investigation Workflow**](SOC_Investigation_Workflow/SOC_Investigation_Workflow.md)
  - Triage methodology
  - Log correlation
  - Noise handling
  - Decision-making process

Together, these files show how alerts are *interpreted*, not just generated.

---

### 6. Hands-On Technical Skills

For evidence of practical, command-line experience, see:

- **CLI Reference**
  - Frequently used Linux, networking, IDS, and SIEM commands
  - Commands selected to reflect real SOC workflows
  - No screenshots required — focused on operational knowledge

This demonstrates comfort working directly on systems without relying solely on GUIs.

---

### 7. What This Project Demonstrates

This lab intentionally focuses on:

- Defensive security thinking
- SOC-oriented workflows
- Realistic trade-offs and limitations
- Clear documentation and reasoning

It is not designed to showcase exploitation, but rather:
- Detection
- Investigation
- Monitoring
- Security architecture decisions

---

### 8. How to Review Efficiently (Summary)

If you are short on time, the recommended order is:

1. Main README (project overview)
2. SOC Overview Dashboard
3. Authentication Dashboard
4. IDS / Network Threats Dashboard
5. SOC Investigation Workflow
6. Lab Hardening Strategy

This provides a complete picture in under 15 minutes.

---

### Final Note

This project reflects an iterative learning process similar to real-world environments:
- Initial design decisions
- Encountered limitations
- Architectural adjustments
- Improved detection quality over time

The focus is on **how problems are approached and solved**, not on claiming perfection.
