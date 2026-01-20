# Log Ingestion and Data Flow

## Purpose of Log Ingestion in the Lab

Effective detection and investigation depend on **reliable and consistent log ingestion**.  
This section documents how logs are collected, forwarded, and centralized in the SIEM, ensuring end-to-end visibility across the lab.

The goal is to demonstrate:
- Multiple log ingestion methods
- Clear data flow from source to SIEM
- Realistic SOC-style telemetry collection
- Transparency in how data reaches the analyst

---

## Overview of Log Sources

The lab ingests logs from three main categories:

### Network Devices
- pfSense firewall (network and connection logs)

### Network Security Tooling
- Suricata IDS alerts (EVE JSON format)

### Endpoints / Hosts
- Linux system logs
- Authentication logs (e.g. SSH activity)

Each source provides a different layer of visibility and context.

---

## Log Ingestion Architecture

The log ingestion pipeline follows this general flow:

1. **Log generation** on hosts or network devices
2. **Log forwarding** using appropriate mechanisms
3. **Reception and indexing** by Splunk
4. **Search, correlation, and visualization** by the analyst

All logs converge in Splunk Enterprise running on `srv-ids`.

---

## Host-Based Log Ingestion

### Splunk Universal Forwarder

The **Splunk Universal Forwarder** is installed on monitored Linux hosts.

Its role is to:
- Monitor specific log files
- Forward events securely to the SIEM
- Minimize performance impact on endpoints

#### Example log types forwarded:
- Authentication logs (`auth.log`)
- System logs (`syslog`)
- Other security-relevant system events

Forwarders send data to Splunk over TCP, ensuring reliable delivery.

---

## Network Device Log Ingestion (pfSense)

pfSense logs are forwarded using **syslog**.

Characteristics:
- Centralized logging of firewall decisions
- Connection tracking visibility
- Network-level context for security events

Syslog ingestion allows pfSense activity to be correlated with:
- IDS alerts
- Host authentication failures
- Dashboard timelines

---

## IDS Log Ingestion (Suricata)

Suricata generates alerts in **EVE JSON format**, which is well-suited for SIEM ingestion.

Key properties of this format:
- Structured fields (IPs, ports, protocols)
- Alert metadata (signature, category, severity)
- Precise timestamps

These logs are ingested directly into Splunk, preserving structure and enabling efficient searches and dashboards.

---

## Time Synchronization and Consistency

Accurate correlation requires consistent timestamps.

The lab ensures:
- System clocks are aligned across hosts
- Events from different sources can be reliably correlated
- Timelines reflect real sequence of activity

Minor discrepancies due to virtualization are acknowledged and documented as part of the lab’s limitations.

---

## Indexing and Source Identification

Logs are logically separated using:
- Indexes to group similar data sources
- Source types to identify log formats

This approach allows:
- Cleaner searches
- More efficient dashboards
- Reduced ambiguity during investigations

Indexes reflect the **purpose of the data**, not just the host that generated it.


---

## Analyst Perspective

From a SOC analyst’s point of view, the ingestion pipeline enables:

- Fast access to raw events
- Correlation between network and host activity
- Reconstruction of attack timelines
- Validation of detection logic

The ingestion design prioritizes **clarity and traceability** over automation.

---

## Design Decisions and Trade-Offs

Several decisions were made intentionally:

- No heavy preprocessing to avoid hiding anomalies
- Minimal enrichment to preserve raw signal
- Acceptance of some background noise
- Focus on analysis rather than perfect ingestion pipelines

These choices reflect real-world SOC environments where analysts must work with imperfect data.

---

## Summary

The log ingestion design provides:

- End-to-end visibility from network to endpoint
- Reliable and structured data flow into the SIEM
- A realistic foundation for detection and investigation
- Clear traceability from source to analysis

This ingestion pipeline enables the dashboards and detection scenarios documented in later sections of the project.
