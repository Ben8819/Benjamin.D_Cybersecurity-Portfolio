# 👁️ Intrusion Detection System (Suricata)

## Purpose of Suricata in the Lab

Suricata is deployed as the **Network Intrusion Detection System (NIDS)** for this lab.  
Its role is to provide **network-level visibility**, detect suspicious traffic patterns, and generate alerts that can be analyzed and correlated in the SIEM.

Suricata was chosen because it is:
- Widely used in SOC environments
- Open-source and transparent
- Capable of deep packet inspection
- Compatible with structured JSON output for SIEM ingestion

The IDS is used strictly for **detection and analysis**, not prevention.

---

## IDS Mode: Detection Only (Passive)

Suricata operates in **IDS (passive) mode**, meaning:

- Traffic is **observed**, not blocked
- No inline packet modification occurs
- All alerts are forwarded to Splunk for analysis
- The firewall (pfSense) remains responsible for enforcement

This design reflects real-world SOC environments where:
- IDS provides visibility
- Decisions are made by analysts
- Prevention is handled separately (firewall, EDR, IPS)

---

## Traffic Visibility and Monitoring Method

Suricata monitors network traffic using a **mirrored / SPAN-like setup**.

Key characteristics:
- Traffic is duplicated for inspection
- Suricata does not interfere with packet flow
- Both internal and outbound traffic can be observed
- The IDS has visibility across multiple security zones

This approach allows detection of:
- Reconnaissance activity
- Lateral movement attempts
- Suspicious connection patterns
- Abnormal protocol behavior

---

## Alert Generation and Logging

Suricata generates alerts based on:
- Signature-based detection rules
- Protocol anomalies
- Traffic behavior patterns

Alerts are written in **EVE JSON format**, which includes:
- Timestamp
- Source and destination IPs
- Ports and protocols
- Alert signature and severity
- Metadata useful for correlation

These logs are forwarded directly to Splunk for centralized analysis.

---

## Noise and False Positives

In a virtualized lab environment, some background noise is expected.

Common sources of noise include:
- TCP retransmissions
- Asymmetric traffic visibility
- Virtual NIC offloading behavior
- SPAN/mirrored traffic artifacts

Rather than attempting to eliminate all noise at the sensor level, this project treats noise as a **normal operational reality** and addresses it through:

- Alert classification in Splunk
- Separation of benign sensor artifacts from suspicious activity
- Documentation of known limitations

This mirrors real SOC workflows, where analysts must distinguish signal from noise.

---

## Sensor Self-Traffic Considerations

Because the IDS operates within the monitored environment, special care was taken to avoid excessive self-generated alerts.

Steps taken include:
- Avoiding inline inspection
- Limiting unnecessary management traffic exposure
- Applying capture-level filtering where possible

Residual noise was documented and handled analytically rather than hidden.

---

## Rule Set and Detection Scope

The default Suricata rule set was used as a baseline to detect:

- Network scans
- Suspicious connection patterns
- Protocol misuse
- Known attack signatures
- Test IDS signatures (e.g. validation traffic)

The focus of the lab is **detection validation**, not exhaustive rule tuning.

---

## Integration with SIEM (Splunk)

Suricata alerts are ingested into Splunk as structured events.

This enables:
- Correlation with firewall logs
- Correlation with authentication events
- Timeline reconstruction
- Dashboard-based monitoring

Alert severity is treated as contextual information, not absolute truth, and is interpreted alongside behavioral patterns.

---

## Security Trade-Offs and Design Decisions

Several trade-offs were made intentionally:

- IDS used instead of IPS to preserve visibility
- Minimal rule tuning to avoid hiding detection behavior
- Noise handled analytically rather than aggressively suppressed
- Emphasis on analyst interpretation over automated blocking

These decisions reflect realistic SOC constraints and priorities.

---

## Summary

Suricata provides the lab with:

- Network-wide detection capability
- Realistic alerting behavior
- Structured data suitable for SIEM analysis
- A foundation for SOC-style investigation workflows

The IDS component is intentionally designed to emphasize **visibility, analysis, and decision-making**, rather than perfect prevention.
