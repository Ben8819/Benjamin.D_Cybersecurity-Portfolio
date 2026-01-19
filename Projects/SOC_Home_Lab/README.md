# SOC Home Lab – Detection, Monitoring & Incident Investigation

## 🎯 Project Objective

The goal of this SOC Home Lab is to design, deploy, and operate a realistic defensive security environment that mirrors the responsibilities of a junior SOC analyst.

This project focuses on:
- Network visibility and segmentation
- Log collection and centralization
- IDS/IPS deployment and tuning
- Alert triage and investigation
- Practical attack simulation and detection
- Documentation and analyst-style reasoning

  
Rather than relying on isolated tools, this lab emphasizes **end-to-end visibility**, from packet capture to alert investigation and response.
<br><br>


 ✅ What This Project Demonstrates

This SOC Home Lab demonstrates:
- Practical defensive security skills
- Understanding of SOC workflows
- Ability to design and operate a monitored network
- Comfort with logs, alerts, and investigation
- Clear documentation and structured thinking
<br>

## 📁 Project Structure Overview
<br>

### 📄 Root Files
- Read Me — Project overview, goals <--- You are here
- [How to Navigate this project](How_To_Navigate_This_Project.md) — Step-by-step guide to reading this repository

---

### 🧱 Lab Architecture
Network topology, VM roles, segmentation, and design decisions.

This lab is built around a segmented virtual network in GNS3 protected by pfSense and monitored using Suricata and Splunk.

Key design principles:
- Clear separation between user, server, management, and monitoring networks
- Centralized logging and analysis
- Dedicated IDS sensor for traffic inspection
- Realistic attacker and victim simulation

The full architecture, network diagrams, and IP plans are documented in: [Lab architecture/](Lab_Architecture)

---

### 🔥 Firewall and Network Security
pfSense configuration, firewall rules, network isolation, and traffic control.

Network traffic is controlled and segmented using **pfSense** as the perimeter and internal firewall.

Implemented concepts include:
- Interface-based segmentation (LAN, SERVER_LAN, MANAGEMENT)
- Explicit allow/deny rules per network role
- Logging of permitted and blocked traffic
- IDS/IPS integration points

Details are documented in: [Firewall and Network security/](Firewall_And_Network_Security)

---

### 🕵️ Detection and Monitoring
IDS deployment, detection strategy, log generation, and monitoring design.

Detection was implemented across the lab, from raw network traffic to actionable security alerts.

Key topics covered include:
- IDS deployment and architecture choices
- Suricata configuration and tuning
- Definition of monitored networks (HOME_NET / EXTERNAL_NET)
- Alert classification (signal vs noise)
- Log generation and enrichment for SIEM ingestion
- Monitoring strategy aligned with SOC use cases

Detection is intentionally designed as a **layered process**:
- pfSense provides network-level visibility and enforcement
- Suricata inspects traffic for suspicious and malicious patterns
- Logs are forwarded to Splunk for correlation, visualization, and investigation

This section focuses on **how and why detection decisions were made**, not just tool installation.  
It highlights the challenges of alert noise, rule tuning, and the trade-offs between inline IPS and passive IDS monitoring.

Documented in: Detection and Monitoring/

---

### 📊 Dashboards
SOC dashboards built to visualize authentication activity, alerts, and network threats.

Splunk is used as the central SIEM platform to:
- Aggregate logs from pfSense, Suricata, and servers
- Visualize security events and trends
- Support investigation workflows

Custom dashboards include:
- Global SOC overview
- IDS signal vs noise analysis
- Authentication activity monitoring
- Suspicious activity timelines

Dashboards are documented with explanations and screenshots in: Dashboards/ 

---

### 🎯 Attack Simulation & Detection
Simulated attacker activity and corresponding detection and alerting.

To validate the detection pipeline, controlled attack simulations were conducted from an attacker machine.

Examples include:
- Network scanning (Nmap)
- SSH Brute-Force (Hydra)

These actions triggered IDS alerts that were:
- Logged
- Correlated
- Investigated using Splunk and pfSense

This section demonstrates the ability to **generate, detect, and interpret security events**, not just deploy tools.

Documented in: Attack Simulation & Detection/ 

---

### 🧠 SOC Investigation Workflow
Incident triage, investigation methodology, and analysis workflows.

This project includes a structured investigation workflow inspired by real SOC processes.

Covered topics:
- Alert validation
- False positive vs true positive analysis
- Context enrichment (source, destination, service, time)
- Timeline reconstruction
- Decision-making and escalation logic

Rather than isolated alerts, the focus is on **analyst reasoning** and repeatable investigation steps.

Documented in: SOC Investigation Workflow/ 

---

### 🔐 Hardening
Security improvements applied after detection and investigation phases.

Security hardening was applied progressively as the lab evolved.

Examples include:
- Firewall rule tightening
- Reduction of exposed services
- IDS rule tuning and suppression
- Separation of monitoring and management planes
- Limiting unnecessary traffic and noise

This reflects a realistic environment where security improves iteratively, not perfectly from day one.

Documented in: Lab Hardening/ 

---

### 🧪 Side Experiments
Additional tests and exploratory configurations performed during the project.

Some tools and configurations were explored outside the core SOC workflow to deepen understanding, including:
- Snort testing and custom rules
- Manual packet inspection
- Configuration experiments and edge cases

These experiments are intentionally separated from the main lab to keep the core project clean and focused.

Documented in: Side experiment/

---

### 📚 Lessons Learned and Limitations
Reflections, trade-offs, technical challenges, and improvement ideas.

No lab is perfect, and this one is no exception.

This section documents:
- Technical limitations encountered
- Design trade-offs
- Tool constraints
- What would be improved in a production environment
- What would be done differently with more resources or time

This section is intentional and reflects professional self-awareness.

Documented in: Lesson learned and limitation/

---
